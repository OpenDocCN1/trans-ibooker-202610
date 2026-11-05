```
if (Monitor.TryEnter(SharedObjects.Conch, TimeSpan.FromSeconds(15)))
{
    for (int i = 0; i < 5; i++)
    {
        Thread.Sleep(SharedObjects.Random.Next(2000));
        SharedObjects.Message += "A";
        Write(".");
    }
}
else
{
    WriteLine("Method A timed out when entering a monitor on conch.");
}
finally
{
    Monitor.Exit(SharedObjects.Conch);
}
```

2. Run the code and view the result, which should return the same results as before (although either A or B could grab the conch first) but is better code because it will prevent potential deadlocks.

![](bbox=[0.17, 0.56, 0.22, 0.64])

**Good Practice:** Only use the lock keyword if you can write your code such that it avoids potential deadlocks. If you cannot avoid potential deadlocks, then always use the Monitor.TryEnter method instead of lock, in combination with a try-finally statement, so that you can supply a timeout and one of the threads will back out of a deadlock if it occurs. You can read more about good threading practices at the following link: https://docs.microsoft.com/en-us/dotnet/standard/threading/managed-threading-best-practices

# Synchronizing events

In Chapter 6, Implementing Interfaces and Inheriting Classes, you learned how to raise and handle events. But .NET events are not thread-safe, so you should avoid using them in multithreaded scenarios and follow the standard event raising code I showed you earlier.

After learning that .NET events are not thread-safe, some developers attempt to use exclusive locks when adding and removing event handlers or when raising an event, as shown in the following code:

```
// event delegate field
public event EventHandler Shout;

// conch
```