```
```csharp
public static class AddOperations
{
    // Overloaded Add() method.
    public static int Add(int x, int y)
    {
        return x + y;
    }
    public static double Add(double x, double y)
    {
        return x + y;
    }
    public static long Add(long x, long y)
    {
        return x + y;
    }
}
```
```

Replace the code in Program.cs with the following:

```
```csharp
using System;
using FunWithMethodOverloading;
using static FunWithMethodOverloading.AddOperations;

Console.WriteLine("***** Fun with Method Overloading *****\n");

// Calls int version of Add()
Console.WriteLine(Add(10, 10));

// Calls long version of Add() (using the new digit separator)
Console.WriteLine(Add(900_000_000_000, 900_000_000_000));

// Calls double version of Add()
Console.WriteLine(Add(4.3, 4.4));

Console.ReadLine();
```
```

> **Note**  The `using static` statement will be covered in Chapter 5. For now, consider it a keyboard shortcut for using methods containing a static class named `AddOperations` in the `FunWithMethodOverloading` namespace.

The top-level statements called three different versions of the Add method, each using a different data type. Both Visual Studio and Visual Studio Code help when calling overloaded methods to boot. When you type in the name of an overloaded method (such as your good friend `Console.WriteLine()`), IntelliSense will list each version of the method in question. Note that you can cycle through each version of an overloaded method using the up and down arrow keys, as indicated in Figure 4-1.