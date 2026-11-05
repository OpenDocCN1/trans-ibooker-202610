```
<Project Sdk="Microsoft.NET.Sdk">

  <PropertyGroup>
    <OutputType>Exe</OutputType>
    <TargetFramework>net6.0</TargetFramework>
    <Nullable>enable</Nullable>
    <ImplicitUsings>enable</ImplicitUsings>
    <RuntimeIdentifiers>
      win10-x64;osx-x64;osx.11.0-arm64;linux-x64;linux-arm64
    </RuntimeIdentifiers>
  </PropertyGroup>

</Project>
```

+   - The win10-x64 RID value means Windows 10 or Windows Server 2016 64-bit. You could also use the win10-arm64 RID value to deploy to a Microsoft Surface Pro X.
+   - The osx-x64 RID value means macOS Sierra 10.12 or later. You can also specify version-specific RID values like osx.10.15-x64 (Catalina), osx.11.0-x64 (Big Sur on Intel), or osx.11.0-arm64 (Big Sur on Apple Silicon).
+   - The linux-x64 RID value means most desktop distributions of Linux like Ubuntu, CentOS, Debian, or Fedora. Use linux-arm for Raspbian or Raspberry Pi OS 32-bit. Use linux-arm64 for a Raspberry Pi running Ubuntu 64-bit.

# Understanding dotnet commands

When you install the .NET SDK, it includes a command-line interface (CLI) named dotnet.

# Creating new projects

The .NET CLI has commands that work on the current folder to create a new project using templates:

+   1. On Windows, start Command Prompt or Windows Terminal. On macOS, start Terminal. If you are using Visual Studio Code, then you can use the integrated terminal.