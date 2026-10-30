
# Understanding Static Read-Only Fields

Unlike a constant field, read-only fields are not implicitly static. Thus, if you want to expose PI from the class level, you must explicitly use the static keyword. If you know the value of a static read-only field at compile time, the initial assignment looks similar to that of a constant (however, in this case, it would be easier to simply use the const keyword in the first place, as you are assigning the data field at the time of declaration).

```
class MyMathClass
{
    public static readonly double PI = 3.14;
}
```

```
//Program.cs
Console.WriteLine("***** Fun with Const *****");
Console.WriteLine("The value of PI is: {0}", MyMathClass.PI);
Console.ReadLine();
```

However, if the value of a static read-only field is not known until runtime, you must use a static constructor as described earlier in this chapter.

```
class MyMathClass
{
    public static readonly double PI;

    static MyMathClass()
    { PI = 3.14; }
}
```

## Understanding Partial Classes

When working with classes, it is important to understand the role of the C# partial keyword. The partial keyword allows for a single class to be partitioned across multiple code files. When you scaffold Entity Framework Core classes from a database, the created classes are all created as partial classes. This way, any code that you have written to augment those files is not overwritten, presuming your code is in separate class files marked with the partial keyword. Another reason is that maybe your class has grown over time into something difficult to manage, and as an intermediate step toward refactoring that class, you can split it up into partials.

In C#, you can partition a single class across multiple code files to isolate the boilerplate code from more readily useful (and complex) members. To illustrate where partial classes could be useful, open the EmployeeApp project you created previously in this chapter in Visual Studio and then open the Employee.cs file for editing. As you recall, this single file contains code of all aspects of the class.

```
class Employee
{
    // Field Data

    // Constructors

    // Methods

    // Properties
}
```

Using partial classes, you could choose to move (for example) the properties, constructors, and field data into a new file named Employee.Core.cs (the name of the file is irrelevant). The first step is to add the partial keyword to the current class definition and cut the code to be placed into the new file.

```
// Employee.cs
partial class Employee
{
    // Methods
    // Properties
}
```

Next, assuming you have inserted a new class file into your project, you can move the data fields and properties to the new file using a simple cut-and-paste operation. In addition, you must add the partial keyword to this aspect of the class definition. Here is an example:

```
// Employee.Core.cs
partial class Employee
{
    // Field data
    // Properties
}
```

Note Remember that each of the partial classes must be marked with the partial keyword!

After you compile the modified project, you should see no difference whatsoever. The whole idea of a partial class is realized only during design time. After the application has been compiled, there is just a single, unified class within the assembly. The only requirement when defining partial types is that the type’s name (Employee in this case) is identical and defined within the same .NET Core namespace.

## Using Records (New 9.0)

New in C# 9.0, record types are a special type of class. Records are reference types that provide synthesized methods to provide value semantics for equality. By default, record types are immutable by default. While you could essentially create an immutable class but using a combination of init-only setters and read-only properties, record types remove that extra work.

To start experimenting with records, create a new console application named FunWithRecords. Consider the following Car class, modified from the examples earlier in the chapter:

```
class Car
{
    public string Make { get; set; }
    public string Model { get; set; }
    public string Color { get; set; }

    public Car() {}

    public Car(string make, string model, string color)
{
    Make = make;
    Model = model;
    Color = color;
}
}
```

As you well know by now, once you create an instance of this class, you can change any of the properties at run time. If each instance needs to be immutable, you can change the property definitions to the following:

```
public string Make { get; init; }
public string Model { get; init; }
public string Color { get; init; }
```

To exercise this new class, the following code creates two instances of the Car class, one through object initialization and the other through the custom constructor. Update the Program.cs file to the following:

```
using System;
using FunWithRecords;

Console.WriteLine("Fun with Records!");

//Use object initialization
Car myCar = new Car
{
    Make = "Honda",
    Model = "Pilot",
    Color = "Blue"
};
Console.WriteLine("My car: ");
DisplayCarStats(myCar);
Console.WriteLine();
//Use the custom constructor
Car anotherMyCar = new Car("Honda", "Pilot", "Blue");
Console.WriteLine("Another variable for my car: ");
DisplayCarStats(anotherMyCar);
Console.WriteLine();

//Compile error if property is changed
//myCar.Color = "Red";

Console.ReadLine();

static void DisplayCarStats(Car c)
{
    Console.WriteLine("Car Make: {0}", c.Make);
    Console.WriteLine("Car Model: {0}", c.Model);
    Console.WriteLine("Car Color: {0}", c.Color);
}
```

As expected, both methods of object creation work, properties get displayed, and trying to change a property after construction raises a compilation error.

To create a Car record type, add a new file named (CarRecord.cs) to your project and add the following code:

```
record CarRecord
{
    public string Make { get; init; }
    public string Model { get; init; }
    public string Color { get; init; }

    public CarRecord () {}
    public CarRecord (string make, string model, string color)
    {
        Make = make;
        Model = model;
        Color = color;
    }
}
```

You can confirm that the behavior is the same as the Car class with init-only settings by running the following code in Program.cs:

```
Console.WriteLine("/***************** RECORDS ********************/");
//Use object initialization
CarRecord myCarRecord = new CarRecord
{
    Make = "Honda",
    Model = "Pilot",
    Color = "Blue"
};
Console.WriteLine("My car: ");
DisplayCarRecordStats(myCarRecord);
Console.WriteLine();

//Use the custom constructor
CarRecord anotherMyCarRecord = new CarRecord("Honda", "Pilot", "Blue");
Console.WriteLine("Another variable for my car: ");
Console.WriteLine(anotherMyCarRecord.ToString());
Console.WriteLine();

//Compile error if property is changed
//myCarRecord.Color = "Red";

Console.ReadLine();
```

While we have not covered equality (next section) or inheritance (next chapter) with records, this first look at records does not seem like much of a benefit. The current Car example includes all of the plumbing code that we have come to expect. With one notable difference on the output: the ToString() method is fancied up for record types, as shown in this following sample output:

/************** RECORDS ****************/
My car:
CarRecord { Make = Honda, Model = Pilot, Color = Blue }
Another variable for my car:
CarRecord { Make = Honda, Model = Pilot, Color = Blue }

But consider this updated definition for the Car record:

```
record CarRecord(string Make, string Model, string Color);
```

Referred to as a *positional record type*, the constructor defines the properties on the record, and all of the other plumbing code has been removed. There are three considerations when using this syntax: the first is that you cannot use object initialization of record types using the compact definition syntax, the second is that the record must be constructed with the properties in the correct position, and the third is that the casing of the properties in the constructor is directly translated to the casing of the properties on the record type.

## Understanding Equality with Record Types

In the Car class example, the two Car instances were created with the exact same data. One *might* think that these two classes are equal, as the following line of code tests:

```
Console.WriteLine($"Cars are the same? {myCar.Equals(anotherMyCar)}");
```

However, that they are not equal. Recall that record types are a specialized type of class, and classes are *reference types*. For two reference types to be equal, they have to point to the same object in memory. As a further test, check to see if the two Car objects point to the same object:

```
Console.WriteLine($"Cars are the same reference? {ReferenceEquals(myCar, anotherMyCar)}");
```

Running the program again produces this (abbreviated) result:

```
Cars are the same? False
CarRecords are the same? False
```

Record types behave differently. Record types implicitly override Equals, ==, and !=, and they produce results as if the instances are value types. Consider the following code and the subsequent results:

```
Console.WriteLine($"CarRecords are the same? {myCarRecord.Equals(anotherMyCarRecord)}");
Console.WriteLine($"CarRecords are the same reference? {ReferenceEquals(myCarRecord,anotherMyCarRecord)}");
Console.WriteLine($"CarRecords are the same? {myCarRecord == anotherMyCarRecord}");
Console.WriteLine($"CarRecords are not the same? {myCarRecord != anotherMyCarRecord}");
```
