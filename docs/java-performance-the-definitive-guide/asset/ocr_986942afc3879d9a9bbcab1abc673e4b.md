+   - If enough CPU is available, using the concurrent collector to avoid full GC pauses will allow the job to finish faster.
+   - If CPU is limited, the extra CPU consumption of the concurrent collector will cause the batch job to take more time.

![](bbox=[0.16, 0.21, 0.25, 0.28])

# Quick Summary

+   - GC algorithms generally divide the heap into old and young generations.
+   - GC algorithms generally employ a stop-the-world approach to clearing objects from the young generation, which is usually a quick operation.
+   - Minimizing the effect of performing GC in the old generation is a trade-off between pause times and CPU usage.

# GC Algorithms

OpenJDK 12 provides a variety of GC algorithms with varying degrees of support in earlier releases. Table 5-1 lists these algorithms and their status in OpenJDK and Oracle Java releases.

# Table 5-1. Support level of various GC algorithmsᵃ

| GC algorithm | Support in JDK 8 | Support in JDK 11 | Support in JDK 12 |
|---|---|---|---|
| Serial GC | S | S | S |
| Throughput (Parallel) GC | S | S | S |
| G1 GC | S | S | S |
| Concurrent Mark-Sweep (CMS) | S | D | D |
| ZGC | - | E | E |
| Shenandoah | E2 | E2 | E2 |
| Epsilon GC | - | E | E |

ᵃ (S: Fully Supported D: Deprecated E: Experimental E2: Experimental; in OpenJDK builds but not Oracle builds)

A brief description of each algorithm follows; Chapter 6 provides more details on tuning them individually.

# The serial garbage collector

The serial garbage collector is the simplest of the collectors. This is the default collector if the application is running on a client-class machine (32-bit JVMs on Windows) or on a single-processor machine. At one point, the serial collector seemed like it was