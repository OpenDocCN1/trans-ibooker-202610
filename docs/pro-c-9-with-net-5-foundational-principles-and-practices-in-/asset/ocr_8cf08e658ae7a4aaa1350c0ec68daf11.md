```
Console.ReadLine();

static IEnumerable<string> GetStringSubset()
{
    string[] colors = {"Light Red", "Green", "Yellow", "Dark Red", "Red", "Purple"};
    // Note subset is an IEnumerable<string>-compatible object.
    IEnumerable<string> theRedColors = from c in colors where c.Contains("Red") select c;
    return theRedColors;
}
```

The results are as expected.

```
Light Red
Dark Red
Red
```

# Returning LINQ Results via Immediate Execution

This example works as expected, only because the return value of GetStringSubset() and the LINQ query within this method has been strongly typed. If you used the var keyword to define the subset variable, it would be permissible to return the value only if the method is still prototyped to return IEnumerable<string> (and if the implicitly typed local variable is in fact compatible with the specified return type).

Because it is a bit inconvenient to operate on IEnumerable<T>, you could use immediate execution. For example, rather than returning IEnumerable<string>, you could simply return a string[], if you transform the sequence to a strongly typed array. Consider this new method of the Program class, which does this very thing:

```
static string[] GetStringSubsetAsArray()
{
    string[] colors = {"Light Red", "Green", "Yellow", "Dark Red", "Red", "Purple"};

    var theRedColors = from c in colors where c.Contains("Red") select c;

    // Map results into an array.
    return theRedColors.ToArray();
}
```

With this, the caller can be blissfully unaware that their result came from a LINQ query and simply work with the array of strings as expected. Here's an example:

```
foreach (string item in GetStringSubsetAsArray())
{
    Console.WriteLine(item);
}
```

Immediate execution is also critical when attempting to return to the caller the results of a LINQ projection. You'll examine this topic a bit later in the chapter. Next up, let's look at how to apply LINQ queries to generic and nongeneric collection objects.