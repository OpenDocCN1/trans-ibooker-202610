(and a few overloaded operators) to interact with the invocation list. Here are some select members of System.MulticastDelegate:

```
public abstract class MulticastDelegate : Delegate
{
    // Returns the list of methods "pointed to."
    public sealed override Delegate[] GetInvocationList();

    // Overloaded operators.
    public static bool operator ==
        (MulticastDelegate d1, MulticastDelegate d2);
    public static bool operator !=
        (MulticastDelegate d1, MulticastDelegate d2);

    // Used internally to manage the list of methods maintained by the delegate.
    private IntPtr _invocationCount;
    private object _invocationList;
}
```

System.MulticastDelegate obtains additional functionality from its parent class, System.Delegate. Here is a partial snapshot of the class definition:

```
public abstract class Delegate : ICloneable, ISerializable
{
    // Methods to interact with the list of functions.
    public static Delegate Combine(params Delegate[] delegates);
    public static Delegate Combine(Delegate a, Delegate b);
    public static Delegate Remove(
        Delegate source, Delegate value);
    public static Delegate RemoveAll(
        Delegate source, Delegate value);

    // Overloaded operators.
    public static bool operator ==(Delegate d1, Delegate d2);
    public static bool operator !=(Delegate d1, Delegate d2);

    // Properties that expose the delegate target.
    public MethodInfo Method { get; }
    public object Target { get; }
}
```

Now, understand that you can never directly derive from these base classes in your code (it is a compiler error to do so). Nevertheless, when you use the delegate keyword, you have indirectly created a class that “is-a” MulticastDelegate. Table 12-1 documents the core members common to all delegate types.