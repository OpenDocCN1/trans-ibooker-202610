
(either top-level statements as shown here or a Main() method) triggers the creation of the primary foreground thread, as soon as the logic in the entry point completes, the AppDomain unloads before the secondary thread can complete its work.

However, if you comment out the line that sets the IsBackground property, you will find that each number prints to the console, as all foreground threads must finish their work before the AppDomain is unloaded from the hosting process.

For the most part, configuring a thread to run as a background type can be helpful when the worker thread in question is performing a noncritical task that is no longer needed when the main task of the program is finished. For example, you could build an application that pings an email server every few minutes for new emails, updates current weather conditions, or performs some other noncritical task.

## The Issue of Concurrency

When you build multithreaded applications, your program needs to ensure that any piece of shared data is protected against the possibility of numerous threads changing its value. Given that all threads in an AppDomain have concurrent access to the shared data of the application, imagine what might happen if multiple threads were accessing the same point of data. As the thread scheduler will force threads to suspend their work at random, what if thread A is kicked out of the way before it has fully completed its work? Thread B is now reading unstable data.

To illustrate the problem of concurrency, let’s build another Console Application project named MultiThreadedPrinting. This application will once again use the Printer class created previously, but this time the PrintNumbers() method will force the current thread to pause for a randomly generated amount of time.

```
using System;
using System.Threading;

namespace MultiThreadedPrinting
{
    public class Printer
    {
        public void PrintNumbers()
        {
            // Display Thread info.
            Console.WriteLine("{0} is executing PrintNumbers()", Thread.CurrentThread.Name);

            // Print out numbers.
            for (int i = 0; i < 10; i++)
            {
                // Put thread to sleep for a random amount of time.
                Random r = new Random();
                Thread.Sleep(1000 * r.Next(5));
                Console.Write("{0}, ", i);
            }
            Console.WriteLine();
        }
    }
}
```

The calling code is responsible for creating an array of ten (uniquely named) Thread objects, each of which is making calls on the same instance of the Printer object as follows:

```
using System;
using System.Threading;
using MultiThreadedPrinting;

Console.WriteLine("*****Synchronizing Threads *****
");

Printer p = new Printer();

// Make 10 threads that are all pointing to the same
// method on the same object.
Thread[] threads = new Thread[10];
for (int i = 0; i < 10; i++)
{
    threads[i] = new Thread(new ThreadStart(p.PrintNumbers))
    {
        Name = $"Worker thread #{i}"
    };
}
// Now start each one.
foreach (Thread t in threads)
{
    t.Start();
}
Console.ReadLine();
```

Before looking at some test runs, let’s recap the problem. The primary thread within this AppDomain begins life by spawning ten secondary worker threads. Each worker thread is told to make calls on the PrintNumbers() method on the same Printer instance. Given that you have taken no precautions to lock down this object’s shared resources (the console), there is a good chance that the current thread will be kicked out of the way before the PrintNumbers() method is able to print the complete results. Because you do not know exactly when (or if) this might happen, you are bound to get unpredictable results. For example, you might find the output shown here:

```
*****Synchronizing Threads *****
-> Worker thread #3 is executing PrintNumbers()
-> Worker thread #0 is executing PrintNumbers()
-> Worker thread #1 is executing PrintNumbers()
-> Worker thread #2 is executing PrintNumbers()
-> Worker thread #4 is executing PrintNumbers()
-> Worker thread #5 is executing PrintNumbers()
-> Worker thread #6 is executing PrintNumbers()
-> Worker thread #7 is executing PrintNumbers()
-> Worker thread #8 is executing PrintNumbers()
-> Worker thread #9 is executing PrintNumbers()
0, 0, 0, 0, 1, 0, 1, 0, 1, 0, 0, 0, 1, 0, 1, 1, 2, 3, 1, 2, 2, 2, 1, 2, 1, 1, 2, 2, 3, 3, 4,
3, 3, 2, 2, 3, 4, 3, 4, 5, 4, 5, 4, 4, 3, 6, 7, 2, 3, 4, 4, 4, 5, 6, 5, 3, 5, 8, 9,
6, 7, 4, 5, 6, 6, 5, 5, 5, 8, 5, 6, 7, 8, 7, 7, 6, 6, 6, 8, 9,
8, 7, 7, 7, 7, 9,
6, 8, 9,
8, 9,
9, 9,
8, 8, 7, 8, 9,
9,
9,
```

Now run the application a few more times and examine the output. It will most likely be different each time.

> Note If you are unable to generate unpredictable outputs, increase the number of threads from 10 to 100 (for example) or introduce another call to Thread.Sleep() within your program. Eventually, you will encounter the concurrency issue.

There are clearly some problems here. As each thread is telling the Printer to print the numerical data, the thread scheduler is happily swapping threads in the background. The result is inconsistent output. What you need is a way to programmatically enforce synchronized access to the shared resources. As you would guess, the System.Threading namespace provides several synchronization-centric types. The C# programming language also provides a keyword for the very task of synchronizing shared data in multithreaded applications.

## Synchronization Using the C# lock Keyword

The first technique you can use to synchronize access to shared resources is the C# lock keyword. This keyword allows you to define a scope of statements that must be synchronized between threads. By doing so, incoming threads cannot interrupt the current thread, thus preventing it from finishing its work. The lock keyword requires you to specify a token (an object reference) that must be acquired by a thread to enter within the lock scope. When you are attempting to lock down a private instance-level method, you can simply pass in a reference to the current type, as follows:

```
private void SomePrivateMethod()
{
    // Use the current object as the thread token.
    lock(this)
    {
        // All code within this scope is thread safe.
    }
}
```

However, if you are locking down a region of code within a public member, it is safer (and a best practice) to declare a private object member variable to serve as the lock token, like so:

```
public class Printer
{
    // Lock token.
    private object threadLock = new object();
```

```
public void PrintNumbers()
{
    // Use the lock token.
    lock (threadLock)
    {
        ...
    }
}
```

In any case, if you examine the PrintNumbers() method, you can see that the shared resource the threads are competing to gain access to is the console window. Scope all interactions with the Console type within a lock scope, as follows:

```
public void PrintNumbers()
{
    // Use the private object lock token.
    lock (threadLock)
    {
        // Display Thread info.
        Console.WriteLine("-> {0} is executing PrintNumbers()", Thread.CurrentThread.Name);
        // Print out numbers.
        Console.Write("Your numbers: ");
        for (int i = 0; i < 10; i++)
        {
            Random r = new Random();
            Thread.Sleep(1000 * r.Next(5));
            Console.Write("{0}, ", i);
        }
        Console.WriteLine();
    }
}
```

When doing this, you have effectively designed a method that will allow the current thread to complete its task. Once a thread enters into a lock scope, the lock token (in this case, a reference to the current object) is inaccessible by other threads until the lock is released after the lock scope has exited. Thus, if thread A has obtained the lock token, other threads are unable to enter any scope that uses the same lock token until thread A relinquishes the lock token.

> Note If you are attempting to lock down code in a static method, simply declare a private static object member variable to serve as the lock token.

If you now run the application, you can see that each thread has ample opportunity to finish its business.

```
****Synchronizing Threads ****
-> Worker thread #0 is executing PrintNumbers()
Your numbers: 0, 1, 2, 3, 4, 5, 6, 7, 8, 9,
-> Worker thread #1 is executing PrintNumbers()
```
