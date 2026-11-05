
A query that skips and takes cannot be written using only the query comprehension syntax, so we could write the query using all extension methods, as shown in the following code:

```
var query = names
    .Where(name => name.Length > 4)
    .Skip(80)
    .Take(10);
```

Or you can wrap query comprehension syntax in parentheses and then switch to using extension methods, as shown in the following code:

```
var query = (from name in names
    where name.Length > 4
    select name)
    .Skip(80)
    .Take(10);
```

> **Good Practice:** Learn both extension methods with lambda expressions and the query comprehension syntax ways of writing LINQ queries, because you are likely to have to maintain code that uses both.

## Using multiple threads with parallel LINQ

By default, only one thread is used to execute a LINQ query. **Parallel LINQ (PLINQ)** is an easy way to enable multiple threads to execute a LINQ query.

> **Good Practice:** Do not assume that using parallel threads will improve the performance of your applications. Always measure real-world timings and resource usage.

## Creating an app that benefits from multiple threads

To see it in action, we will start with some code that only uses a single thread to calculate Fibonacci numbers for 45 integers. We will use the Stopwatch type to measure the change in performance.

We will use operating system tools to monitor the CPU and CPU core usage. If you do not have multiple CPUs or at least multiple cores, then this exercise won't show much!

- 1. Use your preferred code editor to add a new console app named LinqInParallel to the Chapter11 solution/workspace.
- 2. In Visual Studio Code, select LinqInParallel as the active OmniSharp project.
- 3. In Program.cs, delete the existing statements and then import the System.Diagnostics namespace so that we can use the Stopwatch type, and statically import the System.Console type.
- 4. Add statements to create a stopwatch to record timings, wait for a keypress before starting the timer, create 45 integers, calculate the last Fibonacci number for each of them, stop the timer, and display the elapsed milliseconds, as shown in the following code:
- 5. Run the code, but do not press Enter to start the stopwatch yet because we need to make sure a monitoring tool is showing processor activity.

```
Stopwatch watch = new();
Write("Press ENTER to start. ");
ReadLine();
watch.Start();

int max = 45;

IEnumerable<int> numbers = Enumerable.Range(start: 1, count: max);

WriteLine($"Calculating Fibonacci sequence up to {max}. Please wait...");

int[] fibonacciNumbers = numbers
    .Select(number => Fibonacci(number)).ToArray();

watch.Stop();
WriteLine("{0:#,##0} elapsed milliseconds.",
    arg0: watch.ElapsedMilliseconds);

Write("Results:");
foreach (int number in fibonacciNumbers)
{
    Write($" {number}");
}

static int Fibonacci(int term) =>
    term switch
    {
        1 => 0,
        2 => 1,
        _ => Fibonacci(term - 1) + Fibonacci(term - 2)
    };
```

## Using Windows

- 1. If you are using Windows, then right-click on the Windows Start button or press Ctrl + Alt + Delete, and then click on Task Manager.
- 2. At the bottom of the Task Manager window, click More details.
- 3. At the top of the Task Manager window, click on the Performance tab.
- 4. Right-click on the CPU Utilization graph, select Change graph to, and then select Logical processors.

## Using macOS

- 1. If you are using macOS, then launch Activity Monitor.
- 2. Navigate to View | Update Frequency Very often (1 sec).
- 3. To see the CPU graphs, navigate to Window | CPU History.

## For all operating systems

- 1. Rearrange your monitoring tool and your code editor so that they are side by side.
- 2. Wait for the CPUs to settle and then press Enter to start the stopwatch and run the query. The result should be a number of elapsed milliseconds, as shown in the following output:
- 3. In Program.cs, modify the query to make a call to the AsParallel extension method and to sort the resulting sequence because when processing in parallel the results can become misordered, as shown highlighted in the following code:

```
Press ENTER to start.
Calculating Fibonacci sequence up to 45. Please wait...
17,624 elapsed milliseconds.
Results: 0 1 1 2 3 5 8 13 21 34 55 89 144 233 377 610 987 1597 2584 4181
6765 10946 17711 28657 46368 75025 121393 196418 317811 514229 832040
1346269 2178309 3524578 5702887 9227465 14930352 24157817 39088169
63245986 102334155 165580141 267914296 433494437 701408733
```

The monitoring tool will probably show that one or two CPUs were used the most, alternating over time. Others may execute background tasks at the same time, such as the garbage collector, so the other CPUs or cores won't be completely flat, but the work is certainly not being evenly spread among all the possible CPUs or cores. Also, note that some of the logical processors are maxing out at 100%.

```
int[] fibonacciNumbers = numbers.AsParallel()
    .Select(number => Fibonacci(number))
    .OrderBy(number => number)
    .ToArray();
```

> **Good Practice:** Never call AsParallel at the end of a query. This does nothing. You must perform at least one operation after the call to AsParallel for that operation to be parallelized. .NET 6 introduces a code analyzer that will warn about this type of misuse.

- 4. Run the code, wait for CPU charts in your monitoring tool to settle, and then press *Enter* to start the stopwatch and run the query. This time, the application should complete in less time (although it might not be as less as you might hope for — managing those multiple threads takes extra effort!):

```
Press ENTER to start.
Calculating Fibonacci sequence up to 45. Please wait...
9,028 elapsed milliseconds.
Results: 0 1 1 2 3 5 8 13 21 34 55 89 144 233 377 610 987 1597 2584 4181
6765 10946 17711 28657 46368 75025 121393 196418 317811 514229 832040
1346269 2178309 3524578 5702887 9227465 14930352 24157817 39088169
63245986 102334155 165580141 267914296 433494437 701408733
```

- 5. The monitoring tool should show that all CPUs were used equally to execute the LINQ query, and note that none of the logical processors max out at 100% because the work is more evenly spread.

You will learn more about managing multiple threads in Chapter 12, *Improving Performance and Scalability Using Multitasking*.

## Creating your own LINQ extension methods

In Chapter 6, *Implementing Interfaces and Inheriting Classes*, you learned how to create your own extension methods. To create LINQ extension methods, all you must do is extend the IEnumerable<T> type.

> **Good Practice:** Put your own extension methods in a separate class library so that they can be easily deployed as their own assembly or NuGet package.

We will improve the Average extension method as an example. A well-educated school child will tell you that *average* can mean one of three things:

- **Mean:** Sum the numbers and divide by the count.
- **Mode:** The most common number.
- **Median:** The number in the middle of the numbers when ordered.

Microsoft's implementation of the Average extension method calculates the mean. We might want to define our own extension methods for Mode and Median:

- 1. In the LinqWithEFCore project, add a new class file named MyLinqExtensions.cs.
- 2. Modify the class, as shown in the following code:

```
namespace System.Linq; // extend Microsoft's namespace

public static class MyLinqExtensions
{
    // this is a chainable LINQ extension method
    public static IEnumerable<T> ProcessSequence<T>(
        this IEnumerable<T> sequence)
    {
        // you could do some processing here
        return sequence;
    }

    public static IQueryable<T> ProcessSequence<T>(
        this IQueryable<T> sequence)
    {
        // you could do some processing here
        return sequence;
    }

    // these are scalar LINQ extension methods
    public static int? Median(
        this IEnumerable<int?> sequence)
    {
        var ordered = sequence.OrderBy(item => item);
        int middlePosition = ordered.Count() / 2;
        return ordered.ElementAt(middlePosition);
    }

    public static int? Median<T>(
        this IEnumerable<T> sequence, Func<T, int?> selector)
    {
        return sequence.Select(selector).Median();
    }

    public static decimal? Median(
        this IEnumerable<decimal?> sequence)
    {
        var ordered = sequence.OrderBy(item => item);
        int middlePosition = ordered.Count() / 2;
        return ordered.ElementAt(middlePosition);
    }
}
```
