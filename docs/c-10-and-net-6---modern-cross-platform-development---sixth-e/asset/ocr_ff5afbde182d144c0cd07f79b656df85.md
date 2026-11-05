# Adding packages to a project in Visual Studio 2022

Visual Studio has a graphical user interface for adding packages.

+   1. In **Solution Explorer**, right-click the **Instrumenting** project and select **Manage NuGet Packages**.
+   2. Select the **Browse** tab.
+   3. In the search box, enter Microsoft.Extensions.Configuration.
+   4. Select each of these NuGet packages and click the **Install** button, as shown in Figure 4.18:
+      1. Microsoft.Extensions.Configuration
+      2. Microsoft.Extensions.Configuration.Binder
+      3. Microsoft.Extensions.Configuration.Json
+      4. Microsoft.Extensions.Configuration.FileExtensions

![](bbox=[0.13, 0.356, 0.89, 0.588])

Figure 4.18: Installing NuGet packages using Visual Studio 2022

**Good Practice**: There are also packages for loading configuration from XML files, INI files, environment variables, and the command line. Use the most appropriate technique for setting configuration in your projects.

# Reviewing project packages

After adding the NuGet packages, we can see the references in the project file:

+   1. Open Instrumenting.csproj (double-click the **Instrumenting** project in Visual Studio's **Solution Explorer**) and note the <ItemGroup> section with the added NuGet packages, as shown highlighted in the following markup:

```
<Project Sdk="Microsoft.NET.Sdk">

  <PropertyGroup>
```