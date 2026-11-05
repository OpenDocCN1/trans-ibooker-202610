
public interface IKeyHolder
{
    void Lose();
}

public class Person : IGamePlayer, IKeyHolder
{
    public void Lose() // implicit implementation
    {
        // implement losing a key
    }

    void IGamePlayer.Lose() // explicit implementation
    {
        // implement losing a game
    }
}

// calling implicit and explicit implementations of Lose
Person p = new();
p.Lose(); // calls implicit implementation of losing a key

((IGamePlayer)p).Lose(); // calls explicit implementation of losing a game

IGamePlayer player = p as IGamePlayer;
player.Lose(); // calls explicit implementation of losing a game

## Defining interfaces with default implementations

A language feature introduced in C# 8.0 is **default implementations** for an interface. Let's see it in action:

- 1. In the PacktLibrary project, add a new file named IPlayable.cs.
- 2. Modify the statements to define a public IPlayable interface with two methods to Play and Pause, as shown in the following code:

```
namespace Packt.Shared;

public interface IPlayable
{
    void Play();
    void Pause();
}
```

- 3. In the PacktLibrary project, add a new class file named DvdPlayer.cs.
- 4. Modify the statements in the file to implement the IPlayable interface, as shown in the following code:

```
using static System.Console;

namespace Packt.Shared;

public class DvdPlayer : IPlayable
{
    public void Pause()
    {
        WriteLine("DVD player is pausing.");
    }

    public void Play()
    {
        WriteLine("DVD player is playing.");
    }
}
```

This is useful, but what if we decide to add a third method named Stop? Before C# 8.0, this would be impossible once at least one type implements the original interface. One of the main points of an interface is that it is a fixed contract.

C# 8.0 allows an interface to add new members after release as long as they have a default implementation. C# purists do not like the idea, but for practical reasons, such as avoiding breaking changes or having to define a whole new interface, it is useful, and other languages such as Java and Swift enable similar techniques.

Support for default interface implementations requires some fundamental changes to the underlying platform, so they are only supported with C# if the target framework is .NET 5.0 or later, .NET Core 3.0 or later, or .NET Standard 2.1. They are therefore not supported by .NET Framework.

- 5. Modify the IPlayable interface to add a Stop method with a default implementation, as shown highlighted in the following code:

```
using static System.Console;

namespace Packt.Shared;

public interface IPlayable
{
    void Play();
    void Pause();

    void Stop() // default interface implementation
    {
        WriteLine("Default implementation of Stop.");
    }
}
```

- 6. Build the PeopleApp project and note that the projects compile successfully despite the DvdPlayer class not implementing Stop. In the future, we could override the default implementation of Stop by implementing it in the DvdPlayer class.

## Managing memory with reference and value types

I have mentioned reference types a couple of times. Let's look at them in more detail.

There are two categories of memory: stack memory and heap memory. With modern operating systems, the stack and heap can be anywhere in physical or virtual memory.

Stack memory is faster to work with (because it is managed directly by the CPU and because it uses a last-in, first-out mechanism, it is more likely to have the data in its L1 or L2 cache) but limited in size, while heap memory is slower but much more plentiful.

For example, in a macOS terminal, I can enter the command ulimit -a to discover that the stack size is limited to 8,192 KB and that other memory is "unlimited." This limited amount of stack memory is why it is so easy to fill it up and get a "stack overflow."

## Defining reference and value types

There are three C# keywords that you can use to define object types: class, record, and struct. All can have the same members, such as fields and methods. One difference between them is how memory is allocated.

When you define a type using record or class, you are defining a reference type. This means that the memory for the object itself is allocated on the heap, and only the memory address of the object (and a little overhead) is stored on the stack.

When you define a type using record struct or struct, you are defining a value type. This means that the memory for the object itself is allocated on the stack.

If a struct uses field types that are not of the struct type, then those fields will be stored on the heap, meaning the data for that object is stored in both the stack and the heap!

These are the most common struct types:

- **Number System types**: byte, sbyte, short, ushort, int, uint, long, ulong, float, double, and decimal
- **Other System types**: char, DateTime, and bool
- **System.Drawing types**: Color, Point, and Rectangle

Almost all the other types are class types, including string.

Apart from the difference in terms of where in memory the data for a type is stored, the other major difference is that you cannot inherit from a struct.

## How reference and value types are stored in memory

Imagine that you have a console app that declares some variables, as shown in the following code:

```
int number1 = 49;
long number2 = 12;
System.Drawing.Point location = new(x: 4, y: 5);
Person kevin = new() { Name = "Kevin",
    DateOfBirth = new(year: 1988, month: 9, day: 23) };
Person sally;
```

Let's review what memory is allocated on the stack and heap when these statements execute, as shown in Figure 6.1 and as described in the following list:

- The number1 variable is a value type (also known as struct) so it is allocated on the stack and it uses 4 bytes of memory since it is a 32-bit integer. Its value, 49, is stored directly in the variable.
- The number2 variable is also a value type so it is also allocated on the stack, and it uses 8 bytes since it is a 64-bit integer.
- The location variable is also a value type so it is allocated on the stack and it uses 8 bytes since it is made up of two 32-bit integers, x and y.
- The kevin variable is a reference type (also known as class) so 8 bytes for a 64-bit memory address (assuming a 64-bit operating system) is allocated on the stack and enough bytes on the heap to store an instance of a Person.
- The sally variable is a reference type so 8 bytes for a 64-bit memory address is allocated on the stack. It is currently null, meaning no memory has yet been allocated for it on the heap.

![](img/c63b00dca7192cc58cfb9e1f1e64f483_271_0.png)

Figure 6.1: How value and reference types are allocated in the stack and heap

All the allocated memory for a reference type is stored on the heap. If a value type such as DateTime is used for a field of a reference type like Person, then the DateTime value is stored on the heap.

If a value type has a field that is a reference type, then that part of the value type is stored on the heap. Point is a value type that consists of two fields, both of which are themselves value types, so the entire object can be allocated on the stack. If the Point value type had a field that was a reference type, like string, then the string bytes would be stored on the heap.

## Equality of types

It is common to compare two variables using the == and != operators. The behavior of these two operators is different for reference types and value types.

When you check the equality of two value type variables, .NET literally compares the values of those two variables on the stack and returns true if they are equal, as shown in the following code:

```
int a = 3;
int b = 3;
WriteLine($"a == b: {(a == b)}"); // true
```

When you check the equality of two reference type variables, .NET compares the memory addresses of those two variables and returns true if they are equal, as shown in the following code:

```
Person a = new() { Name = "Kevin" };
Person b = new() { Name = "Kevin" };
WriteLine($"a == b: {(a == b)}"); // false
```

This is because they are not the same object. If both variables literally point to the same object on the heap, then they would be equal, as shown in the following code:

```
Person a = new() { Name = "Kevin" };
Person b = a;
WriteLine($"a == b: {(a == b)}"); // true
```

The one exception to this behavior is the string type. It is a reference type, but the equality operators have been overridden to make them behave as if they were value types, as shown in the following code:

```
string a = "Kevin";
string b = "Kevin";
WriteLine($"a == b: {(a == b)}"); // true
```

You can do something similar with your classes to make the equality operators return true even if they are not the same object (same memory address on the heap) but instead if their fields have the same values, but that is beyond the scope of this book. Alternatively, use a record class because one of their benefits is that they implement this behavior for you.

## Defining struct types

Let's explore defining your own value types:

- 1. In the PacktLibrary project, add a file named DisplacementVector.cs.
- 2. Modify the file, as shown in the following code, and note the following:
  - The type is declared using struct instead of class.
  - It has two int fields, named X and Y.
  - It has a constructor for setting initial values for X and Y.
  - It has an operator for adding two instances together that returns a new instance of the type with X added to X, and Y added to Y.

```
namespace Packt.Shared;

public struct DisplacementVector
{
    public int X;
