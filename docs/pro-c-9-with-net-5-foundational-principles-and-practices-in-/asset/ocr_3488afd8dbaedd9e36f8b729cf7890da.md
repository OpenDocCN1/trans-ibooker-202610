```
// This generic delegate can represent any method
// returning void and taking a single parameter of type T.
public delegate void MyGenericDelegate<T>(T arg);
```

Notice that MyGenericDelegate<T> defines a single type parameter that represents the argument to pass to the delegate target. When creating an instance of this type, you are required to specify the value of the type parameter, as well as the name of the method the delegate will invoke. Thus, if you specified a string type, you send a string value to the target method.

```
// Create an instance of MyGenericDelegate<T>
// with string as the type parameter.
MyGenericDelegate<string> strTarget = StringTarget;
strTarget("Some string data");
```

Given the format of the strTarget object, the StringTarget() method must now take a single string as a parameter.

```
static void StringTarget(string arg)
{
    Console.WriteLine(
        "arg in uppercase is: {0}", arg.ToUpper());
}
```

# The Generic Action<> and Func<> Delegates

Over the course of this chapter, you have seen that when you want to use delegates to enable callbacks in your applications, you typically follow the steps shown here:

+   1. Define a custom delegate that matches the format of the method being pointed to.
+   2. Create an instance of your custom delegate, passing in a method name as a constructor argument.
+   3. Invoke the method indirectly, via a call to Invoke() on the delegate object.

When you take this approach, you typically end up with several custom delegates that might never be used beyond the current task at hand (e.g., MyGenericDelegate<T>, CarEngineHandler, etc.). While it may certainly be the case that you do indeed need to have a custom, uniquely named delegate type for your project, other times the exact name of the delegate type is irrelevant. In many cases, you simply want “some delegate” that takes a set of arguments and possibly has a return value other than void. In these cases, you can use the framework’s built-in Action<> and Func<> delegate types. To illustrate their usefulness, create a new Console Application project named ActionAndFuncDelegates.

The generic Action<> delegate is defined in the System namespace, and you can use this generic delegate to “point to” a method that takes up to 16 arguments (that ought to be enough!) and returns void. Now recall, because Action<> is a generic delegate, you will need to specify the underlying types of each parameter as well.