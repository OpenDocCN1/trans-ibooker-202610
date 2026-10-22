

behavior.

## Cell

A Cell<T> simply wraps a T, but allows mutations through a shared reference. To avoid undefined behavior, it only allows you to copy the value out (if T is Copy), or replace it with another value as a whole. In addition, it can only be used within a single thread.

Let’s take a look at similar example as in the previous section, but this time using Cell<i32> instead of i32:

```
fn f(a: &Cell<i32>, b: &Cell<i32>) {
    let before = a.get();
    b.set(b.get() + 1);
    let after = a.get();
    if before != after {
        x(); // might happen
    }
}
```

Unlike last time, it is now possible for the if condition to be true. Because a Cell<i32> has interior mutability, the compiler can no longer assume its value won’t change as long as we have a shared reference to it. Both a and b might refer to the same value, such that mutating through b might affect a as well. It may still assume, however, that no other threads are accessing the cells concurrently.

The restrictions on a Cell are not always easy to work with. Since it can’t directly let us borrow the value it holds, we need to move a value out (leaving something in its place), modify it, then put it back, to mutate its contents.

```
fn f(v: &Cell<Vec<i32>>) {
    let mut v2 = v.take(); // Replaces the contents of the Cell with an empty Vec
    v2.push(1);
    v.set(v2); // Put the modified Vec back
}
```

## RefCell

Unlike a regular Cell, a RefCell does allow you to borrow its contents, at a small run-time cost. A RefCell<T> does not only hold a T, but also holds a counter that keeps track of any outstanding borrows. If you try to borrow it while it is already mutably borrowed, it will panic, which avoids undefined behavior. Just like a Cell, a RefCell can only be used within a single thread.

```
fn f(v: &RefCell<Vec<i32>>) {
    v.borrow_mut().push(1); // We can modify the `Vec` directly.
}
```

While Cell and RefCell can be very useful, they become rather useless when we need to do something with multiple threads. So let’s move on to the types that are relevant for concurrency.

## Mutex and RwLock

An RwLock or reader-writer lock is the concurrent version of a RefCell. An RwLock<T> holds a T, and tracks any outstanding borrows. However, unlike a RefCell, it does not panic on conflicting borrows. Instead, it blocks the current thread—putting it to sleep—while waiting for conflicting borrows to disappear. We’ll just have to patiently wait for our turn with the data, after the other threads are done with it.

Borrowing the contents of an RwLock is called locking. By locking it we temporarily block concurrent conflicting borrows, allowing us to borrow it without causing data races.

A Mutex (short for mutual exclusion) is very similar, but conceptually slightly simpler. Instead of keeping track of the number of shared and exclusive borrows like an RwLock, it only allows exclusive borrows.

We’ll go more into detail on these types in “Locking: Mutexes and RwLocks”.

## Atomics

The atomic types represent the concurrent version of a Cell, and are the main topic of Chapter 2 and X REF HERE FOR CH03. Like a Cell, they avoid undefined behavior by making us copy values in and out as a whole, without letting us borrow the contents directly.

Unlike a Cell though, they cannot be of arbitrary size. Because of this, there is no generic Atomic<T> type for any T, but there are only specific atomic types such as AtomicU32 and AtomicPtr. Which ones are available depends on the platform, since they require support from the processor to avoid data races. (We’ll dive into that in X REF HERE FOR CH04.)

Since they are so limited in size, atomics often don’t directly contain the information that needs to be shared between threads. Instead, they are often used as a tool to make it possible to share other—often bigger—things between threads. When atomics are used to say something about other data, things can get surprisingly complicated.

## UnsafeCell

An UnsafeCell is the primitive building block for interior mutability.

An UnsafeCell<T> wraps a T, but does not come with any conditions or restrictions to avoid undefined behavior. Instead, its get method just gives a raw pointer to the value it wraps, which can only be meaningfully used in unsafe blocks. It leaves it up to the user to use it in a way that does cause any undefined behavior.

Most commonly, an UnsafeCell is not used directly, but wrapped in another type that provides safety through a limited interface, such as Cell or Mutex. All types with interior mutability—including all types discussed above—are built on top of UnsafeCell.

## Thread Safety: Send and Sync

In this chapter, we’ve seen several types that are not *thread safe*, types that can only be used on a single thread, such as `Rc`, `Cell`, and others. Since that restriction is needed to avoid undefined behavior, it’s something the compiler needs to understand and check for you, so you can use these types without having to use `unsafe` blocks.

The language uses two special traits to keep track of which types can be safely used across threads:

### Send

A type is `Send` if it can be sent to another thread. That is, if ownership of a value of that type can be transferred to another thread. For example, `Arc<i32>` is `Send`, but `Rc<i32>` is not.

### Sync

A type is `Sync` if it can be shared with another thread. In other words, a type T is `Sync` if and only if a shared reference to that type, `&T`, is `Send`. For example, a `i32` is `Sync`, but a `Cell<i32>` is not.

All primitive types such as `i32`, `bool`, and `str` are all both `Send` and `Sync`.

Both of these traits are *auto traits*, which means that they are automatically implemented for your types based on their fields. A `struct` with fields that are all `Send` and `Sync`, is itself also `Send` and `Sync`.

The way to opt-out of either of these is to add a field to your type that does not implement the trait. For that purpose, the special `PhantomData<T>` type often comes in handy. That type is treated by the compiler as a T, except it doesn’t actually exist at runtime. It’s a zero sized type, taking no space.

Let’s take a look at this struct:

```
struct X {
    handle: i32,
```

```
_not_sync: PhantomData<Cell<()>>,
}
```

In this example, the struct X would be both Send and Sync if handle was its only field. However, we added a zero-sized PhantomData<Cell<()>> field, which is treated as if it were a Cell<()>. Since a Cell<()> is not Sync, neither is X. It is still Send, however, since all its fields implement Send.

Raw pointers (*const T and *mut T) are neither Send nor Sync, since the compiler doesn’t know much about what they represent.

The way to opt-in to either of the traits is the same as with any other trait: an impl block to implement the trait for you type:

```
struct X {
    p: *mut i32,
}

unsafe impl Send for X {}
unsafe impl Sync for X {}
```

Note how implementing these traits requires the unsafe keyword, since the compiler cannot check for you if it’s correct. It’s a promise you make to the compiler, which it will just have to trust.

If you try to move something into another thread which is not Send, the compiler will politely stop you from doing that. Here is a small example to demonstrate that:

```
fn main() {
    let a = Rc::new(123);
    thread::spawn(move || {
        dbg!(a);
    });
}
```

Here, we try to send an Rc<i32> to a new thread, but Rc<i32>, unlike Arc<i32>, does not implement Send.

If we try to compile this, we’re faced with the following error:

```
error[E0277]: `Rc<i32>` cannot be sent between threads safely
    --> src/main.rs:3:5
     |
3   |         thread::spawn(move || {
     |         ^^^^^^^^^^^^^ `Rc<i32>` cannot be sent between
threads safely
     |
     = help: within `[closure@src/main.rs:3:19: 5:6]`, the trait
`Send` is not implemented for `Rc<i32>`
     = note: required because it appears within the type
`[closure@src/main.rs:5:19: 5:6]`
note: required by a bound in `spawn`
```

The thread::spawn function requires its argument to be Send, and a closure is only Send if all of its captures are. If we try to capture something that’s not Send our mistake is caught, protecting us from undefined behavior.

## Locking: Mutexes and RwLocks

The most commonly used tool for sharing (mutable) data between threads is called a mutex, which is short for "mutual exclusion". The job of a mutex is to ensure threads have exclusive access to some data by temporarily blocking other threads that try to access it at the same time.

Conceptually, a mutex has only two operations: lock and unlock, and two states: locked and unlocked. When a thread locks an unlocked mutex, the mutex is marked as locked and the thread can immediately continue. When a thread then attempts to lock an already locked mutex, that operation will block. The thread is put to sleep while it waits for the mutex to be unlocked. Unlocking is only possible on a locked mutex, and should be done by the same thread that locked it. If other threads are waiting to lock the mutex, unlocking will cause one of those threads to be woken up, so it can try to lock the mutex again and continue its course.

