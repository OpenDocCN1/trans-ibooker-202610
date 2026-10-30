At this point, you might be wondering if it would be advantageous to use the .NET Core Runtime-maintained thread pool rather than explicitly creating Thread objects. Consider these benefits of leveraging the thread pool:

+   - The thread pool manages threads efficiently by minimizing the number of threads that must be created, started, and stopped.
+   - By using the thread pool, you can focus on your business problem rather than the application’s threading infrastructure.

However, using manual thread management is preferred in some cases. Here is an example:

+   - If you require foreground threads or must set the thread priority. Pooled threads are always background threads with default priority (ThreadPriority.Normal).
+   - If you require a thread with a fixed identity to abort it, suspend it, or discover it by name.

That wraps up your investigation of the System.Threading namespace. To be sure, understanding the topics presented thus far in the chapter (especially during your examination of concurrency issues) will be extremely valuable when creating a multithreaded application. Given this foundation, you will now turn your attention to several new thread-centric topics that were introduced in .NET 4.0 and carried forward to .NET Core. To begin, you will examine the role of an alternative threading model, the Task Parallel Library.

# Parallel Programming Using the Task Parallel Library

At this point in the chapter, you have examined the System.Threading namespace objects that allow you to build multithreaded software. Beginning with the release of .NET 4.0, Microsoft introduced a new approach to multithreaded application development using a parallel programming library termed the Task Parallel Library (TPL). Using the types of System.Threading.Tasks, you can build fine-grained, scalable parallel code without having to work directly with threads or the thread pool.

This is not to say, however, that you will not use the types of System.Threading when you use the TPL. Both threading toolkits can work together quite naturally. This is especially true in that the System.Threading namespace still provides most of the synchronization primitives you examined previously (Monitor, Interlocked, etc.). However, you will quite likely find that you will favor working with the TPL rather than the original System.Threading namespace, given that the same set of tasks can be performed in a more straightforward manner.

# The System.Threading.Tasks Namespace

Collectively speaking, the types of System.Threading.Tasks are referred to as the Task Parallel Library. The TPL will automatically distribute your application’s workload across available CPUs dynamically, using the runtime thread pool. The TPL handles the partitioning of the work, thread scheduling, state management, and other low-level details. The result is that you can maximize the performance of your .NET Core applications while being shielded from many of the complexities of directly working with threads.

# The Role of the Parallel Class

A key class of the TPL is System.Threading.Tasks.Parallel. This class contains methods that allow you to iterate over a collection of data (specifically, an object implementing IEnumerable<T>) in a parallel fashion, mainly through two primary static methods, Parallel.For() and Parallel.ForEach(), each of which defines numerous overloaded versions.

These methods allow you to author a body of code statements that will be processed in a parallel manner. In concept, these statements are the same sort of logic you would write in a normal looping