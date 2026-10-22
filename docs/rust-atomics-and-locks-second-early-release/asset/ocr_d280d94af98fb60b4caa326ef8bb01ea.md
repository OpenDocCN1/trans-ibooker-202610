# THREAD BUILDER

The `thread::spawn` function is actually just a convenient shorthand for `thread::Builder::new().spawn().unwrap()`.

A `thread::Builder` allows you to set some settings for the new thread before spawning it. You can use it to configure the stack size for the new thread and to give the new thread a name. The name of a thread is available through `thread::current().name()`, will be used in panic messages, and will be visible in monitoring and debugging tools on most platforms.

Additionally, Builder’s `spawn` function returns an `io::Result`, allowing you to handle situations where spawning a new thread fails. This might happen if the operating system runs out of memory, or if resource limits have been applied to your program. The `thread::spawn` function simply panics if it is unable to spawn a new thread.

# Scoped Threads

# WARNING

As of June 2022, scoped threads are not yet part of the Rust standard library. It has been proposed and implemented as unstable feature, which is available using #![feature(scoped_threads)] on nightly versions of Rust.

If we know for sure that a spawned thread will definitely not outlive a certain scope, that thread could safey borrow things that do not live forever, such as local variables, as long as they outlive that scope.

The Rust standard library provides the `thread::scope` function to spawn such scoped threads. It allows us to spawn threads that cannot