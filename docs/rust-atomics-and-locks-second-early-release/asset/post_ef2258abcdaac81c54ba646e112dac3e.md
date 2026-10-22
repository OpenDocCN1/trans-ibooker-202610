
Protecting data with a mutex is simply the agreement between all threads that they will only access the data when they have the mutex locked. That way, no two threads can ever access that data concurrently and cause a data race.

## Rust’s Mutex

The Rust standard library provides this functionality through `std::sync::Mutex<T>`. It is a generic type over a type T, where T is the type of the data the mutex is protecting mutual exclusive access to. By making this T part of the mutex, the data can only be accessed through the mutex, allowing for a safe interface that can guarantee all threads will uphold the agreement.

To ensure a locked mutex can only be unlocked by the thread that locked it, it does not have an `unlock()` method. Instead, its `lock()` method returns a special type called a `MutexGuard`. This guard represents the guarantee that we have locked the mutex. It behaves like an exclusive reference through the `DerefMut` trait, giving us exclusive access to the data the mutex protects. Unlocking the mutex is done by dropping the guard. When we drop the guard, we give up our ability to access the data, and the `Drop` implementation of the guard will unlock the mutex.

Let’s take a look at an example to see a mutex in practice:

```
fn main() {
    let n = Mutex::new(0);
    thread::scope(|s| {
        for _ in 0..10 {
            s.spawn(|| {
                let mut guard = n.lock().unwrap();
                for _ in 0..100 {
                    *guard += 1;
                }
            });
        }
    });
    assert_eq!(n.into_inner().unwrap(), 1000);
}
```

Here, we have a Mutex<i32>, a mutex protecting an integer, and spawn ten threads to each increment the integer one hundred times. Each thread will first lock the mutex to obtain a mutex guard through lock(), and then use that guard to access the integer and modify it. The guard is implicitly dropped right after, when that variable goes out of scope.

After the threads are done, we can safely remove the protection from the integer through into_inner(). The into_inner method takes an exclusive reference to the mutex, which guarantees nothing else can have a reference to the mutex anymore, making locking unnecessary.

Even though the increments happen in steps of one, a thread observing the integer would only ever see multiples of 100, since it can only look at the integer when the mutex is unlocked. Effectively, thanks to the mutex, the one hundred increments together are now a single undividable, atomic, operation.

To clearly see the effect of the mutex, we can make each thread wait a second before unlocking the mutex:

```
..    s.spawn(|| {
        let mut guard = n.lock().unwrap();
        for _ in 0..100 {
            *guard += 1;
        }
        thread::sleep(Duration::from_secs(1)); // Sleep
before unlocking the mutex.
    });
..
```

When you run the program now, you will see it will take about ten seconds to complete. Each thread only waits for one second, but the mutex will ensure only one thread at a time can do so.

If we drop the guard, and therefore unlock the mutex, before sleeping one second, we will see it happen in parallel instead.

```
..    s.spawn(|| {
```

```
let mut guard = n.lock().unwrap();
for _ in 0..100 {
    *guard += 1;
}
drop(guard); // Drop the guard before sleeping
thread::sleep(Duration::from_secs(1));
});
```

one second.
..

With this change, this program will only take about one second, since now the ten threads can execute their one-second sleep at the same time. This shows the importance of keeping the amount of time a mutex is locked as short as possible. Keeping a mutex locked longer than necessary can completely nullify any benefits of parallelism, effectively forcing everything to happen in series instead.

## Lock Poisoning

The unwrap() calls in the examples above relate to lock poisoning.

A Mutex in Rust gets marked as poisoned when a thread panics while it was holding the lock. The lock method returns an Err if the lock was poisoned.

This is a mechanism to protect against leaving the data that’s protected by a mutex in an inconsistent state. If in our example above a thread would panic after incrementing the integer fewer than one hundred times, the mutex unlocks and the integer is left in an unexpected state where it is not a multiple of 100 anymore, possibly breaking assumptions made by other threads. Automatically marking the mutex as poisoned in that case forces the user to handle this possibility.

Calling lock() on a poisoned mutex still locks the mutex. The Err returned by lock() contains the mutex guard, allowing us to correct an inconsistent state or simply ignore the poison.

While lock poisoning might seem like a good idea, it is not often used in practice. Most code either ignores poison, or uses unwrap() to panic if

the lock was poisoned, effectively propagating panics to all users of the mutex.

## LIFETIME OF THE MUTEX GUARD

While it’s convenient and important for correctness that implicitly dropping a guard unlocks the mutex, it can sometimes lead to subtle surprises. If we assign the guard a name with a let statement (as in our examples above), it’s relatively straight forward to see when it will be dropped, since local variables are dropped at the end of the scope they are defined in. Still, not explicitly dropping a guard might lead to keeping the mutex locked for longer than necessary, as demonstrated in the examples above.

Using a guard without assigning it a name is also possible, and can be very convenient at times. Since a MutexGuard behaves like an exclusive reference to the protected data, we can directly use it without assigning a name to the guard first. For example, if you have a Mutex<Vec<i32>>, you can lock the mutex, push an item into the Vec, and unlock the mutex again, in a single statement:

```
list.lock().unwrap().push(1);
```

Any temporaries produced within a larger expression, such as the guard returned by lock(), will be dropped at the end of the statement. While this might seem obvious and reasonable, it leads to a common pitfall that usually involves a match, if let or while let statement. Here is an example that runs into this pitfall:

```
if let Some(item) = list.lock().unwrap().pop() {
    process_item(item);
}
```

If our intention was to lock the list, pop an item, unlock the list, and then process the item after the list is unlocked, we made a subtle but important mistake here. The temporary guard is not dropped until the end of the entire if let statement, meaning we needlessly hold on to the lock while processing the item.

Perhaps surprisingly, this does not happen for a similar if statement, such as in this example:

```
if list.lock().unwrap().pop() == Some(1) {
    do_something();
}
```

Here, the temporary guard does get dropped before the body of the if statement is executed. The reason is that the condition of a regular if statement is always a plain boolean, which cannot borrow anything. There is no reason to extend the lifetime of temporaries from the condition to the end of the statement. For an if let statement, however, that might not be the case. If we had used front() rather than pop() for example, item would be borrowing from the list, making it necessary to keep the guard around. Since the borrow checker is only really a check and does not influence when or in what order things are dropped, the same happens when we use pop(), even though that wouldn’t have been necessary.

We can avoid this by moving the pop operation to a separate let statement. Then the guard is dropped at the end of that statement, before the if let:

```
let item = list.lock().unwrap().pop();
if let Some(item) = item {
    process_item(item);
}
```

## Reader-Writer Lock

A mutex is only concerned with exclusive access. The MutexGuard will provide us an exclusive reference (&mut T) to the protected data, even if we only wanted to look at the data and a shared reference (&T) would have sufficed.
