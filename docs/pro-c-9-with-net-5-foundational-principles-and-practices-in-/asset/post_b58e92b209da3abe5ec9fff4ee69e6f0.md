
public class PetNameComparer : IComparer
{
    // Test the pet name of each object.
    int IComparer.Compare(object o1, object o2)
    {
        if (o1 is Car t1 && o2 is Car t2)
        {
            return string.Compare(t1.PetName, t2.PetName,
                StringComparison.OrdinalIgnoreCase);
        }
        else
        {
            throw new ArgumentException("Parameter is not a Car!");
        }
    }
}

The object user code can use this helper class. System.Array has several overloaded Sort() methods, one that just happens to take an object implementing IComparer.

```
...
// Now sort by pet name.
Array.Sort(myAutos, new PetNameComparer());

// Dump sorted array.
Console.WriteLine("Ordering by pet name:");
foreach(Car c in myAutos)
{
    Console.WriteLine("{0} {1}", c.CarID, c.PetName);
}
...
```

## Custom Properties and Custom Sort Types

It is worth pointing out that you can use a custom static property to help the object user along when sorting your Car types by a specific data point. Assume the Car class has added a static read-only property named SortByPetName that returns an instance of an object implementing the IComparer interface (PetNameComparer, in this case; be sure to import System.Collections).

```
// We now support a custom property to return
// the correct IComparer interface.
public class Car : IComparable
{
...
// Property to return the PetNameComparer.
public static IComparer SortByPetName
    => (IComparer)new PetNameComparer();
}
```

The object user code can now sort by pet name using a strongly associated property, rather than just “having to know” to use the stand-alone PetNameComparer class type.

```
// Sorting by pet name made a bit cleaner.
Array.Sort(myAutos, Car.SortByPetName);
```

Ideally, at this point you not only understand how to define and implement your own interfaces but also understand their usefulness. To be sure, interfaces are found within every major .NET Core namespace, and you will continue working with various standard interfaces in the remainder of this book.

## Summary

An interface can be defined as a named collection of abstract members. It is common to regard an interface as a behavior that may be supported by a given type. When two or more classes implement the same interface, you can treat each type the same way (interface-based polymorphism) even if the types are defined within unique class hierarchies.

C# provides the interface keyword to allow you to define a new interface. As you have seen, a type can support as many interfaces as necessary using a comma-delimited list. Furthermore, it is permissible to build interfaces that derive from multiple base interfaces.

In addition to building your custom interfaces, the .NET Core libraries define several standard (i.e., framework-supplied) interfaces. As you have seen, you are free to build custom types that implement these predefined interfaces to gain several desirable traits such as cloning, sorting, and enumerating.

# CHAPTER 9

# Understanding Object Lifetime

At this point in the book, you have learned a great deal about how to build custom class types using C#. Now you will see how the runtime manages allocated class instances (aka objects) via garbage collection (GC). C# programmers never directly deallocate a managed object from memory (recall there is no delete keyword in the C# language). Rather, .NET Core objects are allocated to a region of memory termed the managed heap, where they will be automatically destroyed by the garbage collector “sometime in the future.”

After you have looked at the core details of the collection process, you’ll learn how to programmatically interact with the garbage collector using the System.GC class type (which is something you will typically not be required to do for a majority of your projects). Next, you’ll examine how the virtual System.Object.Finalize() method and IDisposable interface can be used to build classes that release internal unmanaged resources in a predictable and timely manner.

You will also delve into some functionality of the garbage collector introduced in .NET 4.0, including background garbage collections and lazy instantiation using the generic System.Lazy<> class. By the time you have completed this chapter, you will have a solid understanding of how .NET Core objects are managed by the runtime.

## Classes, Objects, and References

To frame the topics covered in this chapter, it is important to further clarify the distinction between classes, objects, and reference variables. Recall that a class is nothing more than a blueprint that describes how an instance of this type will look and feel in memory. Classes, of course, are defined within a code file (which in C# takes a *.cs extension by convention). Consider the following simple Car class defined within a new C# Console Application project named SimpleGC:

```
namespace SimpleGC
{
    // Car.cs
    public class Car
    {
        public int CurrentSpeed {get; set;}
        public string PetName {get; set;}

        public Car(){}
        public Car(string name, int speed)
        {
            PetName = name;
            CurrentSpeed = speed;
        }
    }
```

```
public override string ToString()
    => $"{PetName} is going {CurrentSpeed} MPH";
}
}
```

After a class has been defined, you may allocate any number of objects using the C# new keyword. Understand, however, that the new keyword returns a reference to the object on the heap, not the actual object. If you declare the reference variable as a local variable in a method scope, it is stored on the stack for further use in your application. When you want to invoke members on the object, apply the C# dot operator to the stored reference, like so:

```
using System;
using SimpleGC;
Console.WriteLine("***** GC Basics *****");

// Create a new Car object on the managed heap.
// We are returned a reference to the object
// ("refToMyCar").
Car refToMyCar = new Car("Zippy", 50);

// The C# dot operator (.) is used to invoke members
// on the object using our reference variable.
Console.WriteLine(refToMyCar.ToString());
Console.ReadLine();
```

Figure 9-1 illustrates the class, object, and reference relationship.

![](img/587021f4d5ba039be70a11107130377b_388_0.png)

Figure 9-1. References to objects on the managed heap

Note: Recall from Chapter 4 that structures are value types that are always allocated directly on the stack and are never placed on the .NET Core managed heap. Heap allocation occurs only when you are creating instances of classes.

## The Basics of Object Lifetime

When you are building your C# applications, you are correct to assume that the .NET Core runtime environment will take care of the managed heap without your direct intervention. In fact, the golden rule of .NET Core memory management is simple.

Rule: Allocate a class instance onto the managed heap using the new keyword and forget about it.

Once instantiated, the garbage collector will destroy an object when it is no longer needed. The next obvious question, of course, is “How does the garbage collector determine when an object is no longer needed?” The short (i.e., incomplete) answer is that the garbage collector removes an object from the heap only if it is unreachable by any part of your code base. Assume you have a method in your Program class that allocates a local Car object as follows:

```
static void MakeACar()
{
    // If myCar is the only reference to the Car object, it *may* be destroyed when this method returns.
    Car myCar = new Car();
}
```

Notice that this Car reference (myCar) has been created directly within the MakeACar() method and has not been passed outside of the defining scope (via a return value or ref/out parameters). Thus, once this method call completes, the myCar reference is no longer reachable, and the associated Car object is now a candidate for garbage collection. Understand, however, that you can’t guarantee that this object will be reclaimed from memory immediately after MakeACar() has completed. All you can assume at this point is that when the runtime performs the next garbage collection, the myCar object could be safely destroyed.

As you will most certainly discover, programming in a garbage-collected environment greatly simplifies your application development. In stark contrast, C++ programmers are painfully aware that if they fail to manually delete heap-allocated objects, memory leaks are never far behind. In fact, tracking down memory leaks is one of the most time-consuming (and tedious) aspects of programming in unmanaged environments. By allowing the garbage collector to take charge of destroying objects, the burden of memory management has been lifted from your shoulders and placed onto those of the runtime.

## The CIL of new

When the C# compiler encounters the new keyword, it emits a CIL newobj instruction into the method implementation. If you compile the current example code and investigate the resulting assembly using ildasm.exe, you’d find the following CIL statements within the MakeACar() method:

```
.method assembly hidebysig static
    void '<Main>$g__MakeACar|0_0'() cil managed
{
    // Code size       8 (0x8)
    .maxstack 1
    .locals init (class SimpleGC.Car V_0)
    IL_0000: nop
    IL_0001: newobj     instance void SimpleGC.Car::.ctor()
```
