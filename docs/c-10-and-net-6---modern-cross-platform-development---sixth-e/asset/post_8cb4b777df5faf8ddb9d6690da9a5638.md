
```csharp
public static class Recorder
{
    private static Stopwatch timer = new();

    private static long bytesPhysicalBefore = 0;
    private static long bytesVirtualBefore = 0;

    public static void Start()
    {
        // force two garbage collections to release memory that is
        // no longer referenced but has not been released yet
        GC.Collect();
        GC.WaitForPendingFinalizers();
        GC.Collect();

        // store the current physical and virtual memory use
        bytesPhysicalBefore = GetCurrentProcess().WorkingSet64;
        bytesVirtualBefore = GetCurrentProcess().VirtualMemorySize64;
        timer.Restart();
    }

    public static void Stop()
    {
        timer.Stop();
        long bytesPhysicalAfter =
            GetCurrentProcess().WorkingSet64;

        long bytesVirtualAfter =
            GetCurrentProcess().VirtualMemorySize64;

        WriteLine("{0:N0} physical bytes used.",
            bytesPhysicalAfter - bytesPhysicalBefore);

        WriteLine("{0:N0} virtual bytes used.",
            bytesVirtualAfter - bytesVirtualBefore);

        WriteLine("{0} time span elapsed.", timer.Elapsed);

        WriteLine("{0:N0} total milliseconds elapsed.",
            timer.ElapsedMilliseconds);
    }
}
```

The Start method of the Recorder class uses the GC type (garbage collector) to ensure that any currently allocated but not referenced memory is collected before recording the amount of used memory. This is an advanced technique that you should almost never use in application code.

- 2. In Program.cs, write statements to start and stop the Recorder while generating an array of 10,000 integers, as shown in the following code:

```csharp
using Packt.Shared; // Recorder

using static System.Console;

WriteLine("Processing. Please wait...");
Recorder.Start();

// simulate a process that requires some memory resources...
int[] largeArrayOfInts = Enumerable.Range(
    start: 1, count: 10_000).ToArray();

// ...and takes some time to complete
Thread.Sleep(new Random().Next(5, 10) * 1000);

Recorder.Stop();
```

- 3. Run the code and view the result, as shown in the following output:

```
Processing. Please wait...
655,360 physical bytes used.
536,576 virtual bytes used.
00:00:09.0038702 time span elapsed.
9,003 total milliseconds elapsed.
```

Remember that the time elapsed is randomly between 5 and 10 seconds. Your results will vary. For example, when run on my Mac mini M1, less physical memory but more virtual memory was used, as shown in the following output:

```
Processing. Please wait...
294,912 physical bytes used.
10,485,760 virtual bytes used.
00:00:06.0074221 time span elapsed.
6,007 total milliseconds elapsed.
```

## Measuring the efficiency of processing strings

Now that you've seen how the Stopwatch and Process types can be used to monitor your code, we will use them to evaluate the best way to process string variables.

- 1. In Program.cs, comment out the previous statements by wrapping them in multi-line comment characters: /* */.
- 2. Write statements to create an array of 50,000 int variables and then concatenate them with commas as separators using a string and StringBuilder class, as shown in the following code:

```csharp
int[] numbers = Enumerable.Range(
    start: 1, count: 50_000).ToArray();

WriteLine("Using string with +");
Recorder.Start();
string s = string.Empty; // i.e. ""
for (int i = 0; i < numbers.Length; i++)
{
    s += numbers[i] + ", ";
}
Recorder.Stop();

WriteLine("Using StringBuilder");
Recorder.Start();
System.Text.StringBuilder builder = new();
for (int i = 0; i < numbers.Length; i++)
{
    builder.Append(numbers[i]);
    builder.Append(", ");
}
Recorder.Stop();
```

- 3. Run the code and view the result, as shown in the following output:

```
Using string with +
14,883,072 physical bytes used.
3,609,728 virtual bytes used.
00:00:01.6220879 time span elapsed.
1,622 total milliseconds elapsed.
Using StringBuilder
12,288 physical bytes used.
0 virtual bytes used.
00:00:00.0006038 time span elapsed.
0 total milliseconds elapsed.
```

We can summarize the results as follows:

- - The string class with the + operator used about 14 MB of physical memory, 1.5 MB of virtual memory, and took 1.5 seconds.
- - The StringBuilder class used 12 KB of physical memory, zero virtual memory, and took less than 1 millisecond.

In this scenario, StringBuilder is more than 1,000 times faster and about 10,000 times more memory efficient when concatenating text! This is because string concatenation creates a new string each time you use it because string values are immutable so they can be safely pooled for reuse. StringBuilder creates a single buffer while it appends more characters.

> **Good Practice**: Avoid using the String.Concat method or the + operator inside loops. Use StringBuilder instead.

Now that you've learned how to measure the performance and resource efficiency of your code using types built into .NET, let's learn about a NuGet package that provides more sophisticated performance measurements.

## Monitoring performance and memory using Benchmark.NET

There is a popular benchmarking NuGet package for .NET that Microsoft uses in its blog posts about performance improvements, so it is good for .NET developers to know how it works and use it for their own performance testing. Let's see how we could use it to compare performance between string concatenation and StringBuilder:

- 1. Use your preferred code editor to add a new console app to the Chapter12 solution/workspace named Benchmarking.
- 2. In Visual Studio Code, select Benchmarking as the active OmniSharp project.
- 3. Add a package reference to Benchmark.NET, remembering that you can find out the latest version and use that instead of the version I used, as shown in the following markup:
- 4. Build the project to restore packages.
- 5. In Program.cs, delete the existing statements and then import the namespace for running benchmarks, as shown in the following code:
- 6. Add a new class file named StringBenchmarks.cs.
- 7. In StringBenchmarks.cs, add statements to define a class with methods for each benchmark you want to run, in this case, two methods that both combine twenty numbers comma-separated using either string concatenation or StringBuilder, as shown in the following code:

```xml
<ItemGroup>
  <PackageReference Include="BenchmarkDotNet" Version="0.13.1" />
</ItemGroup>
```

```csharp
using BenchmarkDotNet.Running;
```

```csharp
using BenchmarkDotNet.Attributes; // [Benchmark]

public class StringBenchmarks
{
    int[] numbers;

    public StringBenchmarks()
    {
        numbers = Enumerable.Range(
            start: 1, count: 20).ToArray();
    }

    [Benchmark(Baseline = true)]
    public string StringConcatenationTest()
    {
        string s = string.Empty; // e.g. ""
        for (int i = 0; i < numbers.Length; i++)
        {
            s += numbers[i] + ", ";
        }
        return s;
    }

    [Benchmark]
    public string StringBuilderTest()
    {
        System.Text.StringBuilder builder = new();
        for (int i = 0; i < numbers.Length; i++)
        {
            builder.Append(numbers[i]);
            builder.Append(", ");
        }
        return builder.ToString();
    }
}
```

- 8. In Program.cs, add a statement to run the benchmarks, as shown in the following code:

```csharp
BenchmarkRunner.Run<StringBenchmarks>();
```

- 9. In Visual Studio 2022, in the toolbar, set Solution Configurations to Release.
- 10. In Visual Studio Code, in a terminal, use the dotnet run --configuration Release command.
- 11. Run the console app and note the results, including some artifacts like report files, and the most important, a summary table that shows that string concatenation took a mean of 412.990 ns and StringBuilder took a mean of 275.082 ns, as shown in the following partial output and in Figure 12.1:

```
// ***** BenchmarkRunner: Finish *****

// * Export *
BenchmarkDotNet.Artifacts\results\StringBenchmarks-report.csv
BenchmarkDotNet.Artifacts\results\StringBenchmarks-report-github.md
```
