![](bbox=[0.175, 0.065, 0.93, 0.312])

Figure 16.19: Adding a custom HTTP header named super-secure

# Building web services using minimal APIs

For .NET 6, Microsoft put a lot of effort into adding new features to the C# 10 language and simplifying the ASP.NET Core libraries to enable the creation of web services using minimal APIs.

You might remember the weather forecast service that is provided in the Web API project template. It shows the use of a controller class to return a five-day weather forecast using faked data. We will now recreate that weather service using minimal APIs.

First, the weather service has a class to represent a single weather forecast. We will need to use this class in multiple projects, so let's create a class library for that:

+   1. Use your preferred code editor to add a new project, as defined in the following list:
+      1. Project template: Class Library / classlib
+      2. Workspace/solution file and folder: PracticalApps
+      3. Project file and folder: Northwind.Common
+   2. Rename Class1.cs to WeatherForecast.cs.
+   3. Modify WeatherForecast.cs, as shown in the following code:

```
```csharp
namespace Northwind.Common
{
    public class WeatherForecast
    {
        public static readonly string[] Summaries = new[]
        {
```
```