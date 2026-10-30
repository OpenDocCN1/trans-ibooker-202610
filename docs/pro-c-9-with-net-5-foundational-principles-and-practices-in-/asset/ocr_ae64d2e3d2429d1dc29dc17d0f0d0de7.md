![](bbox=[0.124, 0.099, 0.912, 0.5])

# Figure 29-2 Selecting the ASP.NET Core Web API template

Next, add another ASP.NET Core web application to the solution. Select the ASP.NET Core Web App (Model-View-Controller) template. Make sure that .NET Core and ASP.NET Core 5.0 are selected in the top select boxes; leave the Advanced check boxes at their default values.

Finally, add a C# Class Library (.NET Core) to the project and name it AutoLot.Services. Edit the project file to set TargetFramework to net5.0, as shown here:

```
<PropertyGroup>
    <TargetFramework>net5.0</TargetFramework>
</PropertyGroup>
```

# Add in AutoLot.Models and AutoLot.Dal

The solution requires the completed data access layer from Chapter 23. You can either copy the files into the current solution directory or leave them in place. Either way, you need to right-click your solution name in Solution Explorer, select Add ► Existing Project, and navigate to the AutoLot.Models.csproj file and select it. Repeat for the AutoLot.Dal project.

> Note While the order that the projects are added to the solution doesn’t technically matter, Visual Studio will preserve the references between AutoLot.Models and AutoLot.Dal if the models project is added first.