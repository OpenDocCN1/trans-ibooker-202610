# Understanding Constant Field Data

C# offers the const keyword to define constant data, which can never change after the initial assignment. As you might guess, this can be helpful when you are defining a set of known values for use in your applications that are logically connected to a given class or structure.

Assume you are building a utility class named MyMathClass that needs to define a value for pi (which you will assume to be 3.14 for simplicity). Begin by creating a new Console Application project named ConstData and add a file named MyMathClass.cs. Given that you would not want to allow other developers to change this value in code, pi could be modeled with the following constant:

```
//MyMathClass.cs
using System;
namespace ConstData
{
    class MyMathClass
    {
        public const double PI = 3.14;
    }
}
```

Update the code in the Program.cs class to match this:

```
using System;
using ConstData;

Console.WriteLine("**** Fun with Const ****\n");
Console.WriteLine("The value of PI is: {0}", MyMathClass.PI);
// Error! Can't change a constant!
// MyMathClass.PI = 3.1444;

Console.ReadLine();
```

Notice that you are referencing the constant data defined by MyMathClass using a class name prefix (i.e., MyMathClass.PI). This is because constant fields of a class are implicitly static. However, it is permissible to define and access a local constant variable within the scope of a method or property. Here is an example:

```
static void LocalConstStringVariable()
{
    // A local constant data point can be directly accessed.
    const string fixedStr = "Fixed string Data";
    Console.WriteLine(fixedStr);

    // Error!
    // fixedStr = "This will not work!";
}
```

Regardless of where you define a constant piece of data, the one point to always remember is that the initial value assigned to the constant must be specified at the time you define the constant. Assigning the value of pi in a class constructor, as shown in the following code, produces a compilation error: