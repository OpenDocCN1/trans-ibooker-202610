serial collector. At that point, the usefulness of the serial collector wanes, so we’ll drop it from future examples.

One other point about the serial collector: an application with a very small heap (say, 100 MB) may still perform better with the serial collector regardless of the number of cores that are available.

# When to use the throughput collector

When a machine has multiple CPUs available, more-complex interactions can occur between GC algorithms, but at a basic level, the trade-offs between G1 GC and the throughput collector are the same as we’ve just seen. For example, Table 5-6 shows how our sample application works when running either two or four application threads on a machine with four cores (where the cores are not hyper-threaded).

*Table 5-6. Batch processing time with different GC algorithms*

| Application threads | G1 GC | Throughput |
|---|---|---|
| Two | 410 seconds (60.8%) | 446 seconds (59.7%) |
| Four | 513 seconds (99.5%) | 536 seconds (99.5%) |

The times in this table are the number of seconds required to run the test, and the CPU utilization of the machine is shown in parentheses. When there are two application threads, G1 GC is significantly faster than the throughput collector. The main reason is that the throughput collector spent 35 seconds paused for full GCs. G1 GC was able to avoid those collections, at the (relatively slight) increase in CPU time.

Even when there are four application threads, G1 still wins in this example. Here, the throughput collector paused the application threads for a total of 176 seconds. G1 GC paused the application threads for only 88 seconds. The G1 GC background threads did need to compete with the application threads for CPU cycles, which took about 65 seconds away from the application threads. That still meant G1 GC was 23 seconds faster.

When the elapsed time of an application is key, the throughput collector will be advantageous when it spends less time pausing the application threads than G1 GC does. That happens when one or more of these things occur:

+   - There are no (or few) full GCs. Full GC pauses can easily dominate the pause times of an application, but if they don’t occur in the first place, the throughput collector is no longer at a disadvantage.
+   - The old generation is generally full, causing the background G1 GC threads to work more.
+   - The G1 GC threads are starved for CPU.