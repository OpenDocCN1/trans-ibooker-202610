# MUTEXES IN OTHER LANGUAGES

Rust standard library’s Mutex and RwLock look a bit different than those you find in other languages like C or C++.

The biggest difference is that Rust’s Mutex<T> contains the data it is protecting. In C++, for example, std::mutex does not contain the data it protects, nor does it even know what it is protecting at all. This means that it is the responsibility of the user to remember which data is protected and by which mutex, and ensure the right mutex is locked every time protected data is accessed. This is useful to keep in mind when reading code involving mutexes in other languages, or when communicating with programmers who are not familiar with Rust. A Rust programmer might talk about “the data inside the mutex”, or say things like “wrap it in a mutex”, which can be confusing to those only familiar with mutexes in other languages.

If you really need a stand-alone mutex that doesn’t contain anything, for example to protect some external hardware, you can use Mutex<()>. But even in those cases, you are probably better off defining a (possibly zero-sized) type to interface with that hardware and wrapping that in a Mutex instead. That way, you are still forced to lock the mutex before you can interact with the hardware.

# Waiting: Parking and Condition Variables

When data is mutated by multiple threads, there are many situations where they would need to wait for some event; for some condition about the data to be come true. For example, if we have a mutex protecting a Vec, we might want to wait until it contains any elements.

While a mutex does allow threads to wait until it becomes unlocked, it does not provide functionality for waiting for any other conditions. If a mutex