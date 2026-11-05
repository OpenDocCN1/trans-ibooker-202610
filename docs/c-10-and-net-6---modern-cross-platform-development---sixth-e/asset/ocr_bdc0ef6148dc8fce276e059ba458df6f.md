```
optionsBuilder.UseSqlite($"Filename={path}");
}
else
{
    string connection = "Data Source=.;" +
        "Initial Catalog=Northwind;" +
        "Integrated Security=true;" +
        "MultipleActiveResultSets=true;";

    optionsBuilder.UseSqlServer(connection);
}
}
```

If you are using Visual Studio for Windows, then the compiled application executes in the WorkingWithEFCore\bin\Debug\net6.0 folder so it will not find the database file.

8. In Solution Explorer, right-click the Northwind.db file and select Properties.

9. In Properties, set Copy to Output Directory to Copy always.

10. Open WorkingWithEFCore.csproj and note the new elements, as shown in the following markup:

```
<ItemGroup>
    <None Update="Northwind.db">
        <CopyToOutputDirectory>Always</CopyToOutputDirectory>
    </None>
</ItemGroup>
```

If you are using Visual Studio Code, then the compiled application executes in the WorkingWithEFCore folder so it will find the database file without it being copied.

11. Run the console application and note the output showing which database provider you chose to use.

# Defining EF Core models

EF Core uses a combination of conventions, annotation attributes, and Fluent API statements to build an entity model at runtime so that any actions performed on the classes can later be automatically translated into actions performed on the actual database. An entity class represents the structure of a table and an instance of the class represents a row in that table.

First, we will review the three ways to define a model, with code examples, and then we will create some classes that implement those techniques.