# Creating a class library for entity models using SQLite

You will now define entity data models in a class library so that they can be reused in other types of projects including client-side app models. If you are not using SQL Server, you will need to create this class library for SQLite. If you are using SQL Server, then you can create both a class library for SQLite and one for SQL Server and then switch between them as you choose.

We will automatically generate some entity models using the EF Core command-line tool:

+   1. Use your preferred code editor to create a new solution/workspace named `PracticalApps`.
+   2. Add a class library project, as defined in the following list:
+      1. Project template: **Class Library** / `classlib`
+      2. Workspace/solution file and folder: `PracticalApps`
+      3. Project file and folder: `Northwind.Common.EntityModels.Sqlite`
+   3. In the `Northwind.Common.EntityModels.Sqlite` project, add package references for the SQLite database provider and EF Core design-time support, as shown in the following markup:
+   4. Delete the `Class1.cs` file.
+   5. Build the project.
+   6. Create the `Northwind.db` file for SQLite by copying the `Northwind4SQLite.sql` file into the `PracticalApps` folder, and then enter the following command at a command prompt or terminal:
+   7. Be patient because this command might take a while to create the database structure, as shown in the following output:

```
<ItemGroup>
  <PackageReference Include="Microsoft.EntityFrameworkCore.Sqlite" Version="6.0.0" />
  <PackageReference Include="Microsoft.EntityFrameworkCore.Design" Version="6.0.0">
    <PrivateAssets>all</PrivateAssets>
    <IncludeAssets>runtime; build; native; contentfiles; analyzers; buildtransitive</IncludeAssets>
  </PackageReference>
</ItemGroup>
```

```
sqlite3 Northwind.db -init Northwind4SQLite.sql
```