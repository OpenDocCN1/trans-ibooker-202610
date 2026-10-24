![](bbox=[0.14, 0.08, 0.86, 0.62])

Figure 3-8. A flame graph including native code

# Quick Summary

+   - Native profilers provide visibility into both the JVM code and the application code.
+   - If a native profiler shows that time in GC dominates the CPU usage, tuning the collector is the right thing to do. If it shows significant time in the compilation threads, however, that is usually not affecting the application's performance.