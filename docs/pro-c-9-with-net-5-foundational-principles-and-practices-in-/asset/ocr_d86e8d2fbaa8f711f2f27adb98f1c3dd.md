# Anonymous Types

The final C# language feature I’d like to quickly review is that of anonymous types, which were explored in Chapter 11. This feature can be used to quickly model the “shape” of data by allowing the compiler to generate a new class definition at compile time, based on a supplied set of name-value pairs. Recall that this type will be composed using value-based semantics, and each virtual method of System.Object will be overridden accordingly. To define an anonymous type, declare an implicitly typed variable and specify the data’s shape using object initialization syntax.

```
// Make an anonymous type that is composed of another.
var purchaseItem = new {
    TimeBought = DateTime.Now,
    ItemBought =
        new {Color = "Red", Make = "Saab", CurrentSpeed = 55},
    Price = 34.000};
```

LINQ makes frequent use of anonymous types when you want to project new forms of data on the fly. For example, assume you have a collection of Person objects and want to use LINQ to obtain information on the age and Social Security number of each. Using a LINQ projection, you can allow the compiler to generate a new anonymous type that contains your information.

# Understanding the Role of LINQ

That wraps up the quick review of the C# language features that allow LINQ to work its magic. However, why have LINQ in the first place? Well, as software developers, it is hard to deny that a significant amount of programming time is spent obtaining and manipulating data. When speaking of “data,” it is easy to immediately envision information contained within relational databases. However, another popular location for data is within XML documents or simple text files.

Data can be found in numerous places beyond these two common homes for information. For instance, say you have an array or generic List<T> type containing 300 integers and you want to obtain a subset that meets a given criterion (e.g., only the odd or even members in the container, only prime numbers, only nonrepeating numbers greater than 50). Or perhaps you are making use of the reflection APIs and need to obtain only metadata descriptions for each class deriving from a parent class within an array of Types. Indeed, data is everywhere.

Prior to .NET 3.5, interacting with a flavor of data required programmers to use very diverse APIs. Consider, for example, Table 13-1, which illustrates several common APIs used to access various types of data (I’m sure you can think of many other examples).

Table 13-1. Ways to Manipulate Various Types of Data

| The Data You Want | How to Obtain It |
|---|---|
| Relational data | System.Data.dll, System.Data.SqlClient.dll, etc. |
| XML document data | System.Xml.dll |
| Metadata tables | The System.Reflection namespace |
| Collections of objects | System.Array and the System.Collections/System.Collections.Generic namespaces |