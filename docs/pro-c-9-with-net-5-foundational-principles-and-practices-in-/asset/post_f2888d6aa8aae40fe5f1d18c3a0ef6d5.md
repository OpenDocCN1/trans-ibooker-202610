
Here is the output:

```
***** Fun with Extension Methods *****
Int32 lives here: => System.Private.CoreLib

DataSet lives here: => System.Data.Common

Value of myInt: 12345678
Reversed digits of myInt: 87654321
```

## Importing Extension Methods

When you define a class containing extension methods, it will no doubt be defined within a namespace. If this namespace is different from the namespace using the extension methods, you will need to make use of the expected C# using keyword. When you do, your code file has access to all extension methods for the type being extended. This is important to remember because if you do not explicitly import the correct namespace, the extension methods are not available for that C# code file.

In effect, although it can appear on the surface that extension methods are global in nature, they are in fact limited to the namespaces that define them or the namespaces that import them. Recall that you wrapped the MyExtensions class into a namespace named MyExtensionMethods, as follows:

```
namespace MyExtensionMethods
{
    static class MyExtensions
    {
        ...
    }
}
```

To use the extension methods in the class, you need to explicitly import the MyExtensionMethods namespace, as we did in the top-level statements used to exercise the examples.

## Extending Types Implementing Specific Interfaces

At this point, you have seen how to extend classes (and, indirectly, structures that follow the same syntax) with new functionality via extension methods. It is also possible to define an extension method that can only extend a class or structure that implements the correct interface. For example, you could say something to the effect of “If a class or structure implements IEnumerable<T>, then that type gets the following new members.” Of course, it is possible to demand that a type support any interface at all, including your own custom interfaces.

To illustrate, create a new Console Application project named InterfaceExtensions. The goal here is to add a new method to any type that implements IEnumerable, which would include any array and many nongeneric collection classes (recall from Chapter 10 that the generic IEnumerable<T> interface extends the nongeneric IEnumerable interface). Add the following extension class to your new project:

```
using System;

namespace InterfaceExtensions
{
    static class AnnoyingExtensions
    {
        public static void PrintDataAndBeep(
            this System.Collections.IEnumerable iterator)
        {
            foreach (var item in iterator)
            {
                Console.WriteLine(item);
                Console.Beep();
            }
        }
    }
}
```

Given that the PrintDataAndBeep() method can be used by any class or structure that implements IEnumerable, you could test via the following code:

```
using System;
using System.Collections.Generic;
using InterfaceExtensions;

Console.WriteLine("***** Extending Interface Compatible Types *****
");

// System.Array implements IEnumerable!
string[] data =
    { "Wow", "this", "is", "sort", "of", "annoying",
      "but", "in", "a", "weird", "way", "fun!"};
data.PrintDataAndBeep();

Console.WriteLine();

// List<T> implements IEnumerable!
List<int> myInts = new List<int>() {10, 15, 20};
myInts.PrintDataAndBeep();

Console.ReadLine();
```

That wraps up your examination of C# extension methods. Remember that this language feature can be useful whenever you want to extend the functionality of a type but do not want to subclass (or cannot subclass if the type is sealed), for the purposes of polymorphism. As you will see later in the text, extension methods play a key role for LINQ APIs. In fact, you will see that under the LINQ APIs, one of the most common items being extended is a class or structure implementing (surprise!) the generic version of IEnumerable.

## Extension Method GetEnumerator Support (New 9.0)

Prior to C# 9.0, to use foreach on a class, the GetEnumerator() method had to be defined on that class directly. With C# 9.0, the foreach method will examine extension methods on the class and, if a GetEnumerator() method is found, will use that method to get the IEnumerator for that class. To see this in action, add a new Console application named ForEachWithExtensionMethods, and add simplified versions of the Car and Garage classes from Chapter 8.

```
//Car.cs
using System;

namespace ForEachWithExtensionMethods
{
    class Car
    {
        // Car properties.
        public int CurrentSpeed {get; set;} = 0;
        public string PetName {get; set;} = "";

        // Constructors.
        public Car() {}
        public Car(string name, int speed)
        {
            CurrentSpeed = speed;
            PetName = name;
        }

        // See if Car has overheated.
    }
}
```

```
//Garage.cs
namespace ForEachWithExtensionMethods
{
    class Garage
    {
        public Car[] CarsInGarage { get; set; }

        // Fill with some Car objects upon startup.
        public Garage()
        {
            CarsInGarage = new Car[4];
            CarsInGarage[0] = new Car("Rusty", 30);
            CarsInGarage[1] = new Car("Clunker", 55);
            CarsInGarage[2] = new Car("Zippy", 30);
            CarsInGarage[3] = new Car("Fred", 30);
        }
    }
}
```

Note that the Garage class does not implement IEnumerable, nor does it have a GetEnumerator() method. The GetEnumerator() method is added through the GarageExtensions class, shown here: using System.Collections;

```
namespace ForEachWithExtensionMethods
{
    static class GarageExtensions
    {
        public static IEnumerator GetEnumerator(this Garage g)
            => g.CarsInGarage.GetEnumerator();
    }
}
```

The code to test this new feature is the same code used to test the GetEnumerator() method in Chapter 8. Update Program.cs to the following:

```
using System;
using ForEachWithExtensionMethods;

Console.WriteLine("***** Support for Extension Method GetEnumerator *****
");
Garage carLot = new Garage();

// Hand over each car in the collection?
foreach (Car c in carLot)
{
    Console.WriteLine("{0} is going {1} MPH",
        c.PetName, c.CurrentSpeed);
}
```

You will see that the code works, printing to the console the list of cars and their speed.

```
***** Support for Extension Method GetEnumerator *****

Rusty is going 30 MPH
Clunker is going 55 MPH
Zippy is going 30 MPH
Fred is going 30 MPH
```

Note There is a potential drawback to this new feature, in that classes that were never meant to be foreached could now be foreached.

## Understanding Anonymous Types

As an object-oriented programmer, you know the benefits of defining classes to represent the state and functionality of a given item you are attempting to model. To be sure, whenever you need to define a class that is intended to be reused across projects and that provides numerous bits of functionality through a set of methods, events, properties, and custom constructors, creating a new C# class is common practice.

However, there are other times when you want to define a class simply to model a set of encapsulated (and somehow related) data points without any associated methods, events, or other specialized functionality. Furthermore, what if this type is to be used by only a handful of methods in your program? It would be rather a bother to define a full class definition as shown next when you know full well this class will be used in only a handful of places. To accentuate this point, here is the rough outline of what you might need to do when you need to create a “simple” data type that follows typical value-based semantics:

```
class SomeClass
{
    // Define a set of private member variables...

    // Make a property for each member variable...

    // Override ToString() to account for key member variables...

    // Override GetHashCode() and Equals() to work with value-based equality...
}
```

As you can see, it is not necessarily so simple. Not only do you need to author a fair amount of code, but you have another class to maintain in your system. For temporary data such as this, it would be useful to whip up a custom data type on the fly. For example, let’s say you need to build a custom method that receives a set of incoming parameters. You would like to take these parameters and use them to create a new data type for use in this method scope. Further, you would like to quickly print out this data using the typical ToString() method and perhaps use other members of System.Object. You can do this very thing using anonymous type syntax.

## Defining an Anonymous Type

When you define an anonymous type, you do so by using the var keyword (see Chapter 3) in conjunction with object initialization syntax (see Chapter 5). You must use the var keyword because the compiler will automatically generate a new class definition at compile time (and you never see the name of this class in your C# code). The initialization syntax is used to tell the compiler to create private backing fields and (read-only) properties for the newly created type.

To illustrate, create a new Console Application project named AnonymousTypes. Now, add the following method to your Program class, which composes a new type, on the fly, using the incoming parameter data:

```
static void BuildAnonymousType( string make, string color, int currSp )
{
    // Build anonymous type using incoming args.
    var car = new { Make = make, Color = color, Speed = currSp };

    // Note you can now use this type to get the property data!
    Console.WriteLine("You have a {0} {1} going {2} MPH", car.Color, car.Make, car.Speed);
}
```
