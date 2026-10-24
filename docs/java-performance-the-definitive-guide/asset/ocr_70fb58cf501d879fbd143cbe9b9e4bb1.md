![](bbox=[0.169, 0.076, 0.251, 0.159])

# Quick Summary

+   - Basic visibility into the threads of a system provides an overview of the number of threads running.
+   - Thread visibility allows us to determine why threads are blocked: whether because they are waiting for a resource or for I/O.
+   - Java Flight Recorder provides an easy way to examine the events that caused a thread to block.
+   - `jstack` provides a level of visibility into the resources threads are blocked on.

# Summary

Understanding how threads operate can yield important performance benefits. Thread performance, though, is not so much about tuning—there are relatively few JVM flags to tweak, and those few flags have limited effects.

Instead, good thread performance is about following best-practice guidelines for managing the number of threads and for limiting the effects of synchronization. With the help of appropriate profiling and lock analysis tools, applications can be examined and modified so that threading and locking issues do not negatively affect performance.