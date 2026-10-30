
## Parsing Values from String Data

The .NET Core data types provide the ability to generate a variable of their underlying type given a textual equivalent (e.g., parsing). This technique can be extremely helpful when you want to convert some user input data (such as a selection from a GUI-based, drop-down list box) into a numerical value. Consider the following parsing logic within a method named ParseFromStrings():

```
static void ParseFromStrings()
{
    Console.WriteLine("=> Data type parsing:");
    bool b = bool.Parse("True");
    Console.WriteLine("Value of b: {0}", b);
    double d = double.Parse("99.884");
    Console.WriteLine("Value of d: {0}", d);
    int i = int.Parse("8");
    Console.WriteLine("Value of i: {0}", i);
    char c = Char.Parse("w");
    Console.WriteLine("Value of c: {0}", c);
    Console.WriteLine();
}
```

## Using TryParse to Parse Values from String Data

One issue with the preceding code is that an exception will be thrown if the string cannot be cleanly converted to the correct data type. For example, the following will fail at runtime:

```
bool b = bool.Parse("Hello");
```

One solution is to wrap each call to Parse() in a try-catch block (exception handling is covered in detail in Chapter 7), which can add a lot of code, or use a TryParse() statement. The TryParse() statement takes an out parameter (the out modifier is covered in detail in Chapter 4) and returns a bool if the parsing was successful. Create a new method named ParseFromStringWithTryParse() and add the following code:

```
static void ParseFromStringsWithTryParse()
{
    Console.WriteLine("=> Data type parsing with TryParse:");
    if (bool.TryParse("True", out bool b))
    {
        Console.WriteLine("Value of b: {0}", b);
    }
    else
    {
        Console.WriteLine("Default value of b: {0}", b);
    }
    string value = "Hello";
    if (double.TryParse(value, out double d))
    {
        Console.WriteLine("Value of d: {0}", d);
    }
    else
{
    Console.WriteLine("Failed to convert the input {0}) to a double and the variable was assigned the default {1}", value,d);
}
Console.WriteLine();
}
```

If you are new to programming and do not know how if/else statements work, they are covered later in this chapter in detail. The important item to note from the preceding example is that if a string can be converted to the requested data type, the TryParse() method returns true and assigns the parsed value to the variable passed into the method. If the value cannot be parsed, the variable is assigned its default value, and the TryParse() method returns false.

## Using System.DateTime and System.TimeSpan

The System namespace defines a few useful data types for which there are no C# keywords, such as the DateTime and TimeSpan structures. (I will leave the investigation of System.Void, as shown in Figure 3-2, to interested readers.)

The DateTime type contains data that represents a specific date (month, day, year) and time value, both of which may be formatted in a variety of ways using the supplied members. The TimeSpan structure allows you to easily define and transform units of time using various members.

```
static void UseDatesAndTimes()
{
    Console.WriteLine("=> Dates and Times:");

    // This constructor takes (year, month, day).
    DateTime dt = new DateTime(2015, 10, 17);

    // What day of the month is this?
    Console.WriteLine("The day of {0} is {1}", dt.Date, dt.DayOfWeek);

    // Month is now December.
    dt = dt.AddMonths(2);
    Console.WriteLine("Daylight savings: {0}", dt.IsDaylightSavingTime());

    // This constructor takes (hours, minutes, seconds).
    TimeSpan ts = new TimeSpan(4, 30, 0);
    Console.WriteLine(ts);

    // Subtract 15 minutes from the current TimeSpan and
    // print the result.
    Console.WriteLine(ts.Subtract(new TimeSpan(0, 15, 0)));
}
```

## Working with the System.Numerics Namespace

The System.Numerics namespace defines a structure named BigInteger. As its name implies, the BigInteger data type can be used when you need to represent humongous numerical values, which are not constrained by a fixed upper or lower limit.

Note The System.Numerics namespace defines a second structure named Complex, which allows you to model mathematically complex numerical data (e.g., imaginary units, real data, hyperbolic tangents). Consult the .NET Core documentation if you are interested.

While many of your .NET Core applications might never need to make use of the BigInteger structure, if you do find the need to define a massive numerical value, your first step is to add the following using directive to the file:

```
// BigInteger lives here!
using System.Numerics;
```

At this point, you can create a BigInteger variable using the new operator. Within the constructor, you can specify a numerical value, including floating-point data. However, C# implicitly types non-floating-point numbers as an int and floating-point numbers as a double. How, then, can you set BigInteger to a massive value while not overflowing the default data types used for raw numerical values?

The simplest approach is to establish the massive numerical value as a text literal, which can be converted into a BigInteger variable via the static Parse() method. If required, you can also pass in a byte array directly to the constructor of the BigInteger class.

Note After you assign a value to a BigInteger variable, you cannot change it, as the data is immutable. However, the BigInteger class defines a number of members that will return new BigInteger objects based on your data modifications (such as the static Multiply() method used in the following code sample).

In any case, after you have defined a BigInteger variable, you will find this class defines similar members as other intrinsic C# data types (e.g., float, int). In addition, the BigInteger class defines several static members that allow you to apply basic mathematical expressions (such as adding and multiplying) to BigInteger variables. Here is an example of working with the BigInteger class:

```
static void UseBigInteger()
{
    Console.WriteLine("=> Use BigInteger:");
    BigInteger biggy =
        BigInteger.Parse("999999999999999999999999999999999999999999999");
    Console.WriteLine("Value of biggy is {0}", biggy);
    Console.WriteLine("Is biggy an even value?: {0}", biggy.IsEven);
    Console.WriteLine("Is biggy a power of two?: {0}", biggy.IsPowerOfTwo);
    BigInteger reallyBig = BigInteger.Multiply(biggy,
        BigInteger.Parse("88888888888888888888888888888888888888888888"));
    Console.WriteLine("Value of reallyBig is {0}", reallyBig);
}
```

It is also important to note that the BigInteger data type responds to C#’s intrinsic mathematical operators, such as +, -, and *. Therefore, rather than calling BigInteger.Multiply() to multiply two huge numbers, you could author the following code:

```
BigInteger reallyBig2 = biggy * reallyBig;
```

At this point, I hope you understand that the C# keywords representing basic data types have a corresponding type in the .NET Core base class libraries, each of which exposes a fixed functionality. While I have not detailed each member of these data types, you are in a great position to dig into the details as you see fit. Be sure to consult the .NET Core documentation for full details regarding the various .NET data types—you will likely be surprised at the amount of built-in functionality.

## Using Digit Separators (New 7.0)

Sometimes when assigning large numbers to a numeric variable, there are more digits than the eye can keep track of. C# 7.0 introduced the underscore (_) as a digit separator (for integer, long, decimal, double data, or hex types). C# 7.2 allows for hex values (and the new binary literal, covered next, to start with an underscore, after the opening declaration). Here is an example of using the new digit separator:

```
static void DigitSeparators()
{
    Console.WriteLine("=> Use Digit Separators:");
    Console.Write("Integer:");
    Console.WriteLine(123_456);
    Console.Write("Long:");
    Console.WriteLine(123_456_789L);
    Console.Write("Float:");
    Console.WriteLine(123_456.1234F);
    Console.Write("Double:");
    Console.WriteLine(123_456.12);
    Console.Write("Decimal:");
    Console.WriteLine(123_456.12M);
    //Updated in 7.2, Hex can begin with _
    Console.Write("Hex:");
    Console.WriteLine(0x_00_00_FF);
}
```

## Using Binary Literals (New 7.0/7.2)

C# 7.0 introduces a new literal for binary values, for example, for creating bit masks. The new digit separator works with binary literals, and C# 7.2 allows for binary and hex numbers to start with an underscore. Now, binary numbers can be written as you would expect. Here is an example:

0b_0001_0000

Here is a method that shows using the new literals with the digit separator:

```
static void BinaryLiterals()
{
    //Updated in 7.2, Binary can begin with _
    Console.WriteLine("=> Use Binary Literals:");
    Console.WriteLine("Sixteen: {0}",0b_0001_0000);
    Console.WriteLine("Thirty Two: {0}",0b_0010_0000);
    Console.WriteLine("Sixty Four: {0}",0b_0100_0000);
}
```
