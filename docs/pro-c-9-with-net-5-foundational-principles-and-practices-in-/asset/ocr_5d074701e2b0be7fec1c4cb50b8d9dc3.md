```
Console.WriteLine("***** Speeding up *****");
for (int i = 0; i < 6; i++)
{
    c2.Accelerate(20);
}

// Unregister the simple method name.
c2.UnRegisterWithCarEngine(OnCarEngineEvent);

// No more notifications!
for (int i = 0; i < 6; i++)
{
    c2.Accelerate(20);
}

Console.ReadLine();
```

Notice that you are not directly allocating the associated delegate object but rather simply specifying a method that matches the delegate's expected signature (a method returning void and taking a single string, in this case). Understand that the C# compiler is still ensuring type safety. Thus, if the OnCarEngineEvent() method did not take a string and return void, you would be issued a compiler error.

# Understanding Generic Delegates

In Chapter 10, I mentioned that C# allows you to define generic delegate types. For example, assume you want to define a delegate type that can call any method returning void and receiving a single parameter. If the argument in question may differ, you could model this using a type parameter. To illustrate, consider the following code within a new Console Application project named GenericDelegate:

```
Console.WriteLine("***** Generic Delegates *****
");

// Register targets.
MyGenericDelegate<string> strTarget =
    new MyGenericDelegate<string>(StringTarget);
strTarget("Some string data");

//Using the method group conversion syntax
MyGenericDelegate<int> intTarget = IntTarget;
intTarget(9);
Console.ReadLine();

static void StringTarget(string arg)
{
    Console.WriteLine("arg in uppercase is: {0}", arg.ToUpper());
}

static void IntTarget(int arg)
{
    Console.WriteLine("++arg is: {0}", ++arg);
}
```