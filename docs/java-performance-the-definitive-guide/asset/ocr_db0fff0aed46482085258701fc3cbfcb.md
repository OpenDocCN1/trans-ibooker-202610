which need to be tuned to use less memory), but an application that is leaking native memory without bound is typically doing so because of issues in a native library.

No Java-level tools can really help us detect where an application is using native memory from shared libraries. OS-level tools can tell us that the working set of the process is continually growing, and if a process grows to have a working set of 10 GB and NMT tells us that the JVM has committed only 6 GB of memory, we know that the other 4 GB of memory must come from native library allocations.

Figuring out which native library is responsible requires OS-level tools rather than tools from the JDK. Various debugging versions of malloc can be used for this purpose. These are useful to a point, though often native memory is allocated via an mmap call, and most libraries to track malloc calls will miss those.

A good alternative is a profiler that can profile native code as well as Java code. For example, in Chapter 3 we discussed the Oracle Studio Profiler, which is a mixed-language profiler. That profiler has an option to trace memory allocations as well—one caveat is that it can track only the memory allocations of the native code and not the Java code, but that's what we're after in this case.

Figure 8-1 shows the native allocation view within the Studio Profiler.

![](bbox=[0.13, 0.46, 0.85, 0.61])

Figure 8-1. Profiling of native memory

This call graph shows us that the WebLogic native function mapFile has used mmap to allocate about 150 GB of native memory into our process. This is a little misleading: multiple mappings to that file exist, and the profiler isn't quite smart enough to realize that they are sharing the actual memory: if there were, for example, 100 mappings of that 15 GB file, the memory usage increases by only 15 GB. (And, to be frank, I purposely corrupted that file to make it that large; this is in no way reflective of actual usage.) Still, the native profiler has pointed to the location of the issue.

Within the JDK itself, two common operations can lead to large amounts of native memory usage: the use of Inflater and Deflater objects, and the use of NIO buffers. Even without profiling, there are ways to detect if these operations are causing your native memory growth.