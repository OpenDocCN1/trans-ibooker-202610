+   1. In the PacktLibraryModern project/folder, add a new file named Records.cs.
+   2. In Records.cs, define an immutable person class, as shown in the following code:
+   3. In Program.cs, add statements to instantiate a new immutable person and then try to change one of its properties, as shown in the following code:
+   4. Compile the console app and note the compile error, as shown in the following output:
+   5. Comment out the attempt to set the FirstName property after instantiation.

```
```csharp
namespace Packt.Shared; // C# 10 file-scoped namespace

public class ImmutablePerson
{
    public string? FirstName { get; init; }
    public string? LastName { get; init; }
}
```
```

```
```csharp
ImmutablePerson jeff = new()
{
    FirstName = "Jeff",
    LastName = "Winger"
};

jeff.FirstName = "Geoff";
```
```

```
```
Program.cs(254,7): error CS8852: Init-only property or indexer 'ImmutablePerson.FirstName' can only be assigned in an object initializer, or on 'this' or 'base' in an instance constructor or an 'init' accessor.
[/Users/markjprice/Code/Chapter05/PeopleApp/PeopleApp.csproj]
```
```

# Understanding records

Init-only properties provide some immutability to C#. You can take the concept further by using records. These are defined by using the record keyword instead of the class keyword. That can make the whole object immutable, and it acts like a value when compared. We will discuss equality and comparisons of classes, records, and value types in more detail in Chapter 6, Implementing Interfaces and Inheriting Classes.

Records should not have any state (properties and fields) that changes after instantiation. Instead, the idea is that you create new records from existing ones with any changed state. This is called non-destructive mutation. To do this, C# 9 introduced the with keyword: