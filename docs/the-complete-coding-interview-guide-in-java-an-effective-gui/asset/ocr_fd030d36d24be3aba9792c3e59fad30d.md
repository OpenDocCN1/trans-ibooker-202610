Concurrency is achieved via *multithreading*. *Multithreading* is a technique that enables a program or application to handle more than one task at a time and to also synchronize those tasks. This means that multithreading allows the maximum utilization of a CPU by executing two or more tasks virtually at the same time. We say *virtually at the same time* here because the tasks only look like they are running simultaneously; however, essentially, they cannot do that. They take advantage of CPU *context switching* or the *time slicing* feature of the operating system. In other words, CPU time is shared across all running tasks, and each task is scheduled to run for a certain period of time. Hence, multithreading is the key to *multitasking*.

**Important note**
With a single-core CPU, we may achieve concurrency but *not* parallelism.

In conclusion, threads can create the illusion of multitasking; however, at any given point in time, the CPU is executing only one thread. The CPU switches control between the threads so quickly that it creates the illusion that the tasks are executed (or advance) in parallel. Actually, they are executed concurrently. Nevertheless, with advances in hardware technology, it is now common to have multi-core machines and computers. This means that applications can take advantage of these architectures and have a dedicated CPU running each thread.

The following diagram clarifies the confusion between concurrency and parallelism via four threads (T1, T2, T3, and T4):

![](bbox=[0.2, 0.55, 0.78, 0.73])

16.1 – Concurrency versus parallelism

So, an application can be one of the following:

+   - **Concurrent but not parallel**: It executes more than one task at the same time, but no two tasks are executed at the same time.