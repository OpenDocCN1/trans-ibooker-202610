1. In SharedObjects, declare and instantiate an object variable to act as a conch, as shown in the following code:

```
public static object Conch = new();
```

2. In both MethodA and MethodB, add a lock statement for the conch around the for statements, as shown highlighted in the following code:

```
lock (SharedObjects.Conch)
{
    for (int i = 0; i < 5; i++)
    {
        Thread.Sleep(SharedObjects.Random.Next(2000));
        SharedObjects.Message += "A";
        Write(".");
    }
}
```

![](bbox=[0.22, 0.38, 0.3, 0.47])

Good Practice: Note that since checking the conch is voluntary, if you only use the lock statement in one of the two methods, the shared resource will continue to be accessed by both methods. Make sure that all methods that access a shared resource respect the conch.

3. Run the code and view the result, as shown in the following output:

```
Please wait for the tasks to complete.
..........
Results: BBBBAAAAA.
10,345 elapsed milliseconds.
```

Although the time elapsed was longer, only one method at a time could access the shared resource. Either MethodA or MethodB can start first. Once a method has finished its work on the shared resource, then the conch gets released, and the other method has the chance to do its work.

# Understanding the lock statement

You might wonder what the lock statement does when it "locks" an object variable (hint: it does not lock the object!), as shown in the following code:

```
lock (SharedObjects.Conch)
{
    // work with shared resource
}
```