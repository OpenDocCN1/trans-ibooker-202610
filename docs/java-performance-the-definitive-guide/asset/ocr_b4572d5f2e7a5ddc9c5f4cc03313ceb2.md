64-bit JVMs aren’t limited that way by the process size, but they are limited by the total amount of virtual memory on the machine. Say you have a small server with 4 GB of physical memory and 10 GB of virtual memory, and start a JVM with a maximum heap size of 6 GB. That will reserve 6 GB of virtual memory (plus more for non-heap memory sections). Regardless of how large that heap grows (and the memory that is committed), a second JVM will be able to reserve only less than 4 GB of memory on that machine.

All things being equal, it’s convenient to oversize JVM structures and let the JVM optimally use that memory. But it isn’t always feasible.

This difference applies to almost all significant memory that the JVM allocates. The code cache grows from an initial to a maximum value as more code gets compiled. Metaspace is allocated separately from the heap and grows between its initial (committed) size and its maximum (reserved) size.

Thread stacks are an exception to this. Every time the JVM creates a thread, the OS allocates some native memory to hold that thread’s stack, committing more memory to the process (until the thread exits at least). Thread stacks, though, are fully allocated when they are created.

In Unix systems, the footprint of an application can be estimated by the *resident set size (RSS)* of the process as reported by various OS tools. That value is a good estimate of the amount of committed memory a process is using, though it is inexact in two ways. First, the few pages that are shared at the OS level between JVM and other processes (that is, the text portions of shared libraries) are counted in the RSS of each process. Second, a process may have committed more memory than it paged in at any moment. Still, tracking the RSS of a process is a good first-pass way to monitor the total memory use. On more recent Linux kernels, the PSS is a refinement of the RSS that removes the data shared by other programs.

On Windows systems, the equivalent idea is called the *working set* of an application, which is what is reported by the task manager.

# Minimizing Footprint

To minimize the footprint used by the JVM, limit the amount of memory used by the following:

# *Heap*

The heap is the biggest chunk of memory, though surprisingly it may take up only 50% to 60% of the total footprint. Using a smaller maximum heap (or setting the GC tuning parameters such that the heap never fully expands) limits the program’s footprint.