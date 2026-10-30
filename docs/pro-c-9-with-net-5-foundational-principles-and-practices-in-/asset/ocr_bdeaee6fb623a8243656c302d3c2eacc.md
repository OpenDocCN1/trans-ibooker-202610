These are two important considerations for tuples:

+   - The fields are not validated.
+   - You cannot define your own methods.

They are really designed to just be a lightweight data transport mechanism.

# Getting Started with Tuples

Enough theory. Let’s write some code! To create a tuple, simply enclose the values to be assigned to the tuple in parentheses, as follows:

```
("a", 5, "c")
```

Notice that they do not all have to be the same data type. The parenthetical construct is also used to assign the tuple to a variable (or you can use the var keyword and the compiler will assign the data types for you). To assign the previous example to a variable, the following two lines achieve the same thing. The values variable will be a tuple with two string properties and an int property sandwiched in between.

```
(string, int, string) values = ("a", 5, "c");
var values = ("a", 5, "c");
```

By default, the compiler assigns each property the name ItemX, where X represents the one-based position in the tuple. For the previous example, the property names are Item1, Item2, and Item3. Accessing them is done as follows:

```
Console.WriteLine($"First item: {values.Item1}");
Console.WriteLine($"Second item: {values.Item2}");
Console.WriteLine($"Third item: {values.Item3}");
```

Specific names can also be added to each property in the tuple on either the right side or the left side of the statement. While it is not a compiler error to assign names on both sides of the statement, if you do, the right side will be ignored, and only the left-side names are used. The following two lines of code show setting the names on the left and the right to achieve the same end:

```
(string FirstLetter, int TheNumber, string SecondLetter) valuesWithNames = ("a", 5, "c");
var valuesWithNames2 = (FirstLetter: "a", TheNumber: 5, SecondLetter: "c");
```

Now the properties on the tuple can be accessed using the field names as well as the ItemX notation, as shown in the following code:

```
Console.WriteLine($"First item: {valuesWithNames.FirstLetter}");
Console.WriteLine($"Second item: {valuesWithNames.TheNumber}");
Console.WriteLine($"Third item: {valuesWithNames.SecondLetter}");
//Using the item notation still works!
Console.WriteLine($"First item: {valuesWithNames.Item1}");
Console.WriteLine($"Second item: {valuesWithNames.Item2}");
Console.WriteLine($"Third item: {valuesWithNames.Item3}");
```