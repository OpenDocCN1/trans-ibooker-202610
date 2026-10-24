# Quick Summary

+   - The calculations for the default initial and maximum heap sizes are fairly straightforward on most machines.
+   - Around the edges, these calculations can be quite involved.

# Experimental GC Algorithms

In JDK 8 and JDK 11 production VMs with multiple CPUs, you’ll use either the G1 GC or throughput collector, depending on your application requirements. On small machines, you’ll use the serial collector if that is appropriate for your hardware. Those are the production-supported collectors.

JDK 12 introduces new collectors. Although these collectors are not necessarily production-ready, we’ll take a peek into them for experimental purposes.

# Concurrent Compaction: ZGC and Shenandoah

Existing concurrent collectors are not fully concurrent. Neither G1 GC nor CMS has concurrent collection of the young generation: freeing the young generation requires all application threads to be stopped. And neither of those collectors does concurrent compaction. In G1 GC, the old generation is compacted as an effect of the mixed GC cycles: within a target region, objects that are not freed are compacted into empty regions. In CMS, the old generation is compacted when it becomes too fragmented to allow new allocations. Collections of the young generation also compact that portion of the heap by moving surviving objects into the survivor spaces or the old generation.

During compaction, objects move their position in memory. This is the primary reason the JVM stops all application threads during that operation—the algorithms to update the memory references are much simpler if the application threads are known to be stopped. So the pause times of an application are dominated by the time spent moving objects and making sure references to them are up-to-date.

Two experimental collectors are designed to address this problem. The first is the Z garbage collector, or ZGC; the second is the Shenandoah garbage collector. ZGC first appeared in JDK 11; Shenandoah GC first appeared in JDK 12 but has now been backported to JDK 8 and JDK 11. JVM builds from AdoptOpenJDK (or that you compile yourself from source) contain both collectors; builds that come from Oracle contain only ZGC.

To use these collectors, you must specify the `-XX:+UnlockExperimentalVMOptions` flag (by default, it is false). Then you specify either `-XX:+UseZGC` or `-XX:+UseShenandoahGC` in place of other GC algorithms. Like other GC algorithms,