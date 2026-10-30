
```csharp
static void ChangeDynamicDataType()
{
    // Declare a single dynamic data point
    // named "t".
    dynamic t = "Hello!";
    Console.WriteLine("t is of type: {0}", t.GetType());

    t = false;
    Console.WriteLine("t is of type: {0}", t.GetType());

    t = new List<int>();
    Console.WriteLine("t is of type: {0}", t.GetType());
}
```

```
t is of type: System.String
t is of type: System.Boolean
t is of type: System.Collections.Generic.List`1[System.Int32]
```

At this point in your investigation, do be aware that the previous code would compile and execute identically if you were to declare the t variable as a System.Object. However, as you will soon see, the dynamic keyword offers many additional features.

## Calling Members on Dynamically Declared Data

Given that a dynamic variable can take on the identity of any type on the fly (just like a variable of type System.Object), the next question on your mind might be about calling members on the dynamic variable (properties, methods, indexers, and events, etc.). Well, syntactically speaking, it will again look no different. Just apply the dot operator to the dynamic data variable, specify a public member, and supply any arguments (if required).

However (and this is a very big “however”), the validity of the members you specify will not be checked by the compiler! Remember, unlike a variable defined as a System.Object, dynamic data is not statically typed. It is not until runtime that you will know whether the dynamic data you invoked supports a specified member, whether you passed in the correct parameters, whether you spelled the member correctly, and so on. Thus, as strange as it might seem, the following method compiles perfectly:

```csharp
static void InvokeMembersOnDynamicData()
{
    dynamic textData1 = "Hello";
    Console.WriteLine(textData1.ToUpper());

    // You would expect compiler errors here!
    // But they compile just fine.
    Console.WriteLine(textData1.toupper());
    Console.WriteLine(textData1.Foo(10, "ee", DateTime.Now));
}
```

Notice the second call to WriteLine() attempts to call a method named toupper() on the dynamic data point (note the incorrect casing—it should be ToUpper()). As you can see, textData1 is of type string, and therefore, you know it does not have a method of this name in all lowercase letters. Furthermore, string certainly does not have a method named Foo() that takes int, string, and DateTime objects!

Nevertheless, the C# compiler is satisfied. However, if you invoke this method from within Main(), you will get runtime errors like the following output:

```
Unhandled Exception: Microsoft.CSharp.RuntimeBinder.RuntimeBinderException:
'string' does not contain a definition for 'toupper'
```

Another obvious distinction between calling members on dynamic data and strongly typed data is that when you apply the dot operator to a piece of dynamic data, you will not see the expected Visual Studio IntelliSense. The IDE will allow you to enter any member name you could dream up.

It should make sense that IntelliSense is not possible with dynamic data. However, remember that this means you need to be extremely careful when you are typing C# code on such data points. Any misspelling or incorrect capitalization of a member will throw a runtime error, specifically an instance of the RuntimeBinderException class.

The RuntimeBinderException represents an error that will be thrown if you attempt to invoke a member on a dynamic data type that does not actually exist (as in the case of the toupper() and Foo() methods). This same error will be raised if you specify the wrong parameter data to a member that does exist.

Because dynamic data is so volatile, whenever you are invoking members on a variable declared with the C# dynamic keyword, you could wrap the calls within a proper try/catch block and handle the error in a graceful manner, like so:

```csharp
static void InvokeMembersOnDynamicData()
{
    dynamic textData1 = "Hello";

    try
    {
        Console.WriteLine(textData1.ToUpper());
        Console.WriteLine(textData1.toupper());
        Console.WriteLine(textData1.Foo(10, "ee", DateTime.Now));
    }
    catch (Microsoft.CSharp.RuntimeBinder.RuntimeBinderException ex)
    {
        Console.WriteLine(ex.Message);
    }
}
```

If you call this method again, you will find the call to ToUpper() (note the capital T and U) works correctly; however, you then find the error data displayed to the console.

```
HELLO
'string' does not contain a definition for 'toupper'
```

Of course, the process of wrapping all dynamic method invocations in a try/catch block is rather tedious. If you watch your spelling and parameter passing, this is not required. However, catching exceptions is handy when you might not know in advance if a member will be present on the target type.

## The Scope of the dynamic Keyword

Recall that implicitly typed data (declared with the var keyword) is possible only for local variables in a member scope. The var keyword can never be used as a return value, a parameter, or a member of a class/structure. This is not the case with the dynamic keyword, however. Consider the following class definition:

```csharp
namespace DynamicKeyword
{
    class VeryDynamicClass
    {
        // A dynamic field.
        private static dynamic _myDynamicField;

        // A dynamic property.
        public dynamic DynamicProperty { get; set; }

        // A dynamic return type and a dynamic parameter type.
        public dynamic DynamicMethod(dynamic dynamicParam)
        {
            // A dynamic local variable.
            dynamic dynamicLocalVar = "Local variable";

            int myInt = 10;

            if (dynamicParam is int)
            {
                return dynamicLocalVar;
            }
            else
            {
                return myInt;
            }
        }
    }
}
```

You could now invoke the public members as expected; however, as you are operating on dynamic methods and properties, you cannot be completely sure what the data type will be! To be sure, the VeryDynamicClass definition might not be useful in a real-world application, but it does illustrate the scope of where you can apply this C# keyword.

## Limitations of the dynamic Keyword

While a great many things can be defined using the dynamic keyword, there are some limitations regarding its usage. While they are not showstoppers, do know that a dynamic data item cannot make use of lambda expressions or C# anonymous methods when calling a method. For example, the following code will always result in errors, even if the target method does indeed take a delegate parameter that takes a string value and returns void:

```csharp
dynamic a = GetDynamicObject();

// Error! Methods on dynamic data can't use lambdas!
a.Method(arg => Console.WriteLine(arg));
```

To circumvent this restriction, you will need to work with the underlying delegate directly, using the techniques described in Chapter 12. Another limitation is that a dynamic point of data cannot understand any extension methods (see Chapter 11). Unfortunately, this would also include any of the extension methods that come from the LINQ APIs. Therefore, a variable declared with the dynamic keyword has limited use within LINQ to Objects and other LINQ technologies.

```csharp
dynamic a = GetDynamicObject();
// Error! Dynamic data can't find the Select() extension method!
var data = from d in a select d;
```

## Practical Uses of the dynamic Keyword

Given that dynamic data is not strongly typed, not checked at compile time, has no ability to trigger IntelliSense, and cannot be the target of a LINQ query, you are absolutely correct to assume that using the dynamic keyword just for the sake of doing so is a poor programming practice.

However, in a few circumstances, the dynamic keyword can radically reduce the amount of code you need to author by hand. Specifically, if you are building a .NET Core application that makes heavy use of late binding (via reflection), the dynamic keyword can save you typing time. As well, if you are building a .NET Core application that needs to communicate with legacy COM libraries (such as Microsoft Office products), you can greatly simplify your code base via the dynamic keyword. By way of a final example, web applications built using ASP.NET Core frequently use the ViewBag type, which can also be accessed in a simplified manner using the dynamic keyword.

> Note COM interaction is strictly a Windows paradigm and removes the cross-platform capabilities of your application.

Like any "shortcut," you need to weigh the pros and cons. The use of the dynamic keyword is a trade-off between brevity of code and type safety. While C# is a strongly typed language at its core, you can opt in (or opt out) of dynamic behaviors on a call-by-call basis. Always remember that you never need to use the dynamic keyword. You could always get to the same end result by authoring alternative code by hand (and typically much more of it).

## The Role of the Dynamic Language Runtime

Now that you better understand what "dynamic data" is about, let's learn how it is processed. Since the release of .NET 4.0, the Common Language Runtime (CLR) was supplemented with a complementary runtime environment named the Dynamic Language Runtime. The concept of a "dynamic runtime" is certainly not new. In fact, many programming languages such as JavaScript, LISP, Ruby, and Python have used it for years. In a nutshell, a dynamic runtime allows a dynamic language the ability to discover types completely at runtime with no compile-time checks.

> Note While a lot of the DLR was ported to .NET Core (starting with 3.0), feature parity between the DLR in .NET Core 5 and .NET 4.8 hasn't been achieved.
