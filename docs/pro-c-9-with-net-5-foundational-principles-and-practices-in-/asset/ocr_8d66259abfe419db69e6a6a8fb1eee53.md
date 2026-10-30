```
while (true);

Console.ReadLine();
```

Now, inform the PLINQ query that it should be on the lookout for an incoming cancellation request by chaining on the WithCancellation() extension method and passing in the token. In addition, you will want to wrap this PLINQ query in a proper try/catch scope and deal with the possible exception. Here is the final version of the ProcessIntData() method:

```
void ProcessIntData()
{
    // Get a very large array of integers.
    int[] source = Enumerable.Range(1, 10_000_000).ToArray();
    // Find the numbers where num % 3 == 0 is true, returned
    // in descending order.
    int[] modThreeIsZero = null;
    try
    {
        modThreeIsZero = (from num in source.AsParallel().WithCancellation(_cancelToken.Token)
                          where num % 3 == 0
                          orderby num descending
                          select num).ToArray();
        Console.WriteLine();
        Console.WriteLine($"Found {modThreeIsZero.Count()} numbers that match query!");
    }
    catch (OperationCanceledException ex)
    {
        Console.WriteLine(ex.Message);
    }
}
```

When running this, you will want to hit Q and enter quickly to see the message from the cancellation token. On my development machine, I had about one second to quit before it finished on its own.

# Async Calls with the async/await

I have covered a lot of material in this (rather lengthy) chapter. To be sure, building, debugging, and understanding complex multithreaded applications are challenging in any framework. While the TPL, PLINQ, and the delegate type can simplify matters to some extent (especially when compared to other platforms and languages), developers are still required to know the ins and outs of various advanced techniques.

Since the release of .NET 4.5, the C# programming language has been updated with two new keywords that further simplify the process of authoring asynchronous code. In contrast to all the examples in this chapter, when you use the new async and await keywords, the compiler will generate a good deal of threading code on your behalf, using numerous members of the System.Threading and System.Threading.Tasks namespaces.

# A First Look at the C# async and await Keywords (Updated 7.1, 9.0)

The async keyword of C# is used to qualify that a method, lambda expression, or anonymous method should be called in an asynchronous manner automatically. Yes, it is true. Simply by marking a method