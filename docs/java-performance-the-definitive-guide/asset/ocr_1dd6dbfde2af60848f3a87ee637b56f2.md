Table 9-4. Average response time for calculating mock stock price histories

| Number of client threads | Average response time | Percent of baseline |
| 1 | 0.022 second | 100% |
| 2 | 0.022 second | 100% |
| 4 | 0.024 second | 109% |
| 8 | 0.046 second | 209% |
| 16 | 0.093 second | 422% |
| 32 | 0.187 second | 885% |

Once the REST server is the bottleneck in this example (i.e., at four client threads), adding load into the server is quite harmful.

This example may seem somewhat contrived. Who would add more client threads when the server is already CPU-bound? But I’ve used this example simply because it is easy to understand and uses only Java programs. You can run it yourself to understand how it works, without having to set up database connections and schemas and whatnot.

The point is that the same principle holds here for a REST server that is sending requests to a database that is CPU- or I/O-bound. You might look only at the server’s CPU, see that is it well below 100% and that it has additional requests to process, and assume that increasing the number of threads in the server is a good idea. That would lead to a big surprise, because increasing the number of threads in that situation will actually decrease the total throughput (and possibly significantly), just as increasing the number of client threads did in the Java-only example.

This is another reason it is important to know where the actual bottleneck in a system is: if load is increased into the bottleneck, performance will decrease significantly. Conversely, if load into the current bottleneck is reduced, performance will likely increase.

This is also why self-tuning of thread pools is difficult. Thread pools usually have some visibility into the amount of work that they have pending and perhaps even how much CPU the machine has available—but they usually have no visibility into other aspects of the entire environment in which they are executing. Hence, adding threads when work is pending—a key feature of many self-tuning thread pools (as well as certain configurations of the ThreadPoolExecutor)—is often exactly the wrong thing to do.

In Table 9-4, the default configuration of the REST server was to create 16 threads on the four-CPU machine. That makes sense in a general default case, because the threads can be expected to make external calls. When those calls block waiting for a response, other tasks can be run, and the server will require more than four threads in order to execute those tasks. So a default that creates a few too many threads is a