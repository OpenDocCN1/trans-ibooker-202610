```
Console.WriteLine("Name: {0}, Value: {0:D}",
    enumData.GetValue(i));
}
```

To test this new method, update your code to create variables of several enumeration types declared in the System namespace (as well as an EmpTypeEnum enumeration for good measure). The following code is an example:

```
Console.WriteLine("**** Fun with Enums *****");
...
EmpTypeEnum e2 = EmpType.Contractor;

// These types are enums in the System namespace.
DayOfWeek day = DayOfWeek.Monday;
ConsoleColor cc = ConsoleColor.Gray;

EvaluateEnum(e2);
EvaluateEnum(day);
EvaluateEnum(cc);
Console.ReadLine();
```

Some partial output is shown here:

```
=> Information about DayOfWeek

Underlying storage type: System.Int32
This enum has 7 members.
Name: Sunday, Value: 0
Name: Monday, Value: 1
Name: Tuesday, Value: 2
Name: Wednesday, Value: 3
Name: Thursday, Value: 4
Name: Friday, Value: 5
Name: Saturday, Value: 6
```

As you will see over the course of this text, enumerations are used extensively throughout the .NET Core base class libraries. When you make use of any enumeration, always remember that you can interact with the name-value pairs using the members of System.Enum.

# Using Enums, Flags, and Bitwise Operations

Bitwise operations provide a fast mechanism for operating on binary numbers at the bit level. Table 4-3 contains the C# bitwise operators, what they do, and an example of each.