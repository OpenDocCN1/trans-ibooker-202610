```
let id = thread::current().id();
println!("This is my thread id: {id:?}");
}
```

We spawn two threads that will both execute f as their main function. Both of these threads will print a message and show their thread id, while the main thread will also print its own message.

# THREAD ID

The Rust standard library assigns every thread a unique identifier. This identifier is accessible through Thread::id() and is of the type ThreadId. There’s not much you can do with a ThreadId other than copying it around and checking for equality. There is no guarantee that these IDs will be assigned consecutively; only that they will be different for each thread.

If you run our example program above several times, you might notice the output varies between runs. This is the output I got on my machine during one particular run:

```
Hello from the main thread.
Hello from another thread!
This is my thread id:
```

Surprisingly, part of the output seems to be missing.

What happened here, is that the main thread finished executing the main function before the newly spawned threads finished executing their function.

Returning from main will exit the entire program, even if other threads are still running.

In this particular example, one of the newly spawned threads had just enough time to get to halfway through the second message, before the program was shut down by the main thread.