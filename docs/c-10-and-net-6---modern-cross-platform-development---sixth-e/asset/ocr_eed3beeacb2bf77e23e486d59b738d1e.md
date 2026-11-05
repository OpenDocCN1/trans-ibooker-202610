![](bbox=[0.15, 0.08, 0.22, 0.16])

**Good Practice:** Before calling the Dispose method, check that the object is not null.

# Simplifying disposal by using the using statement

You can simplify the code that needs to check for a null object and then call its Dispose method by using the using statement. Generally, I would recommend using using rather than manually calling Dispose unless you need a greater level of control.

Confusingly, there are two uses for the using keyword: importing a namespace and generating a finally statement that calls Dispose on an object that implements IDisposable.

The compiler changes a using statement block into a try-finally statement without a catch statement. You can use nested try statements; so, if you do want to catch any exceptions, you can, as shown in the following code example:

```
using (FileStream file2 = File.OpenWrite(
    Path.Combine(path, "file2.txt")))
{
    using (StreamWriter writer2 = new StreamWriter(file2))
    {
        try
        {
            writer2.WriteLine("Welcome, .NET!");
        }
        catch(Exception ex)
        {
            WriteLine($"{ex.GetType()} says {ex.Message}");
        }
    } // automatically calls Dispose if the object is not null
} // automatically calls Dispose if the object is not null
```

You can even simplify the code further by not explicitly specifying the braces and indentation for the using statements, as shown in the following code:

```
using FileStream file2 = File.OpenWrite(
    Path.Combine(path, "file2.txt"));

using StreamWriter writer2 = new(file2);

try
{
    writer2.WriteLine("Welcome, .NET!");
}
```