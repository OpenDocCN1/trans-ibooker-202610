![](bbox=[0.15, 0.08, 0.25, 0.16])

# Quick Summary

+   - G1 GC tuning should begin by setting a reasonable pause-time target.
+   - If full GCs are still an issue after that and the heap size cannot be increased, specific tunings can be applied for specific failure:
+     - To make the background threads run more frequently, adjust InitiatingHeapOccupancyPercent.
+     - If additional CPU is available, adjust the number of threads via the ConcGCThreads flag.
+     - To prevent promotion failures, decrease the size of G1MixedGCCountTarget.

# Understanding the CMS Collector

Although the CMS collector is deprecated, it is still available in current JDK builds. So this section covers how to tune it, as well as why it has been deprecated.

CMS has three basic operations:

+   - Collecting the young generation (stopping all application threads)
+   - Running a concurrent cycle to clean data out of the old generation
+   - Performing a full GC to compact the old generation, if necessary

A CMS collection of the young generation appears in Figure 6-7.

![](bbox=[0.14, 0.62, 0.82, 0.82])

Figure 6-7. Young collection performed by CMS