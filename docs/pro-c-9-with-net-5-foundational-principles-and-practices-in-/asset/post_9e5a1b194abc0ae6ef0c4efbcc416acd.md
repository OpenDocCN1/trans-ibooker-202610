
```
public string[] Verbs { get; }
public ProcessWindowStyle WindowStyle { get; set; }
public string WorkingDirectory { get; set; }
}
```

To illustrate how to fine-tune your process startup, here is a modified version of StartAndKillProcess(), which will load Microsoft Edge and navigate to www.facebook.com, using the windows association MsEdge:

```
static void StartAndKillProcess()
{
    Process proc = null;

    // Launch Microsoft Edge, and go to Facebook, with maximized window.
    try
    {
        ProcessStartInfo startInfo = new
            ProcessStartInfo("MsEdge", "www.facebook.com");
        startInfo.UseShellExecute = true;
        proc = Process.Start(startInfo);
    }
    catch (InvalidOperationException ex)
    {
        Console.WriteLine(ex.Message);
    }
    ...
}
```

In .NET Core, the UseShellExecute property defaults to false, while in prior versions of .NET, the UseShellExecute property defaults to true. This is the reason that the previous version of Process.Start(), shown here, no longer works without using ProcessStartInfo and setting the UseShellExecute property to true:

```
Process.Start("msedge")
```

## Leveraging OS Verbs with ProcessStartInfo

In addition to using the OS shortcuts to launch applications, you can also take advantage of file associations with ProcessStartInfo. On Windows, if you right-click a Word document, there are options to edit or print the document. Let’s use the ProcessStartInfo to determine the verbs available and then use them to manipulate the process.

Create a new method with the following code:

```
static void UseApplicationVerbs()
{
    int i = 0;
    //adjust this path and name to a document on your machine
    ProcessStartInfo si =
        new ProcessStartInfo("@..\TestPage.docx");
    foreach (var verb in si.Verbs)
    {
        Console.WriteLine($" {i++}. {verb}");
    }
}
```

```
si.WindowStyle = ProcessWindowStyle.Maximized;
si.Verb = "Edit";
si.UseShellExecute = true;
Process.Start(si);
}
```

When you run this code, the first part prints out all the available verbs for a Word document, as the following shows:

```
**** Fun with Processes ****
0. Edit
1. OnenotePrintto
2. Open
3. OpenAsReadOnly
4. Print
5. Printto
6. ViewProtected
```

After setting WindowStyle to maximized, the verb is set to Edit, which opens the document in edit mode. If you set the verb to Print, the document will be sent straight to the printer.

Now that you understand the role of Windows processes and how to interact with them from C# code, you are ready to investigate the concept of a .NET application domain.

> Note The directory in which the application runs is dependent on how you run the sample application. If you use the CLI command dotnet run, the current directory is the same as where the project file is located. If you are using Visual Studio, the current directory will be the directory of the compiled assembly, which is .\bin\debug\net5.0. You will need to adjust the path to the Word document accordingly.

## Understanding .NET Application Domains

Under the .NET and .NET Core platforms, executables are not hosted directly within a Windows process, as is the case in traditional unmanaged applications. Rather, .NET and .NET Core executables are hosted by a logical partition within a process called an application domain. This partition of a traditional Windows process offers several benefits, some of which are as follows:

- AppDomains are a key aspect of the OS-neutral nature of the .NET Core platform, given that this logical division abstracts away the differences in how an underlying OS represents a loaded executable.
- AppDomains are far less expensive in terms of processing power and memory than a full-blown process. Thus, the CoreCLR can load and unload application domains much quicker than a formal process and can drastically improve scalability of server applications.

AppDomains are fully and completely isolated from other AppDomains within a process. Given this fact, be aware that an application running in one AppDomain is unable to obtain data of any kind (global variables or static fields) within another AppDomain, unless they use a distributed programming protocol.

Note Support for AppDomains is changed in .NET Core. In .NET Core, there is exactly one AppDomain. Creation of new AppDomains is no longer supported because they require runtime support and are generally expensive to create. The ApplicationLoadContext (covered later in this chapter) provides assembly isolation in .NET Core.

## The System.AppDomain Class

The AppDomain class is largely deprecated with .NET Core. While most of the remaining support is designed to make migrating from .NET 4.x to .NET Core easier, the remaining features can still provide value, as covered in the next two sections.

## Interacting with the Default Application Domain

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

The output of this example is shown here:

```
**** Fun with the default AppDomain ****
Name of this domain: DefaultAppDomainApp
ID of domain in this process: 1
Is this the default domain?: True
Base directory of this domain: C:\GitHub\Books\csharp8-wf\Code\Chapter_14\
DefaultAppDomainApp\DefaultAppDomainApp\bin\Debug\net5.0\
Setup Information for this domain:
    Application Base: C:\GitHub\Books\csharp8-wf\Code\Chapter_14\DefaultAppDomainApp\
DefaultAppDomainApp\bin\Debug\net5.0\
    Target Framework: .NETCoreApp,Version=v5.0
```

Notice that the name of the default application domain will be identical to the name of the executable that is contained within it (DefaultAppDomainApp.exe, in this example). Also notice that the base directory value, which will be used to probe for externally required private assemblies, maps to the current location of the deployed executable.

## Enumerating Loaded Assemblies

It is also possible to discover all the loaded .NET Core assemblies within a given application domain using the instance-level GetAssemblies() method. This method will return to you an array of Assembly objects (covered in Chapter 17). To do this, you must have added the System.Reflection namespace to your code file (as you did earlier in this section).

To illustrate, define a new method named ListAllAssembliesInAppDomain() within the Program class. This helper method will obtain all loaded assemblies and print the friendly name and version of each.

```
static void ListAllAssembliesInAppDomain()
{
    // Get access to the AppDomain for the current thread.
    AppDomain defaultAD = AppDomain.CurrentDomain;

    // Now get all loaded assemblies in the default AppDomain.
    Assembly[] loadedAssemblies = defaultAD.GetAssemblies();
    Console.WriteLine("**** Here are the assemblies loaded in {0} ****\n", defaultAD.FriendlyName);
    foreach(Assembly a in loadedAssemblies)
    {
        Console.WriteLine("$-> Name, Version: {a.GetName().Name}:{a.GetName().Version} ");
    }
}
```

Assuming you have updated your top-level statements to call this new member, you will see that the application domain hosting your executable is currently using the following .NET Core libraries:

```
**** Here are the assemblies loaded in DefaultAppDomainApp ****
-> Name, Version: System.Private.CoreLib:5.0.0.0
-> Name, Version: DefaultAppDomainApp:1.0.0.0
-> Name, Version: System.Runtime:5.0.0.0
```
