# The CPU Run Queue

Both Windows and Unix systems allow you to monitor the number of threads that can be run (meaning that they are not blocked on I/O, or sleeping, and so on). Unix systems refer to this as the run queue, and several tools include the run queue length in their output. That includes the vmstat output in the previous section: the first number in each line is the length of the run queue. Windows refers to this number as the processor queue and reports it (among other ways) via typeperf:

```
C:> typeperf -si 1 "\System\Processor Queue Length"
"05/11/2019 19:09:42.678", "0.000000"
"05/11/2019 19:09:43.678", "0.000000"
```

There is an important difference in this output: the run queue length number on a Unix system (which was either 1 or 2 in the sample vmstat output) is the number of all threads that are running or that could run if there were an available CPU. In that example, there was always at least one thread that wanted to run: the single thread doing application work. Hence, the run queue length was always at least 1. Keep in mind that the run queue represents everything on the machine, so sometimes there are other threads (from completely separate processes) that want to run, which is why the run queue length sometimes was 2 in that sample output.

In Windows, the processor queue length does not include the number of threads that are currently running. Hence, in the typeperf sample output, the processor queue number was 0, even though the machine was running the same single-threaded application with one thread always executing.

If there are more threads to run than available CPUs, performance begins to degrade. In general, then, you want the processor queue length to be 0 on Windows and equal to (or less than) the number of CPUs on Unix systems. That isn't a hard-and-fast rule; system processes and other things will come along periodically and briefly raise that value without any significant performance impact. But if the run queue length is too high for any significant period of time, it's an indication that the machine is overloaded, and you should look into reducing the amount of work the machine is doing (either by moving jobs to another machine or by optimizing the code).

![](bbox=[0.15, 0.72, 0.24, 0.81])

# Quick Summary

+   - CPU time is the first thing to examine when looking at the performance of an application.
+   - The goal in optimizing code is to drive the CPU usage up (for a shorter period of time), not down.
+   - Understand why CPU usage is low before diving in and attempting to tune an application.