Without getting too hung up on the details of System.Object and System.ValueType, just understand that because a C# keyword (such as int) is simply shorthand notation for the corresponding system type (in this case, System.Int32), the following is perfectly legal syntax, given that System.Int32 (the C# int) eventually derives from System.Object and, therefore, can invoke any of its public members, as illustrated by this additional helper function:

```
static void ObjectFunctionality()
{
    Console.WriteLine("=> System.Object Functionality:");

    // A C# int is really a shorthand for System.Int32,
    // which inherits the following members from System.Object.
    Console.WriteLine("12.GetHashCode() = {0}", 12.GetHashCode());
    Console.WriteLine("12.Equals(23) = {0}", 12.Equals(23));
    Console.WriteLine("12.ToString() = {0}", 12.ToString());
    Console.WriteLine("12.GetType() = {0}", 12.GetType());
    Console.WriteLine();
}
```

If you were to call this method from within Main(), you would find the output shown here:

```
=> System.Object Functionality:

12.GetHashCode() = 12
12.Equals(23) = False
12.ToString() = 12
12.GetType() = System.Int32
```

# Understanding the Members of Numerical Data Types

To continue experimenting with the intrinsic C# data types, understand that the numerical types of .NET Core support MaxValue and MinValue properties that provide information regarding the range a given type can store. In addition to the MinValue/MaxValue properties, a given numerical system type may define further useful members. For example, the System.Double type allows you to obtain the values for epsilon and infinity (which might be of interest to those of you with a mathematical flare). To illustrate, consider the following helper function:

```
static void DataTypeFunctionality()
{
    Console.WriteLine("=> Data type Functionality:");

    Console.WriteLine("Max of int: {0}", int.MaxValue);
    Console.WriteLine("Min of int: {0}", int.MinValue);
    Console.WriteLine("Max of double: {0}", double.MaxValue);
    Console.WriteLine("Min of double: {0}", double.MinValue);
    Console.WriteLine("double.Epsilon: {0}", double.Epsilon);
    Console.WriteLine("double.PositiveInfinity: {0}",
        double.PositiveInfinity);
    Console.WriteLine("double.NegativeInfinity: {0}",
```