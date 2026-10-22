
## Atomic Loads and Stores

The first two atomic operations we’ll look at are the most basic ones: load and store. Their function signatures are as follows:

```
impl AtomicI32 {
    pub fn load(&self, ordering: Ordering) -> i32;
    pub fn store(&self, value: i32, ordering: Ordering);
}
```

The load method atomically loads the value stored in the atomic variable, and the store method atomically stores a new value in it. Note how the store method takes a shared reference (&T) rather than an exclusive reference (&mut T), even though it modifies the value.

Let’s take a look at some realistic use cases for these two methods.

## Example: Stop Flag

The first example uses an AtomicBool for a stop flag. Such a flag is used to inform other threads to stop running.

```
fn main() {
    static STOP: AtomicBool = AtomicBool::new(false);

    // Spawn a thread to do the work.
    let background_thread = thread::spawn(|| {
        while !STOP.load(Relaxed) {
            some_work();
        }
    });

    // Use the main thread to listen for user input.
    for line in std::io::stdin().lines() {
        match line.unwrap().as_str() {
            "help" => println!("commands: help, stop"),
            "stop" => break,
            cmd => println!("unknown command: {cmd:?}"),
        }
    }

    // Inform the background thread it needs to stop.
    STOP.store(true, Relaxed);

    // Wait until the background thread finishes.
    background_thread.join().unwrap();
}
```

In this example, the background thread is repeatedly running some_work(), while the main thread allows the user to enter some commands to interact with the program. In this simple example, the only useful command is stop to make the program stop.

To make the background thread stop, the atomic STOP boolean is used to communicate this condition to the background thread. When the foreground thread reads the stop command, it sets the flag to true, which is checked by the background thread before each new iteration. The main thread waits until the background thread is finished with its current iteration using the join method.

This simple solution works great as long as the flag is regularly checked by the background thread. If it can get stuck in some_work() for a long time, this might result in an unacceptable delay between the stop command and the program quitting.

## Example: Progress Reporting

In our next example we process 100 items one by one on a background thread, while the main thread gives the user regular updates on the progress.

```
fn main() {
    let num_done = AtomicUsize::new(0);

    thread::scope(|s| {
        // A background thread to process all 100 items.
        s.spawn(|| {
            for i in 0..100 {
                process_item(i);
                num_done.store(i + 1, Relaxed);
            }
        });

        // The main thread shows status updates, every second.
        loop {
            let n = num_done.load(Relaxed);
            if n == 100 { break; }
            println!("Working.. {n}/100 done");
            thread::sleep(Duration::from_secs(1));
        }
    });

    println!("Done!");
}
```

This time, we use a scoped thread (“Scoped Threads”), which will automatically handle the joining of the thread for us, and also allow us to borrow local variables.

Every time the background thread finishes processing an item, it stores the number of processed items in an AtomicUsize. Meanwhile, the main thread shows that number to the user to inform them of the progress, about once per second. Once the main thread sees all 100 items have been processed, it exits the scope, which implicitly joins the background thread, and informs the user everything is done.

## Synchronization

Once the last item is processed, it might take up to one whole second for the main thread to know that, introducing an unnecessary delay at the end. To solve this, we can use thread parking (“Thread parking”) to wake the main thread from its sleep whenever there is new information it might be interested in.

Here’s the same example, but now using thread::park_timeout rather than thread::sleep:

```
fn main() {
    let num_done = AtomicUsize::new(0);

    let main_thread = thread::current();

    thread::scope(|s| {
        // A background thread to process all 100 items.
        s.spawn(|| {
            for i in 0..100 {
                process_item(i);
                num_done.store(i + 1, Relaxed);
                main_thread.unpark(); // Wake up the main thread.
            }
        });

        // The main thread shows status updates.
        loop {
            let n = num_done.load(Relaxed);
            if n == 100 { break; }
            println!("Working.. {n}/100 done");
            thread::park_timeout(Duration::from_secs(1));
        }
    });

    println!("Done!");
}
```

Not much has changed. We’ve obtained a handle to the main thread through thread::current(), that’s now used by the background thread to unpark the main thread after every status update. The main thread now uses park_timeout rather than sleep, such that it can be interrupted.

Now, any status updates are immediately reported to the user, while still repeating the last update every second to show the program is still running.

## Example: Lazy Initialization

The last example before we move on to more advanced atomic operations is about lazy initialization.

Imagine there is a value x which we are reading from a file, obtaining from the operating system, or calculating in some other way, that we expect to be constant. Maybe x is the version of the operating system, or the total amount of memory, or the 400th digit of tau. It doesn’t really matter for this example.

Since we don’t expect it to change, we can request or calculate it only the first time we need it, and remember the result. The first thread that needs it will have to calculate the value, but it can store it in an atomic static to make it available for all threads, including itself if it needs it again later.

Let’s take a look at an example of this. To keep things simple, we’ll assume x is never zero, such that we can use zero as a placeholder before it has been calculated.

```
fn get_x() -> u64 {
    static X: AtomicU64 = AtomicU64::new(0);
    let mut x = X.load(Relaxed);
    if x == 0 {
        x = calculate_x();
        X.store(x, Relaxed);
    }
    x
}
```

The first thread to call get_x() will check the static X and see it is still zero, calculate its value, and store the result back in the static to make it available for future use. Later, any call to get_x() will see the value in the static is nonzero, and return it immediately without calculating it again.

However, if a second thread calls get_x() while the first one is still calculating x, the second thread will also see a zero and also calculate x in parallel. One of the threads will end up overwriting the result of the other, depending on which one finishes first. This is called a race. Not a data race, which is undefined behaviour and impossible in Rust without using unsafe, but still a race with an unpredictable winner.

Since we expect x to be constant, it doesn’t matter who wins the race, as the result will be the same regardless. Depending on how much time we expect calculate_x() to take, this might be a very good or very bad strategy.

If calculate_x() is expected to take a long time, it’s better if threads wait while the first thread is still initializing x. You could implement this using a condition variable or thread parking (“Waiting: Parking and Condition Variables”), but that quickly gets too complicated for a small example. The Rust standard library provides exactly this functionality through std::sync::Once and std::lazy::SyncOnceCell, so there’s usually no need to implement these yourself.

## Fetch-and-Modify Operations

Now that we’ve seen a few use cases for the basic load and store operations, let’s move on to more interesting operations: the fetch and modify operations. These operations modify the atomic variable, but also load (fetch) the original value, as a single atomic operation.

The most commonly used ones are fetch_add and fetch_sub, which perform addition and subtraction respectively. Some of the other available operations are fetch_or and fetch_and for bitwise operations, and fetch_max and fetch_min which can be used to keep a running maximum or minimum.

Their function signatures are as follows:

```
impl AtomicI32 {
    pub fn fetch_add(&self, v: i32, ordering: Ordering) -> i32;
    pub fn fetch_sub(&self, v: i32, ordering: Ordering) -> i32;
    pub fn fetch_or(&self, v: i32, ordering: Ordering) -> i32;
    pub fn fetch_and(&self, v: i32, ordering: Ordering) -> i32;
    pub fn fetch_nand(&self, v: i32, ordering: Ordering) -> i32;
    pub fn fetch_xor(&self, v: i32, ordering: Ordering) -> i32;
    pub fn fetch_max(&self, v: i32, ordering: Ordering) -> i32;
    pub fn fetch_min(&self, v: i32, ordering: Ordering) -> i32;
    pub fn swap(&self, v: i32, ordering: Ordering) -> i32; // "fetch_store"
}
```

The one outlier is the operation that simply stores a new value, regardless of the old value. Instead of fetch_store, it has been called swap.

Here’s a quick demonstration showing how fetch_add returns the value before the operation:

```
let a = AtomicU32::new(100);
let b = a.fetch_add(23, Relaxed);
```
