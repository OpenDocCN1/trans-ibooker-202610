# Understanding the ThreadPool

The next thread-centric topic you will examine in this chapter is the role of the runtime thread pool. There is a cost with starting a new thread, so for purposes of efficiency, the thread pool holds onto created (but inactive) threads until needed. To allow you to interact with this pool of waiting threads, the System.Threading namespace provides the ThreadPool class type.

If you want to queue a method call for processing by a worker thread in the pool, you can use the ThreadPool.QueueUserWorkItem() method. This method has been overloaded to allow you to specify an optional System.Object for custom state data in addition to an instance of the WaitCallback delegate.

```
public static class ThreadPool
{
    ...
    public static bool QueueUserWorkItem(WaitCallback callBack);
    public static bool QueueUserWorkItem(WaitCallback callBack,
        object state);
}
```

The WaitCallback delegate can point to any method that takes a System.Object as its sole parameter (which represents the optional state data) and returns nothing. Do note that if you do not provide a System.Object when calling QueueUserWorkItem(), the .NET Core Runtime automatically passes a null value. To illustrate queuing methods for use by the .NET Core Runtime thread pool, ponder the following program (in a console application called ThreadPoolApp), which uses the Printer type once again. In this case, however, you are not manually creating an array of Thread objects; rather, you are assigning members of the pool to the PrintNumbers() method.

```
using System;
using System.Threading;
using ThreadPoolApp;

Console.WriteLine("***** Fun with the .NET Core Runtime Thread Pool ****\n");

Console.WriteLine("Main thread started. ThreadID = {0}",
    Thread.CurrentThread.ManagedThreadId);

Printer p = new Printer();

WaitCallback workItem = new WaitCallback(PrintTheNumbers);

// Queue the method ten times.
for (int i = 0; i < 10; i++)
{
    ThreadPool.QueueUserWorkItem(workItem, p);
}
Console.WriteLine("All tasks queued");
Console.ReadLine();

static void PrintTheNumbers(object state)
{
    Printer task = (Printer)state;
    task.PrintNumbers();
}
```