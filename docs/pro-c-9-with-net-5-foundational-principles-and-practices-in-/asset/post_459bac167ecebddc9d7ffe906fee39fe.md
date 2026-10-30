
The obvious problem with these older techniques is the tremendous lack of symmetry. Each approach is more or less tailored to a given technology, a given language, and perhaps even a given project. To put an end to this madness, the .NET platform provides a standard technique to send and trap runtime errors: structured exception handling. The beauty of this approach is that developers now have a unified approach to error handling, which is common to all languages targeting the .NET platform. Therefore, the way in which a C# programmer handles errors is syntactically similar to that of a VB programmer, or a C++ programmer using C++/CLI.

As an added bonus, the syntax used to throw and catch exceptions across assemblies and machine boundaries is identical. For example, if you use C# to build an ASP.NET Core RESTful service, you can throw a JSON fault to a remote caller, using the same keywords that allow you to throw an exception between methods in the same application.

Another bonus of .NET exceptions is that rather than receiving a cryptic numerical value, exceptions are objects that contain a human-readable description of the problem, as well as a detailed snapshot of the call stack that triggered the exception in the first place. Furthermore, you are able to give the end user help link information that points the user to a URL that provides details about the error, as well as custom programmer-defined data.

## The Building Blocks of .NET Exception Handling

Programming with structured exception handling involves the use of four interrelated entities.

- A class type that represents the details of the exception
- A member that throws an instance of the exception class to the caller under the correct circumstances
- A block of code on the caller’s side that invokes the exception-prone member
- A block of code on the caller’s side that will process (or catch) the exception, should it occur

The C# programming language offers five keywords (`try`, `catch`, `throw`, `finally`, and `when`) that allow you to throw and handle exceptions. The object that represents the problem at hand is a class extending `System.Exception` (or a descendant thereof). Given this fact, let’s check out the role of this exception-centric base class.

## The System.Exception Base Class

All exceptions ultimately derive from the `System.Exception` base class, which in turn derives from `System.Object`. Here is the crux of this class (note that some of these members are virtual and may thus be overridden by derived classes):

```csharp
public class Exception : ISerializable
{
    // Public constructors
    public Exception(string message, Exception innerException);
    public Exception(string message);
    public Exception();
    ...
    // Methods
    public virtual Exception GetBaseException();
    public virtual void GetObjectData(SerializationInfo info,
        StreamingContext context);
    // Properties
    public virtual IDictionary Data { get; }
    public virtual string HelpLink { get; set; }
    public int HResult { get; set; }
    public Exception InnerException { get; }
    public virtual string Message { get; }
    public virtual string Source { get; set; }
    public virtual string StackTrace { get; }
    public MethodBase TargetSite { get; }
}
```

As you can see, many of the properties defined by `System.Exception` are read-only in nature. This is because derived types will typically supply default values for each property. For example, the default message of the `IndexOutOfRangeException` type is “Index was outside the bounds of the array.”

Table 7-1 describes the most important members of `System.Exception`.

## Table 7-1. Core Members of the System.Exception Type

| System.Exception Property | Meaning in Life |
|---|---|
| Data | This read-only property retrieves a collection of key-value pairs (represented by an object implementing `IDictionary`) that provide additional, programmer-defined information about the exception. By default, this collection is empty. |
| HelpLink | This property gets or sets a URL to a help file or website describing the error in full detail. |
| InnerException | This read-only property can be used to obtain information about the previous exceptions that caused the current exception to occur. The previous exceptions are recorded by passing them into the constructor of the most current exception. |
| Message | This read-only property returns the textual description of a given error. The error message itself is set as a constructor parameter. |
| Source | This property gets or sets the name of the assembly, or the object, that threw the current exception. |
| StackTrace | This read-only property contains a string that identifies the sequence of calls that triggered the exception. As you might guess, this property is useful during debugging or if you want to dump the error to an external error log. |
| TargetSite | This read-only property returns a `MethodBase` object, which describes numerous details about the method that threw the exception (invoking `ToString()` will identify the method by name). |

## The Simplest Possible Example

To illustrate the usefulness of structured exception handling, you need to create a class that will throw an exception under the correct (or one might say exceptional) circumstances. Assume you have created a new C# Console Application project (named `SimpleException`) that defines two class types (`Car` and `Radio`) associated by the “has-a” relationship. The `Radio` type defines a single method that turns the radio’s power on or off.

```csharp
using System;

namespace SimpleException
{
    class Radio
    {
        public void TurnOn(bool on)
        {
            Console.WriteLine(on ? "Jamming..." : "Quiet time...");
        }
    }
}
```

In addition to leveraging the `Radio` class via containment/delegation, the `Car` class (shown next) is defined in such a way that if the user accelerates a `Car` object beyond a predefined maximum speed (specified using a constant member variable named `MaxSpeed`), its engine explodes, rendering the `Car` unusable (captured by a private `bool` member variable named `_carIsDead`).

Beyond these points, the `Car` type has a few properties to represent the current speed and a user-supplied "pet name," as well as various constructors to set the state of a new `Car` object. Here is the complete definition (with code comments):

```csharp
using System;

namespace SimpleException
{
    class Car
    {
        // Constant for maximum speed.
        public const int MaxSpeed = 100;

        // Car properties.
        public int CurrentSpeed { get; set; } = 0;
        public string PetName { get; set; } = "";

        // Is the car still operational?
        private bool _carIsDead;

        // A car has-a radio.
        private readonly Radio _theMusicBox = new Radio();

        // Constructors.
        public Car() { }
        public Car(string name, int speed)
        {
            CurrentSpeed = speed;
            PetName = name;
        }

        public void CrankTunes(bool state)
        {
            // Delegate request to inner object.
            _theMusicBox.TurnOn(state);
        }
    }
}
```

```csharp
// See if Car has overheated.
public void Accelerate(int delta)
{
    if (_carIsDead)
    {
        Console.WriteLine("{0} is out of order...", PetName);
    }
    else
    {
        CurrentSpeed += delta;
        if (CurrentSpeed > MaxSpeed)
        {
            Console.WriteLine("{0} has overheated!", PetName);
            CurrentSpeed = 0;
            _carIsDead = true;
        }
        else
        {
            Console.WriteLine("=> CurrentSpeed = {0}",
                CurrentSpeed);
        }
    }
}
```

Next, update your `Program.cs` code to force a `Car` object to exceed the predefined maximum speed (set to 100 in the `Car` class), as shown here:

```csharp
using System;
using System.Collections;
using SimpleException;

Console.WriteLine("***** Simple Exception Example *****");
Console.WriteLine("=> Creating a car and stepping on it!");
Car myCar = new Car("Zippy", 20);
myCar.CrankTunes(true);

for (int i = 0; i < 10; i++)
{
    myCar.Accelerate(10);
}
Console.ReadLine();
```

When executing the code, you would see the following output:

```text
***** Simple Exception Example *****
=> Creating a car and stepping on it!
Jamming...
=> CurrentSpeed = 30
=> CurrentSpeed = 40
```
