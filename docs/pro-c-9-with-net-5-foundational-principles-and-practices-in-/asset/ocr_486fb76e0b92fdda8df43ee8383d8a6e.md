Note Support for AppDomains is changed in .NET Core. In .NET Core, there is exactly one AppDomain. Creation of new AppDomains is no longer supported because they require runtime support and are generally expensive to create. The ApplicationLoadContext (covered later in this chapter) provides assembly isolation in .NET Core.

# The System.AppDomain Class

The AppDomain class is largely deprecated with .NET Core. While most of the remaining support is designed to make migrating from .NET 4.x to .NET Core easier, the remaining features can still provide value, as covered in the next two sections.

# Interacting with the Default Application Domain

Your application has access to the default application domain using the static AppDomain.CurrentDomain property. After you have this access point, you can use the methods and properties of AppDomain to perform some runtime diagnostics.

To learn how to interact with the default application domain, begin by creating a new Console Application project named DefaultAppDomainApp. Now, update your Program.cs class with the following logic, which will simply display some details about the default application domain, using a number of members of the AppDomain class:

```
using System;
using System.IO;
using System.Linq;
using System.Reflection;
using System.Runtime.Loader;

Console.WriteLine("***** Fun with the default AppDomain ****\n");
DisplayDADStats();
Console.ReadLine();

static void DisplayDADStats()
{
    // Get access to the AppDomain for the current thread.
    AppDomain defaultAD = AppDomain.CurrentDomain;
    // Print out various stats about this domain.
    Console.WriteLine("Name of this domain: {0}",
        defaultAD.FriendlyName);
    Console.WriteLine("ID of domain in this process: {0}",
        defaultAD.Id);
    Console.WriteLine("Is this the default domain?: {0}",
        defaultAD.IsDefaultAppDomain());
    Console.WriteLine("Base directory of this domain: {0}",
        defaultAD.BaseDirectory);
    Console.WriteLine("Setup Information for this domain:");
    Console.WriteLine("\t Application Base: {0}",
        defaultAD.SetupInformation.ApplicationBase);
    Console.WriteLine("\t Target Framework: {0}",
        defaultAD.SetupInformation.TargetFrameworkName);
}
```