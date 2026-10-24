At one point, the application (NetBeans) was using a maximum of 45 threads. At the beginning of the graph, we can see a burst where the application was using up to 38, but it settled on using between 30 and 31. `jconsole` can also print an individual thread stack; as the figure shows, the Java2D Disposer thread is presently waiting on a reference queue lock.

# Blocked Thread Visibility

Real-time thread monitoring is useful for a very high-level picture of what threads are running in the application, but it doesn’t really provide any data on what those threads are doing. Determining whether the threads are spending CPU cycles requires the use of a profiler, as discussed in [Chapter 3](#). Profilers provide great visibility into what threads are executing, and they are generally sophisticated enough to guide you to areas in the code where better algorithms and code choices can speed up overall execution.

It is more difficult to diagnose threads that are blocked, although that information is often more important in the overall execution of an application—particularly if that code is running on a multi-CPU system and is not utilizing all the available CPU. Three approaches can be used to perform this diagnosis. One approach is again to use a profiler, since most profiling tools will provide a timeline of thread execution that allows you to see the points when a thread was blocked. An example was given in [Chapter 3](#).

# Blocked threads and JFR

By far, the best way to know when threads are blocked is to use tools that can look into the JVM and know at a low level when the threads are blocked. One such tool is the Java Flight Recorder, introduced in [Chapter 3](#). We can drill into the events that JFR captures and look for those that are causing a thread to block. The usual event to look for is threads that are waiting to acquire a monitor, but if we observe threads with long reads (and rarely, long writes) to a socket, they are likely blocked as well.

These events can be easily viewed on the Histogram panel of Java Mission Control, as shown in [Figure 9-3](#).

In this sample, the lock associated with the `HashMap` in the `sun.awt.AppContext.get()` method was contended 163 times (over 66 seconds), causing an average 31 ms increase in the response time of the request being measured. The stack trace points out that the contention stems from the way the JSP is writing a `java.util.Date` object. To improve the scalability of this code, a thread-local date formatter could be used instead of simply calling the date’s `toString()` method.