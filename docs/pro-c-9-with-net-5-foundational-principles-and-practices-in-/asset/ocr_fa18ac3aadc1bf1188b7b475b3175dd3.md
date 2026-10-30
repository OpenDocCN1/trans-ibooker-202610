# Using the foreach Loop

The C# foreach keyword allows you to iterate over all items in a container without the need to test for an upper limit. Unlike a for loop, however, the foreach loop will walk the container only in a linear (n+1) fashion (thus, you cannot go backward through the container, skip every third element, or whatnot).

However, when you simply need to walk a collection item by item, the foreach loop is the perfect choice. Here are two examples using foreach—one to traverse an array of strings and the other to traverse an array of integers. Notice that the data type before the in keyword represents the type of data in the container.

```
// Iterate array items using foreach.
static void ForEachLoopExample()
{
    string[] carTypes = {"Ford", "BMW", "Yugo", "Honda"};
    foreach (string c in carTypes)
    {
        Console.WriteLine(c);
    }

    int[] myInts = { 10, 20, 30, 40 };
    foreach (int i in myInts)
    {
        Console.WriteLine(i);
    }
}
```

The item after the in keyword can be a simple array (seen here) or, more specifically, any class implementing the IEnumerable interface. As you will see in Chapter 10, the .NET Core base class libraries ship with a number of collections that contain implementations of common abstract data types (ADTs). Any of these items (such as the generic List<T>) can be used within a foreach loop.

# Using Implicit Typing Within foreach Constructs

It is also possible to use implicit typing within a foreach looping construct. As you would expect, the compiler will correctly infer the correct "type of type." Recall the LINQ example method shown earlier in this chapter. Given that you do not know the exact underlying data type of the subset variable, you can iterate over the result set using implicit typing. Make sure to add the following using statement to the top of the file:

```
using System.Linq;
static void LinqQueryOverInts()
{
    int[] numbers = { 10, 20, 30, 40, 1, 2, 3, 8 };

    // LINQ query!
    var subset = from i in numbers where i < 10 select i;
    Console.Write("Values in subset: ");

    foreach (var i in subset)
    {
        Console.Write("{0} ", i);
    }
}
```