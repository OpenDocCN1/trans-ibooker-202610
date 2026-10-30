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

Console.WriteLine("***** The Amazing Thread App *****
");
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
```