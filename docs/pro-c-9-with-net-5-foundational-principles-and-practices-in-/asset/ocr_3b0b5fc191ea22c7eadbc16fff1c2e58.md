```
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

# The AutoResetEvent Class

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