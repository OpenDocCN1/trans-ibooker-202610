
```
{
    // Display Thread info.
    Console.WriteLine("-> {0} is executing PrintNumbers()",
        Thread.CurrentThread.Name);

    // Print out numbers.
    Console.Write("Your numbers: ");
    for(int i = 0; i < 10; i++)
    {
        Console.Write("{0}, ", i);
        Thread.Sleep(2000);
    }
    Console.WriteLine();
}
}
```

Now, within Program.cs, you will add top-level statements to first prompt the user to determine whether one or two threads will be used to perform the application’s work. If the user requests a single thread, you will simply invoke the PrintNumbers() method within the primary thread. However, if the user specifies two threads, you will create a ThreadStart delegate that points to PrintNumbers(), pass this delegate object into the constructor of a new Thread object, and call Start() to inform the .NET Core Runtime this thread is ready for processing. Here is the complete implementation:

```
using System;
using System.Threading;
using SimpleMultiThreadApp;

Console.WriteLine("***** The Amazing Thread App *****");
Console.Write("Do you want [1] or [2] threads? ");
string threadCount = Console.ReadLine();

// Name the current thread.
Thread primaryThread = Thread.CurrentThread;
primaryThread.Name = "Primary";

// Display Thread info.
Console.WriteLine("-> {0} is executing Main()",
Thread.CurrentThread.Name);

// Make worker class.
Printer p = new Printer();

switch(threadCount)
{
case "2":
    // Now make the thread.
    Thread backgroundThread =
        new Thread(new ThreadStart(p.PrintNumbers));
    backgroundThread.Name = "Secondary";
    backgroundThread.Start();
    break;
case "1":
    p.PrintNumbers();
    break;
default:
    Console.WriteLine("I don't know what you want...you get 1 thread.");
    goto case "1";
}
// Do some additional work.
Console.WriteLine("This is on the main thread, and we are finished.");
Console.ReadLine();
```

Now, if you run this program with a single thread, you will find that the final message box will not display the message until the entire sequence of numbers has printed to the console. As you are explicitly pausing for approximately two seconds after each number is printed, this will result in a less-than-stellar end-user experience. However, if you select two threads, the message box displays instantly, given that a unique Thread object is responsible for printing the numbers to the console.

## Working with the ParameterizedThreadStart Delegate

Recall that the ThreadStart delegate can point only to methods that return void and take no arguments. While this might fit the bill in some cases, if you want to pass data to the method executing on the secondary thread, you will need to use the ParameterizedThreadStart delegate type. To illustrate, create a new Console Application project named AddWithThreads and import the System.Threading namespace. Now, given that ParameterizedThreadStart can point to any method taking a System.Object parameter, you will create a custom type containing the numbers to be added, like so:

```
namespace AddWithThreads
{
    class AddParams
    {
        public int a, b;

        public AddParams(int numb1, int numb2)
        {
            a = numb1;
            b = numb2;
        }
    }
}
```

Next, create a method in the Program class that will take an AddParams parameter and print the sum of the two numbers involved, as follows:

```
void Add(object data)
{
    if (data is AddParams ap)
    {
        Console.WriteLine("ID of thread in Add(): {0}",
            Thread.CurrentThread.ManagedThreadId);

        Console.WriteLine("{0} + {1} is {2}",
            ap.a, ap.b, ap.a + ap.b);
    }
}
```

The code within Program.cs is straightforward. Simply use ParameterizedThreadStart rather than ThreadStart, like so:

```
using System;
using System.Threading;
using AddWithThreads;

Console.WriteLine("***** Adding with Thread objects *****");
Console.WriteLine("ID of thread in Main(): {0}",
    Thread.CurrentThread.ManagedThreadId);

// Make an AddParams object to pass to the secondary thread.
AddParams ap = new AddParams(10, 10);
Thread t = new Thread(new ParameterizedThreadStart(Add));
t.Start(ap);

// Force a wait to let other thread finish.
Thread.Sleep(5);
Console.ReadLine();
```

## The AutoResetEvent Class

In these first few examples, there is not a clean way to know when the secondary thread has completed its work. In the last example, Sleep was called with an arbitrary time to let the other thread finish. One simple and thread-safe way to force a thread to wait until another is completed is to use the AutoResetEvent class. In the thread that needs to wait, create an instance of this class and pass in false to the constructor to signify you have not yet been notified. Then, at the point at which you are willing to wait, call the WaitOne() method. Here is the update to the Program.cs class, which will do this very thing using a static-level AutoResetEvent member variable:

```
AutoResetEvent _waitHandle = new AutoResetEvent(false);

Console.WriteLine("***** Adding with Thread objects *****");
Console.WriteLine("ID of thread in Main(): {0}",
    Thread.CurrentThread.ManagedThreadId);
AddParams ap = new AddParams(10, 10);
Thread t = new Thread(new ParameterizedThreadStart(Add));
t.Start(ap);

// Wait here until you are notified!
_waitHandle.WaitOne();
Console.WriteLine("Other thread is done!");

Console.ReadLine();
...
```

When the other thread is completed with its workload, it will call the Set() method on the same instance of the AutoResetEvent type.

```
void Add(object data)
{
    if (data is AddParams ap)
    {
        Console.WriteLine("ID of thread in Add(): {0}",
            Thread.CurrentThread.ManagedThreadId);

        Console.WriteLine("{0} + {1} is {2}",
            ap.a, ap.b, ap.a + ap.b);

        // Tell other thread we are done.
        _waitHandle.Set();
    }
}
```

## Foreground Threads and Background Threads

Now that you have seen how to programmatically create new threads of execution using the System.Threading namespace, let’s formalize the distinction between foreground threads and background threads:

- *Foreground threads* can prevent the current application from terminating. The .NET Core Runtime will not shut down an application (which is to say, unload the hosting AppDomain) until all foreground threads have ended.
- *Background threads* (sometimes called *daemon threads*) are viewed by the .NET Core Runtime as expendable paths of execution that can be ignored at any point in time (even if they are currently laboring over some unit of work). Thus, if all foreground threads have terminated, all background threads are automatically killed when the application domain unloads.

It is important to note that foreground and background threads are *not* synonymous with primary and worker threads. By default, every thread you create via the Thread.Start() method is automatically a foreground thread. Again, this means that the AppDomain will not unload until all threads of execution have completed their units of work. In most cases, this is exactly the behavior you require.

For the sake of argument, however, assume that you want to invoke Printer.PrintNumbers() on a secondary thread that should behave as a background thread. Again, this means the method pointed to by the Thread type (via the ThreadStart or ParameterizedThreadStart delegate) should be able to halt safely as soon as all foreground threads are done with their work. Configuring such a thread is as simple as setting the IsBackground property to true, like so:

```
Console.WriteLine("***** Background Threads *****");
Printer p = new Printer();
Thread bgroundThread =
    new Thread(new ThreadStart(p.PrintNumbers));

// This is now a background thread.
bgroundThread.IsBackground = true;
bgroundThread.Start();
```

Notice that this code is *not* making a call to Console.ReadLine() to force the console to remain visible until you press the Enter key. Thus, when you run the application, it will shut down immediately because the Thread object has been configured as a background thread. Given that the entry point into an application
