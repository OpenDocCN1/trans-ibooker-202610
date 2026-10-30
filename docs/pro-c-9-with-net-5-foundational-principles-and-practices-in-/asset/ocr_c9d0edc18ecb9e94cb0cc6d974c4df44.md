# Configure your new project

Class Library (.NET Core) C# Windows Linux macOS Library

Project name
CommonSnappableTypes

Location
C:\GitHub\Books\csharp8-wf\Code\Chapter_17

Solution
Create new solution

Solution name
ExtendableApp

Place solution and project in the same directory

Back Create

# Figure 17-3. Creating the CommonSnappableTypes project and the ExtendableApp solution

To create the ExtendableApp solution, start by selecting File ► New Project to load the New Project dialog. Select Class Library and enter the name CommonSnappableTypes. Before you click OK, enter the solution name ExtendableApp, as shown in Figure 17-3.

To add another project into the solution, right-click the solution name (ExtendableApp) in Solution Explorer (or click File ► Add ► New Project) and select Add ► New Project. When adding another project into an existing solution, the Add New Project dialog box is a little different now; the solution options are no longer there, so you will see just the project information (name and location). Name the Class Library project CSharpSnapIn and click Create.

Next, add a reference to the CommonSnappableTypes project from the CSharpSnapIn project. To do this in Visual Studio, right-click the CSharpSnapIn project and select Add ► Project Reference. In the Reference Manager dialog, select Projects ► Solution from the left (if not already selected) and check the box next to CommonSnappableTypes.

Repeat the process for a new Visual Basic class library (VBSnapIn) that references the CommonSnappableTypes project.

The final project to add is a .NET Core Console app named MyExtendableApp. Add a reference to the CommonSnappableTypes project and set the console app as the startup project for the solution. To do this, right-click the MyExtendableApp project in Solution Explorer and select Set as StartUp Project.

> Note If you right-click the ExtendableApp solution instead of one of the projects, the context menu option displayed is Set StartUp Projects. In addition to having just one project execute when you click Run, you can set up multiple projects to execute. This will be demonstrated in later chapters.