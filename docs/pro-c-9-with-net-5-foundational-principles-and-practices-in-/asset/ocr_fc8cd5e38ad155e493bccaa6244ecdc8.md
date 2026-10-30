# The Generic EventHandler<T> Delegate

Given that so many custom delegates take an object as the first parameter and an EventArgs descendant as the second, you could further streamline the previous example by using the generic EventHandler<T> type, where T is your custom EventArgs type. Consider the following update to the Car type (notice how you no longer need to define a custom delegate type at all):

```
public class Car
{
    ...
    public event EventHandler<CarEventArgs> Exploded;
    public event EventHandler<CarEventArgs> AboutToBlow;
}
```

The calling code could then use EventHandler<CarEventArgs> anywhere you previously specified CarEventHandler (or, once again, use method group conversion).

```
Console.WriteLine("***** Prim and Proper Events *****
");

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

# Understanding C# Anonymous Methods

As you have seen, when a caller wants to listen to incoming events, it must define a custom method in a class (or structure) that matches the signature of the associated delegate. Here’s an example:

```
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