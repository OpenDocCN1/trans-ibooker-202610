+   1. Use your preferred coding tool to add a new Class Library to the Chapter04 workspace/solution named CallStackExceptionHandlingLib.
+   2. Rename the Class1.cs file to Calculator.cs.
+   3. Open Calculator.cs and modify its contents, as shown in the following code:
+   4. Use your preferred coding tool to add a new Console Application to the Chapter04 workspace/solution named CallStackExceptionHandling.
+   5. In Visual Studio Code, select CallStackExceptionHandling as the active OmniSharp project. When you see the pop-up warning message saying that required assets are missing, click Yes to add them.
+   6. In the CallStackExceptionHandling project, add a reference to the CallStackExceptionHandlingLib project.
+   7. In Program.cs, add statements to define two methods and chain calls to them, and the methods in the class library, as shown in the following code:

```
```csharp
using static System.Console;

namespace Packt;

public class Calculator
{
    public static void Gamma() // public so it can be called from outside
    {
        WriteLine("In Gamma");
        Delta();
    }

    private static void Delta() // private so it can only be called internally
    {
        WriteLine("In Delta");
        File.OpenText("bad file path");
    }
}
```
```

```
```csharp
using Packt;

using static System.Console;

WriteLine("In Main");
Alpha();

static void Alpha()
{
    WriteLine("In Alpha");
    Beta();
}
```
```