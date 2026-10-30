To change to a different .NET Core version to .NET 5, simply change the TargetFramework value to net5.0, as shown here:

```
<Project Sdk="Microsoft.NET.Sdk">
    <PropertyGroup>
        <OutputType>Exe</OutputType>
        <TargetFramework>net5.0</TargetFramework>
    </PropertyGroup>
</Project>
```

You can also change the target framework by right-clicking the project name in Solution Explorer and selecting Properties, opening the Application tab, and updating the Target Framework value, as shown in Figure 2-5.

![](bbox=[0.07, 0.32, 0.89, 0.7])

Figure 2-5. Changing the target framework for your application

# Using C# 9 Features

In prior versions of .NET, the version of C# supported by a project could be changed. With .NET Core 3.0+, the version of C# used is tied into the framework version. To confirm this, right-click the project name in Solution Explorer, and select Properties. In the Properties dialog, click Build in the left rail and then Advanced in the lower-right corner. That will bring up the dialog shown in Figure 2-6.