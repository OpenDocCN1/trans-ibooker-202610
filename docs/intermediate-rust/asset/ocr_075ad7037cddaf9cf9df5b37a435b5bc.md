your client futures are Send, it can run them in parallel since it can hold &muts to the separate tasks. Listing 9-16 gives an example of what this might look like.

```
async fn server(socket: TcpListener) -> Result<()> {
    while let Some(stream) = socket.accept().await? {
        // Spawn a new task with the Future that represents this client.
        // The current task will continue to just poll for more connections,
        // and will run concurrently (and possibly in parallel) with handle_client.
        spawn(handle_client(stream));
    }
}
```

Listing 9-16: Spawning futures to create more tasks that can be polled concurrently

When you spawn a future, and thus make it a task, it’s sort of like spawning a thread. The future continues running in the background and is multiplexed concurrently with any other tasks given to the executor. However, unlike a spawned thread, spawned tasks still depend on being polled by the executor. If the executor stops running, either because you drop it or because your code no longer runs the executor’s code, those spawned tasks will stop making progress. In the server example, imagine what will happen if the main server future resolves for some reason. Since the executor has returned control back to your code, it cannot continue doing, well, anything. Multi-threaded executors often spawn background threads that continue to poll tasks even if the executor yields control back to the user’s code, but not all executors do this, so check your executor before you rely on that behavior!

# BLOCKING IN ASYNC CODE

You must be careful about calling synchronous code from asynchronous code, since any time an executor thread spends executing the current task is time it’s not spending running other tasks. If a task occupies the current thread for a prolonged period of time without yielding back to the executor, which might happen when executing a blocking system call (like std::sync::sleep) or running in a tight loop with no awaits, then other tasks the current executor thread is responsible for won’t get to run during that time. Usually, this manifests as long delays between when certain tasks can make progress (such as when a client connects) and when they actually get to execute.

Some multithreaded executors implement work-stealing techniques, where idle executor threads steal tasks from busy executor threads, but this is more of a mitigation than a solution. Ultimately, you could end up in a situation where all the executor threads are blocked, and thus no tasks get run until one of the blocking operations completes.

In general, you should be very careful with executing compute-intensive operations or calling functions that could block in an asynchronous context.

continued