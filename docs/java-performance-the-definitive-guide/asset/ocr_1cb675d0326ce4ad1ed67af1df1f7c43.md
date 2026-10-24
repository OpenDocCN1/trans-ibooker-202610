Tests that measure throughput also commonly report the average response time of requests. That is an interesting piece of information, but changes in that number don't indicate a performance problem unless the reported throughput is the same. A server that can sustain 500 OPS with a 0.5-second response time is performing better than a server that reports a 0.3-second response time but only 400 OPS.

Throughput measurements are almost always taken after a suitable warm-up period, particularly because what is being measured is not a fixed set of work.

# Response-Time Tests

The last common test is one that measures *response time*: the amount of time that elapses between the sending of a request from a client and the receipt of the response.

The difference between a response-time test and a throughput test (assuming the latter is client/server based) is that client threads in a response-time test sleep for a period of time between operations. This is referred to as *think time*. A response-time test is designed to mimic more closely what a user does: the user enters a URL in a browser, spends time reading the page that comes back, clicks a link in the page, spends time reading that page, and so on.

When think time is introduced into a test, throughput becomes fixed: a given number of clients executing requests with a given think time will always yield the same TPS (with slight variance; see the following sidebar). At that point, the important measurement is the response time for the request: the effectiveness of the server is based on how quickly it responds to that fixed load.

# Think Time and Throughput

The throughput of a test in which the clients include think time can be measured in two ways. The simplest way is for clients to sleep for a period of time between requests:

```
while (!done) {
    executeOperation();
    Thread.currentThread().sleep(30*1000);
}
```

In this case, the throughput is somewhat dependent on the response time. If the response time is 1 second, it means that the client will send a request every 31 seconds, which will yield a throughput of 0.032 OPS. If the response time is 2 seconds, each client will send a request every 32 seconds, yielding a throughput of 0.031 OPS.

The other alternative is known as *cycle time* (rather than think time). Cycle time sets the total time between requests to 30 seconds, so that the time the client sleeps depends on the response time: