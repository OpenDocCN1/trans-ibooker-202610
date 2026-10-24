
```java
public String sleepEndpoint(
    @DefaultValue("100") @QueryParam("delay") long delay
) throws ParseException {
    try { Thread.sleep(delay); } catch (InterruptedException ie) {}
    return "{\"sleepTime\": \"" + delay + "\"}";
}
```

The point of the sleep in this example is just for testing: assume that the sleep is making a remote database call or calling another REST server, and that remote call takes 100 ms. If I run that test on a Helidon server with default configuration, it will handle 32 simultaneous requests. A load generator with a concurrency of 32 will report that each request takes 100 ms (plus 1–2 ms for processing). A load generator with a concurrency of 64 will report that each request takes 200 ms, since each request will have to wait for another request to finish before it can start processing.

Other servers will have a different configuration, but the effect is the same: there will be some throttle based on the size of the request thread pool. Often that is a good thing: if the 100 ms is spent as active CPU time (instead of sleeping) in this example, then the server won’t really be able to handle 32 requests simultaneously unless it is running on a very large machine.

In this case, though, the machine is not even close to being CPU-bound; it may take only 20%–30% of a single core to process the load when there is no processing to be done (and again, the same amount to process the load if those 100 ms time intervals are just a remote call to another service). So we can increase the concurrency on this machine by changing the configuration of the default thread pool to run more calls. The limit here would be based on the concurrency of the remote systems; we still want to throttle the calls into those systems so that they are not overwhelmed.

JAX-RS provides a second way to increase the concurrency, and that is by utilizing an asynchronous response. The asynchronous response allows us to defer the business logic processing to a different thread pool:

```java
ThreadPoolExecutor tpe = Executors.newFixedThreadPool(64);
@GET
@Path("/asyncsleep")
@Produces(MediaType.APPLICATION_JSON)
public void sleepAsyncEndpoint(
    @DefaultValue("100") @QueryParam("delay") long delay,
    @Suspended final AsyncResponse ar
) throws ParseException {
    tpe.execute(() -> {
        try { Thread.sleep(delay); } catch (InterruptedException ie) {}
        ar.resume("{\"sleepTime\": \"" + delay + "\"}");
    });
}
```

In this example, the initial request comes in on the server's default thread pool. That request sets up a call to execute the business logic in a separate thread pool (called the async thread pool), and then the sleepAsyncEndpoint() method immediately returns. That frees the thread from the default thread pool so it can immediately handle another request. Meanwhile, the async response (annotated with the @Suspended tag) is waiting for the logic to complete; when that happens, it is resumed with the response to be sent back to the user.

This allows us to run 64 (or whatever parameter we pass to the thread pool) simultaneous requests before the requests start to back up. But frankly, we haven't achieved anything different from resizing the default thread pool to 64. In fact, in this case, our response will be slightly worse, since the request gets sent to a different thread for processing, which will take a few milliseconds.

There are three reasons you would use an async response:

- To introduce more parallelism into the business logic. Imagine that instead of sleeping for 100 ms, our code had to make three (unrelated) JDBC calls to obtain data needed for the response. Using an async response allows the code to process each call in parallel, with each JDBC call using a separate thread in the async thread pool.
- To limit the number of active threads.
- To properly throttle the server.

In most REST servers, if we just throttle the request thread pool, new requests will wait their turn, and the queue for the thread pool will grow. Often, this queue is unbounded (or at least has a very large bound) so that the total number of requests ends up being unmanageable. Requests that spend a long time in a thread pool queue will often be abandoned by the time they are processed, and even if they are not abandoned, the long response times are going to kill the total throughput of the system.

A better approach is to look at the async thread pool status before queueing the response, and rejecting the request if the system is too busy.

```java
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

```java
tpe.execute(() -> {
    // Simulate processing delay using sleep
    try { Thread.sleep(delay); } catch (InterruptedException ie) {}
    ar.resume("{\"sleepTime\": \"" + delay + "\"}");
});
```

That can be accomplished in many ways, but for this simple example, we’ll look at the active count running in the pool. If the count is equal to the pool size, the response is immediately canceled. (A more sophisticated example would set up a bounded queue for the pool and cancel the request in the thread pool’s rejected execution handler.) The effect here is that the caller will immediately receive an HTTP 503 Service Unavailable status, indicating that the request cannot be processed at this time. That is the preferred way to handle an overloaded server in the REST world, and immediately returning that status will reduce the load on our overloaded server, which in the end will lead to much better overall performance.

![](img/1711f142ae3cfd740ef106afbbba2046_329_0.png)

## Quick Summary

- Nonblocking I/O using Java’s NIO APIs allows servers to scale by reducing the number of threads required to handle multiple clients.
- This technique means that a server will need one or more thread pools to handle the client requests. This pool should be tuned based on the maximum number of simultaneous requests the server should handle.
- A few extra threads are then needed for handling selectors (whether as part of the worker thread pool or a separate thread pool depending on the server framework).
- Server frameworks often have a mechanism to defer long requests to a different thread pool, which offers more robust handling of requests on the main thread pool.

## Asynchronous Outbound Calls

The preceding section gave the example of a server with two CPUs that needed a pool of 20 threads to obtain its maximum throughput. That was because the threads spent 90% of their time blocked on I/O while making an outbound call to another resource.

Nonblocking I/O can help in this instance too: if those outbound HTTP or JDBC calls are nonblocking, we needn’t dedicate a thread to the call and can reduce the size of the thread pool accordingly.

## Asynchronous HTTP

HTTP clients are classes that (unsurprisingly) handle HTTP requests to a server. There are many clients, and they all have different functional as well as performance characteristics. In this section, we’ll look into the performance characteristics for common use cases among them.

Java 8 has a basic HTTP client, the java.net.HttpURLConnection class (and for secure connections, the subclass java.net.HttpsURLConnection). Java 11 adds a new client: the java.net.http.HttpClient class (which also handles HTTPS). Other HTTP client classes from other packages include org.apache.http.client.HttpClient from the Apache Foundation, org.asynchttpclient.AsyncHttpClient built on top of the Netty Project, and org.eclipse.jetty.client.HttpClient from the Eclipse Foundation.

Although it is possible to perform basic operations with the HttpURLConnection class, most REST calls are made using a framework such as JAX-RS. Hence, most HTTP clients directly implement those APIs (or slight variants), but the default implementation of JAX-RS also provides connectors for the most popular HTTP clients. Hence, you can use JAX-RS with the underlying HTTP client that gives you the best performance. The JAX-RS and underlying HTTP clients carry two basic performance considerations.

First, the JAX-RS connectors provide a Client object that is used to make the REST calls; when using the clients directly, they similarly provide a client object with a name like HttpClient (the HttpURLConnection class is an exception; it cannot be reused). A typical client would be created and used like this:

```
private static Client client;
static {
    ClientConfig cc = new ClientConfig();
    cc.connectorProvider(new JettyConnectorProvider());
    client = ClientBuilder.newClient(cc);
}

public Message getMessage() {
    Message m = client.target(URI.create(url))
        .request(MediaType.APPLICATION_JSON)
        .get(Message.class);
    return m;
}
```

The key in this example is that the client object is a static, shared object. All client objects are threadsafe, and all are expensive to instantiate, so you want only a limited number of them (e.g., one) in your application.

The second performance consideration is to make sure that the HTTP client properly pools connections and uses keepalive to keep connections open. Opening a socket for
