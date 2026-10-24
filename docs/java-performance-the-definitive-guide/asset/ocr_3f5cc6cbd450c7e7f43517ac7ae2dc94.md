# Table 9-1. Time required to calculate 10,000 mock price histories

| Number of threads | Seconds required | Percent of baseline |
| 1 | 55.2 ± 0.6 | 100% |
| 2 | 28.3 ± 0.3 | 51.2% |
| 4 | 13.9 ± 0.6 | 25.1% |
| 8 | 14.3 ± 0.2 | 25.9% |
| 16 | 14.5 ± 0.3 | 26.2% |

If the tasks in the application were completely parallel, the “Percent of baseline” column would show 50% for two threads and 25% for four threads. Such completely linear scaling is impossible to come by for several reasons: if nothing else, the threads must coordinate among themselves to pick a task from the run queue (and in general, there is usually more synchronization among the threads). By the time four threads are used, the system is consuming 100% of available CPU, and although the machine may not be running any other user-level applications, various system-level processes will kick in and use some CPU, preventing the JVM from utilizing all 100% of the cycles. Still, this application is doing a good job of scaling, and even if the number of threads in the pool is overestimated, we have only a slight penalty to pay.

# The Effect of Hyper-Threading

What if those four CPUs were hyper-threaded so that we have two cores and four hardware threads? Table 9-2 shows the same experiment on such a machine. As in the previous example, this appears to the JVM as a four-core machine, but the scaling of the benchmark is quite different.

# Table 9-2. Time required to calculate 10,000 mock price histories with hyper-threading

| Number of threads | Seconds required | Percent of baseline |
| 1 | 55.7 ± 0.1 | 100% |
| 2 | 28.1 ± 0.4 | 50.4% |
| 4 | 25.5 ± 0.4 | 45.7% |
| 8 | 25.7 ± 0.2 | 46.1% |
| 16 | 26.0 ± 0.2 | 46.6% |

Now the program scales well from one to two CPUs, since they are both full cores. But adding two hyper-threads gives us only a little benefit. This is the worst-case scenario; the benefit of hyper-threading is more apparent if the threads pause for I/O or wait for a lock held by another thread. Still, adding a hyper-thread will usually give only a 20% improvement.