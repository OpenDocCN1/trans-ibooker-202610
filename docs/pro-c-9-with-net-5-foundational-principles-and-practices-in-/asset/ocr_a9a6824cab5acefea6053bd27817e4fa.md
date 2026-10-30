# The Role of Thread Synchronization

At this point, it should be clear that multithreaded programs are in themselves quite volatile, as numerous threads can operate on the shared resources at (more or less) the same time. To protect an application’s resources from possible corruption, .NET Core developers must use any number of threading primitives (such as locks, monitors, and the [Synchronization] attribute or language keyword support) to control access among the executing threads.

Although the .NET Core platform cannot make the difficulties of building robust multithreaded applications completely disappear, the process has been simplified considerably. Using types defined within the System.Threading namespace, the Task Parallel Library, and the C# async and await language keywords, you can work with multiple threads with minimal fuss and bother.

Before diving into the System.Threading namespace, the TPL, and the C# async and await keywords, you will begin by examining how the .NET Core delegate type can be used to invoke a method in an asynchronous manner. While it is most certainly true that since .NET 4.6 the new C# async and await keywords offer a simpler alternative to asynchronous delegates, it is still important that you know how to interact with code using this approach (trust me, there is a ton of code in production that uses asynchronous delegates).

# The System.Threading Namespace

Under the .NET and .NET Core platforms, the System.Threading namespace provides types that enable the direct construction of multithreaded applications. In addition to providing types that allow you to interact with a .NET Core Runtime thread, this namespace defines types that allow access to the .NET Core Runtime-maintained thread pool, a simple (non-GUI-based) Timer class, and numerous types used to provide synchronized access to shared resources. Table 15-1 lists some of the important members of this namespace. (Be sure to consult the .NET Core SDK documentation for full details.)

Table 15-1. Core Types of the System.Threading Namespace

| Type | Meaning in Life |
|------|-----------------|
| Interlocked | This type provides atomic operations for variables that are shared by multiple threads. |
| Monitor | This type provides the synchronization of threading objects using locks and wait/signals. The C# lock keyword uses a Monitor object under the hood. |
| Mutex | This synchronization primitive can be used for synchronization between application domain boundaries. |
| ParameterizedThreadStart | This delegate allows a thread to call methods that take any number of arguments. |
| Semaphore | This type allows you to limit the number of threads that can access a resource concurrently. |
| Thread | This type represents a thread that executes within the .NET Core Runtime. Using this type, you can spawn additional threads in the originating AppDomain. |
| ThreadPool | This type allows you to interact with the .NET Core Runtime-maintained thread pool within a given process. |