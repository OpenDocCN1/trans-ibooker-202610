Recall that with .NET Core, there is only a single AppDomain. Even though extra AppDomain’s cannot be created, an application’s AppDomain can have numerous threads executing within it at any given time. To get a reference to the AppDomain that is hosting the application, call the static Thread.GetDomain() method, like so:

```
static void ExtractAppDomainHostingThread()
{
    // Obtain the AppDomain hosting the current thread.
    AppDomain ad = Thread.GetDomain();
}
```

A single thread may also be moved into an execution context at any given time, and it may be relocated within a new execution context at the whim of the .NET Core Runtime. When you want to obtain the current execution context a thread happens to be executing in, use the static Thread.CurrentThread.ExecutionContext property, like so:

```
static void ExtractCurrentThreadExecutionContext()
{
    // Obtain the execution context under which the
    // current thread is operating.
    ExecutionContext ctx =
        Thread.CurrentThread.ExecutionContext;
}
```

Again, the .NET Core Runtime oversees moving threads into (and out of) execution contexts. As a .NET Core developer, you can usually remain blissfully unaware where a given thread ends up. Nevertheless, you should be aware of the various ways of obtaining the underlying primitives.

# The Problem of Concurrency

One of the many “joys” (read: painful aspects) of multithreaded programming is that you have little control over how the underlying operating system or the runtime uses its threads. For example, if you craft a block of code that creates a new thread of execution, you cannot guarantee that the thread executes immediately. Rather, such code only instructs the OS/Runtime to execute the thread as soon as possible (which is typically when the thread scheduler gets around to it).

Furthermore, given that threads can be moved between application and contextual boundaries as required by the runtime, you must be mindful of which aspects of your application are thread-volatile (e.g., subject to multithreaded access) and which operations are atomic (thread-volatile operations are the dangerous ones!).

To illustrate the problem, assume a thread is invoking a method of a specific object. Now assume that this thread is instructed by the thread scheduler to suspend its activity to allow another thread to access the same method of the same object.

If the original thread was not finished with its operation, the second incoming thread may be viewing an object in a partially modified state. At this point, the second thread is basically reading bogus data, which is sure to give way to extremely odd (and hard to find) bugs, which are even harder to replicate and debug.

Atomic operations, on the other hand, are always safe in a multithreaded environment. Sadly, there are few operations in the .NET Core base class libraries that are guaranteed to be atomic. Even the act of assigning a value to a member variable is not atomic! Unless the .NET Core documentation specifically says an operation is atomic, you must assume it is thread-volatile and take precautions.