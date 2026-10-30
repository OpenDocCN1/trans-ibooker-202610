To illustrate dynamic loading, create a new Console Application project named ExternalAssemblyReflector. Your task is to construct code that prompts for the name of an assembly (minus any extension) to load dynamically. You will pass the Assembly reference into a helper method named DisplayTypes(), which will simply print the names of each class, interface, structure, enumeration, and delegate it contains. The code is refreshingly simple.

```
using System;
using System.Reflection;
using System.IO; // For FileNotFoundException definition.

Console.WriteLine("***** External Assembly Viewer *****");
string asmName = "";
Assembly asm = null;
do
{
Console.WriteLine("\nEnter an assembly to evaluate");
Console.Write("or enter Q to quit: ");
// Get name of assembly.
asmName = Console.ReadLine();
// Does user want to quit?
if (asmName.Equals("Q",StringComparison.OrdinalIgnoreCase))
{
    break;
}

// Try to load assembly.
try
{
    asm = Assembly.LoadFrom(asmName);
    DisplayTypesInAsm(asm);
}
catch
{
    Console.WriteLine("Sorry, can't find assembly.");
}
} while (true);

static void DisplayTypesInAsm(Assembly asm)
{
    Console.WriteLine("\n***** Types in Assembly *****");
    Console.WriteLine("{0}", asm.FullName);
    Type[] types = asm.GetTypes();
    foreach (Type t in types)
    {
        Console.WriteLine("Type: {0}", t);
    }
    Console.WriteLine("");
}
```

If you want to reflect over CarLibrary.dll, you will need to copy the CarLibrary.dll binary (from the previous chapter) to the project directory (if using Visual Studio Code) or \bin\Debug\net5.0 (if using