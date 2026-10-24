In this example, the initial request comes in on the server's default thread pool. That request sets up a call to execute the business logic in a separate thread pool (called the async thread pool), and then the sleepAsyncEndpoint() method immediately returns. That frees the thread from the default thread pool so it can immediately handle another request. Meanwhile, the async response (annotated with the @Suspended tag) is waiting for the logic to complete; when that happens, it is resumed with the response to be sent back to the user.

This allows us to run 64 (or whatever parameter we pass to the thread pool) simultaneous requests before the requests start to back up. But frankly, we haven't achieved anything different from resizing the default thread pool to 64. In fact, in this case, our response will be slightly worse, since the request gets sent to a different thread for processing, which will take a few milliseconds.

There are three reasons you would use an async response:

+   - To introduce more parallelism into the business logic. Imagine that instead of sleeping for 100 ms, our code had to make three (unrelated) JDBC calls to obtain data needed for the response. Using an async response allows the code to process each call in parallel, with each JDBC call using a separate thread in the async thread pool.
+   - To limit the number of active threads.
+   - To properly throttle the server.

In most REST servers, if we just throttle the request thread pool, new requests will wait their turn, and the queue for the thread pool will grow. Often, this queue is unbounded (or at least has a very large bound) so that the total number of requests ends up being unmanageable. Requests that spend a long time in a thread pool queue will often be abandoned by the time they are processed, and even if they are not abandoned, the long response times are going to kill the total throughput of the system.

A better approach is to look at the async thread pool status before queueing the response, and rejecting the request if the system is too busy.

```
@GET
@Path("/asyncreject")
@Produces(MediaType.APPLICATION_JSON)
public void sleepAsyncRejectEndpoint(
    @DefaultValue("100") @QueryParam("delay") long delay,
    @Suspended final AsyncResponse ar
) throws ParseException {
    if (tpe.getActiveCount() == 64) {
        ar.cancel();
        return;
    }
}
```