```
}
    return null;
}

[HttpDelete("{id}")]
public async Task<int> DeleteCustomerAsync(string id)
{
    Customer? c = await db.Customers.FirstOrDefaultAsync
        (c => c.CustomerId == id);

    if (c != null)
    {
        db.Customers.Remove(c);
        int affected = await db.SaveChangesAsync();
        return affected;
    }
    return 0;
}
}
```

# Configuring the client for Blazor WebAssembly

Second, we can reuse the components from the Blazor Server project. Since the components will be identical, we can copy them and only need to make changes to the local implementation of the abstracted Northwind service:

+   1. In the Client project, open Northwind.BlazorWasm.Client.csproj and add statements to reference the Northwind entity models library project (not the database context project) for either SQL Server or SQLite, as shown in the following markup:
+   2. Build the Northwind.BlazorWasm.Client project.
+   3. In the Client project, open _Imports.razor and import the Packt.Shared namespace to make the Northwind entity model types available in all Blazor components, as shown in the following code:
+   4. In the Client project, in the Shared folder, open NavMenu.razor and add a NavLink element for customers worldwide and in France, as shown in the following markup:

```
<ItemGroup>
    <!-- change Sqlite to SqlServer if you prefer -->
    <ProjectReference Include="..\..\Northwind.Common.EntityModels.Sqlite\
Northwind.Common.EntityModels.Sqlite.csproj" />
</ItemGroup>
```

```
@using Packt.Shared
```

```
<div class="nav-item px-3">
    <NavLink class="nav-link" href="customers" Match="NavLinkMatch.All">
```