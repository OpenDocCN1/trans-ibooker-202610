
```il
IL_000b: call class [System.Runtime]System.Delegate [System.Runtime]System.Delegate::Remove(class [System.Runtime]System.Delegate, class [System.Runtime]System.Delegate)
...
}
```

Finally, the CIL code representing the event itself uses the .addon and .removeon directives to map the names of the correct add_XXX() and remove_XXX() methods to invoke.

```il
.event class [System.Runtime]System.EventHandler`1<class CarEvents.CarEventArgs> AboutToBlow
{
.addon instance void CarEvents.Car::add_AboutToBlow(
    class [System.Runtime]System.EventHandler`1<class CarEvents.CarEventArgs>)
.removeon instance void CarEvents.Car::remove_AboutToBlow(
    class [System.Runtime]System.EventHandler`1<class CarEvents.CarEventArgs>)
} // end of event Car::AboutToBlow
```

Now that you understand how to build a class that can send C# events (and are aware that events are little more than a typing time-saver), the next big question is how to listen to the incoming events on the caller’s side.

## Listening to Incoming Events

C# events also simplify the act of registering the caller-side event handlers. Rather than having to specify custom helper methods, the caller simply uses the += and -= operators directly (which triggers the correct add_XXX() or remove_XXX() method in the background). When you want to register with an event, follow the pattern shown here:

```csharp
// NameOfObject.NameOfEvent +=
//   new RelatedDelegate(functionToCall);
//
Car.CarEngineHandler d =
  new Car.CarEngineHandler(CarExplodedEventHandler);
myCar.Exploded += d;
```

When you want to detach from a source of events, use the -= operator, using the following pattern:

```csharp
// NameOfObject.NameOfEvent -=
//   new RelatedDelegate(functionToCall);
//
myCar.Exploded -= d;
```

Note that you can also use the method group conversion syntax with events as well:

```csharp
Car.CarEngineHandler d = CarExplodedEventHandler;
myCar.Exploded += d;
```

Given these very predictable patterns, here is the refactored calling code, now using the C# event registration syntax:

```csharp
Console.WriteLine("***** Fun with Events *****");
Car c1 = new Car("SlugBug", 100, 10);

// Register event handlers.
c1.AboutToBlow += CarIsAlmostDoomed;
c1.AboutToBlow += CarAboutToBlow;

Car.CarEngineHandler d = CarExploded;
c1.Exploded += d;

Console.WriteLine("***** Speeding up *****");
for (int i = 0; i < 6; i++)
{
    c1.Accelerate(20);
}

// Remove CarExploded method
// from invocation list.
c1.Exploded -= d;

Console.WriteLine("\n***** Speeding up *****");
for (int i = 0; i < 6; i++)
{
    c1.Accelerate(20);
}
Console.ReadLine();

static void CarAboutToBlow(string msg)
{
    Console.WriteLine(msg);
}

static void CarIsAlmostDoomed(string msg)
{
    Console.WriteLine("=> Critical Message from Car: {0}", msg);
}

static void CarExploded(string msg)
{
    Console.WriteLine(msg);
}
```

## Simplifying Event Registration Using Visual Studio

Visual Studio helps with the process of registering event handlers. When you apply the += syntax during event registration, you will find an IntelliSense window displayed, inviting you to hit the Tab key to autocomplete the associated delegate instance (see Figure 12-1), which is captured using method group conversion syntax.

Figure 12-1. Delegate selection IntelliSense

After you hit the Tab key, the IDE will generate the new method automatically, as shown in Figure 12-2.

![](img/587021f4d5ba039be70a11107130377b_519_0.png)

![](img/587021f4d5ba039be70a11107130377b_519_1.png)

Figure 12-2. Delegate target format IntelliSense

Note the stub code is in the correct format of the delegate target (note that this method has been declared static because the event was registered within a static method).

```csharp
static void NewCar_AboutToBlow(string msg)
{
    throw new NotImplementedException();
}
```

IntelliSense is available to all .NET Core events, your custom events, and all the events in the base class libraries. This IDE feature is a massive time-saver, given that it saves you from having to search the help system to figure out both the correct delegate to use with an event and the format of the delegate target method.

## Creating Custom Event Arguments

Truth be told, there is one final enhancement you could make to the current iteration of the Car class that mirrors Microsoft’s recommended event pattern. As you begin to explore the events sent by a given type in the base class libraries, you will find that the first parameter of the underlying delegate is a System.Object, while the second parameter is a descendant of System.EventArgs.

The System.Object argument represents a reference to the object that sent the event (such as the Car), while the second parameter represents information regarding the event at hand. The System.EventArgs base class represents an event that is not sending any custom information.

```csharp
public class EventArgs
{
    public static readonly EventArgs Empty;
    public EventArgs();
}
```

For simple events, you can pass an instance of EventArgs directly. However, when you want to pass along custom data, you should build a suitable class deriving from EventArgs. For this example, assume you have a class named CarEventArgs, which maintains a string representing the message sent to the receiver.

```csharp
using System;

namespace CarEvents
{
    public class CarEventArgs : EventArgs
    {
        public readonly string msg;
        public CarEventArgs(string message)
        {
            msg = message;
        }
    }
}
```

With this, you would now update the CarEngineHandler delegate type definition as follows (the events would be unchanged):

```csharp
public class Car
{
    public delegate void CarEngineHandler(object sender, CarEventArgs e);
    ...
}
```

Here, when firing the events from within the Accelerate() method, you would now need to supply a reference to the current Car (via the this keyword) and an instance of the CarEventArgs type. For example, consider the following partial update:

```csharp
public void Accelerate(int delta)
{
    // If the car is dead, fire Exploded event.
    if (carIsDead)
    {
        Exploded?.Invoke(this, new CarEventArgs("Sorry, this car is dead..."));
    }
    ...
}
```

On the caller's side, all you would need to do is update your event handlers to receive the incoming parameters and obtain the message via the read-only field. Here's an example:

```csharp
static void CarAboutToBlow(object sender, CarEventArgs e)
{
    Console.WriteLine($"{sender} says: {e.msg}");
}
```

If the receiver wants to interact with the object that sent the event, you can explicitly cast the System.Object. From this reference, you can make use of any public member of the object that sent the event notification.

```csharp
static void CarAboutToBlow(object sender, CarEventArgs e)
{
    // Just to be safe, perform a
    // runtime check before casting.
    if (sender is Car c)
    {
        Console.WriteLine(
            $"Critical Message from {c.PetName}: {e.msg}");
    }
}
```

## The Generic EventHandler<T> Delegate

Given that so many custom delegates take an object as the first parameter and an EventArgs descendant as the second, you could further streamline the previous example by using the generic EventHandler<T> type, where T is your custom EventArgs type. Consider the following update to the Car type (notice how you no longer need to define a custom delegate type at all):

```csharp
public class Car
{
    ...
    public event EventHandler<CarEventArgs> Exploded;
    public event EventHandler<CarEventArgs> AboutToBlow;
}
```

The calling code could then use EventHandler<CarEventArgs> anywhere you previously specified CarEngineHandler (or, once again, use method group conversion).

```csharp
Console.WriteLine("***** Prim and Proper Events *****");

// Make a car as usual.
Car c1 = new Car("SlugBug", 100, 10);

// Register event handlers.
c1.AboutToBlow += CarIsAlmostDoomed;
c1.AboutToBlow += CarAboutToBlow;

EventHandler<CarEventArgs> d = CarExploded;
c1.Exploded += d;
...
```

Great! At this point, you have seen the core aspects of working with delegates and events in the C# language. While you could use this information for just about all your callback needs, you will wrap up this chapter with a look at some final simplifications, specifically anonymous methods and lambda expressions.

## Understanding C# Anonymous Methods

As you have seen, when a caller wants to listen to incoming events, it must define a custom method in a class (or structure) that matches the signature of the associated delegate. Here’s an example:

```csharp
SomeType t = new SomeType();

// Assume "SomeDelegate" can point to methods taking no
// args and returning void.
t.SomeEvent += new SomeDelegate(MyEventHandler);

// Typically only called by the SomeDelegate object.
static void MyEventHandler()
{
    // Do something when event is fired.
}
```
