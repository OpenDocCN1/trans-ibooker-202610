
2. Enter the dotnet new --list or dotnet new -l command to list your currently installed templates, as shown in Figure 7.2:

![](img/c63b00dca7192cc58cfb9e1f1e64f483_320_0.png)

Figure 7.2: A list of installed dotnet new project templates

Most dotnet command-line switches have a long and a short version. For example, --list or -l. The short ones are quicker to type but more likely to be misinterpreted by you or other humans. Sometimes more typing is clearer.

## Getting information about .NET and its environment

It is useful to see what .NET SDKs and runtimes are currently installed, alongside information about the operating system, as shown in the following command:

```
dotnet --info
```

Note the results, as shown in the following partial output:

```
.NET SDK (reflecting any global.json):
  Version:    6.0.100
  Commit:     22d70b47bc

Runtime Environment:
  OS Name:      Windows
  OS Version:   10.0.19043
  OS Platform:  Windows
  RID:          win10-x64
  Base Path:    C:\Program Files\dotnet\sdk\6.0.100\

Host (useful for support):
  Version: 6.0.0
  Commit:  91ba01788d

.NET SDKs installed:
  3.1.412 [C:\Program Files\dotnet\sdk]
  5.0.400 [C:\Program Files\dotnet\sdk]
  6.0.100 [C:\Program Files\dotnet\sdk]

.NET runtimes installed:
  Microsoft.AspNetCore.All 2.1.29 [...\dotnet\shared\Microsoft.AspNetCore.All]
  ...
```

## Managing projects

The .NET CLI has the following commands that work on the project in the current folder, to manage the project:

- dotnet restore: This downloads dependencies for the project.
- dotnet build: This builds, aka compiles, the project.
- dotnet test: This builds and then runs unit tests for the project.
- dotnet run: This builds and then runs the project.
- dotnet pack: This creates a NuGet package for the project.
- dotnet publish: This builds and then publishes the project, either with dependencies or as a self-contained application.
- dotnet add: This adds a reference to a package or class library to the project.
- dotnet remove: This removes a reference to a package or class library from the project.
- dotnet list: This lists the package or class library references for the project.

## Publishing a self-contained app

Now that you have seen some example dotnet tool commands, we can publish our cross-platform console app:

- 1. At the command line, make sure that you are in the DotNetEverywhere folder.
- 2. Enter a command to build and publish the release version of the console application for Windows 10, as shown in the following command:
- 3. Note the build engine restores any needed packages, compiles the project source code into an assembly DLL, and creates a publish folder, as shown in the following output:
- 4. Enter the commands to build and publish the release versions for macOS and Linux variants, as shown in the following commands:
- 5. Open a macOS Finder window or Windows File Explorer, navigate to DotNetEverywhere\bin\Release\net6.0, and note the output folders for the various operating systems.
- 6. In the win10-x64 folder, select the publish folder, note all the supporting assemblies like Microsoft.CSharp.dll.
- 7. Select the DotNetEverywhere executable file, and note it is 161 KB, as shown in Figure 7.3:

```
dotnet publish -c Release -r win10-x64
```

```
Microsoft (R) Build Engine version 17.0.0+073022eb4 for .NET
Copyright (C) Microsoft Corporation. All rights reserved.

Determining projects to restore...
Restored C:\Code\Chapter07\DotNetEverywhere\DotNetEverywhere.csproj (in 46.89 sec).
DotNetEverywhere -> C:\Code\Chapter07\DotNetEverywhere\bin\Release\
net6.0\win10-x64\DotNetEverywhere.dll
DotNetEverywhere -> C:\Code\Chapter07\DotNetEverywhere\bin\Release\
net6.0\win10-x64\publish\
```

```
dotnet publish -c Release -r osx-x64
dotnet publish -c Release -r osx.11.0-arm64
dotnet publish -c Release -r linux-x64
dotnet publish -c Release -r linux-arm64
```

![](img/c63b00dca7192cc58cfb9e1f1e64f483_322_0.png)

Good Practice: You could automate these commands by using a scripting language like PowerShell and execute it on any operating system using the cross-platform PowerShell Core. Just create a file with the extension .ps1 with the five commands on it. Then execute the file. Learn more about PowerShell at the following link: https://github.com/markjprice/cs10dotnet6/tree/main/docs/powershell

![](img/c63b00dca7192cc58cfb9e1f1e64f483_323_0.png)

Figure 7.3: The DotNetEverywhere executable file for Windows 10 64-bit

- 8. If you are on Windows, then double-click to execute the program and note the result, as shown in the following output:

```
I can run everywhere!
OS Version is Microsoft Windows NT 10.0.19042.0.
I am Windows 10.
Press ENTER to stop me.
```

- 9. Note that the total size of the publish folder and all its files is 64.8 MB.

- 10. In the osx.11.0-arm64 folder, select the publish folder, note all the supporting assemblies, and then select the DotNetEverywhere executable file, and note the executable is 126 KB, and the publish folder is 71.8 MB.

If you copy any of those publish folders to the appropriate operating system, the console application will run; this is because it is a self-contained deployable .NET application. For example, on macOS with Intel, as shown in the following output:

```
I can run everywhere!
OS Version is Unix 11.2.3
I am macOS.
Press ENTER to stop me.
```

This example used a console app, but you could just as easily create an ASP.NET Core website or web service, or a Windows Forms or WPF app. Of course, you can only deploy Windows desktop apps to Windows computers, not Linux or macOS.

## Publishing a single-file app

To publish as a "single" file, you can specify flags when publishing. With .NET 5, single-file apps were primarily focused on Linux because there are limitations in both Windows and macOS that mean true single-file publishing is not technically possible. With .NET 6, you can now create proper single-file apps on Windows.

If you can assume that .NET 6 is already installed on the computer on which you want to run your app, then you can use the extra flags when you publish your app for release to say that it does not need to be self-contained and that you want to publish it as a single file (if possible), as shown in the following command (that must be entered on a single line):

```
dotnet publish -r win10-x64 -c Release --self-contained=false
/p:PublishSingleFile=true
```

This will generate two files: DotNetEverywhere.exe and DotNetEverywhere.pdb. The .exe is the executable. The .pdb file is a program debug database file that stores debugging information.

![](img/c63b00dca7192cc58cfb9e1f1e64f483_324_0.png)

> There is no .exe file extension for published applications on macOS, so if you use osx-x64 in the command above, the filename will not have an extension.

If you prefer the .pdb file to be embedded in the .exe file, then add a <DebugType> element to the <PropertyGroup> element in your .csproj file and set it to embedded, as shown highlighted in the following markup:

```
<PropertyGroup>

    <OutputType>Exe</OutputType>
    <TargetFramework>net6.0</TargetFramework>
    <Nullable>enable</Nullable>
    <ImplicitUsings>enable</ImplicitUsings>
    <RuntimeIdentifiers>
        win10-x64;osx-x64;osx.11.0-arm64;linux-x64;linux-arm64
    </RuntimeIdentifiers>
    <DebugType>embedded</DebugType>

</PropertyGroup>
```

If you cannot assume that .NET 6 is already installed on a computer, then although Linux also only generates the two files, expect the following additional files for Windows: coreclr.dll, clrjit.dll, clrcompression.dll, and mscordaccore.dll.

Let's see an example for Windows:

- 1. At the command line, enter the command to build the release version of the console application for Windows 10, as shown in the following command:
- 2. Navigate to the DotNetEverywhere\bin\Release\net6.0\win10-x64\publish folder, select the DotNetEverywhere executable file, and note the executable is now 58.3 MB, and there is also a .pdb file that is 10 KB. The sizes on your system will vary.

```
dotnet publish -c Release -r win10-x64 /p:PublishSingleFile=true
```
