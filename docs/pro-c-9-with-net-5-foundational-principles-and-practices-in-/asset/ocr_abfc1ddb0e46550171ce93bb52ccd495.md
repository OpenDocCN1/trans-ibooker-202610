# Understanding Tuples as Method Return Values

Earlier in this chapter, out parameters were used to return more than one value from a method call. There are additional ways to do this, such as creating a class or structure specifically to return the values. But if this class or struct is only to be used as a data transport for one method, that is extra work and extra code that does not need to be developed. Tuples are perfectly suited for this task, are lightweight, and are easy to declare and use.

This is one of the examples from the out parameter section. It returns three values but requires three parameters passed in as transport mechanisms for the calling code.

```
static void FillTheseValues(out int a, out string b, out bool c)
{
    a = 9;
    b = "Enjoy your string.";
    c = true;
}
```

By using a tuple, you can remove the parameters and still get the three values back.

```
static (int a,string b,bool c) FillTheseValues()
{
    return (9,"Enjoy your string.",true);
}
```

Calling this method is as simple as calling any other method.

```
var samples = FillTheseValues();
Console.WriteLine($"Int is: {samples.a}");
Console.WriteLine($"String is: {samples.b}");
Console.WriteLine($"Boolean is: {samples.c}");
```

Perhaps a better example is deconstructing a full name into its individual parts (first, middle, last). The following code takes in a full name and returns a tuple with the different parts:

```
static (string first, string middle, string last) SplitNames(string fullName)
{
    //do what is needed to split the name apart
    return ("Philip", "F", "Japikse");
}
```

# Understanding Discards with Tuples

Following up on the SplitNames() example, suppose you know that you need only the first and last names and do not care about the middle. By providing variable names for the values you want returned and filling in the unneeded values with an underscore (_) placeholder, you can refine the return value like this:

```
var (first, _, last) = SplitNames("Philip F Japikse");
Console.WriteLine($"{first}:{last}");
```

The middle name value of the tuple is discarded.