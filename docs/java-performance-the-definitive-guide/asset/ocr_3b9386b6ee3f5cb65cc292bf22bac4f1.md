In other circumstances, though, the penalty for too many threads can be larger. In the REST version of the stock history calculator, having too many threads has a bigger effect, as is shown in Table 9-3. The application server is configured to have the given number of threads, and a load generator is sending 16 simultaneous requests to the server.

# Table 9-3. Operations per second for mock stock prices through a REST server

| Number of threads | Operations per second | Percent of baseline |
|---|---|---|
| 1 | 46.4 | 27% |
| 4 | 169.5 | 100% |
| 8 | 165.2 | 97% |
| 16 | 162.2 | 95% |

Given that the REST server has four available CPUs, maximum throughput is achieved with that many threads in the pool.

Chapter 1 discussed the need to determine where the bottleneck is when investigating performance issues. In this example, the bottleneck is clearly the CPU: at four CPUs, the CPU is 100% utilized. Still, the penalty for adding more threads in this case is somewhat minimal, at least until there are four times too many threads.

But what if the bottleneck is elsewhere? This example is also somewhat unusual in that the tasks are completely CPU-bound: they do no I/O. Typically, the threads might be expected to make calls to a database or write their output somewhere or even rendezvous with another resource. In that case, the CPU won't necessarily be the bottleneck: that external resource might be.

When that is the case, adding threads to the thread pool is detrimental. Although I said (only somewhat tongue in cheek) in Chapter 1 that the database is always the bottleneck, the bottleneck can be any external resource.

As an example, consider the stock REST server with the roles reversed: what if the goal is to make optimal use of the load generator machine (which, after all, is simply running a threaded Java program)?

In typical usage, if the REST application is run in a server with four CPUs and has only a single client requesting data, the REST server will be about 25% busy, and the client machine will be almost idle. If the load is increased to four concurrent clients, the server will be 100% busy, and the client machine may be only 20% busy.

Looking only at the client, it is easy to conclude that because the client has a lot of excess CPU, it should be possible to add more threads to the client and improve its throughput. Table 9-4 shows how wrong that assumption is: when threads are added to the client, performance is drastically affected.