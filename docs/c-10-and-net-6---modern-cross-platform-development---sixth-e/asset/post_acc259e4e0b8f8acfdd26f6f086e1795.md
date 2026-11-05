
2. Build the Northwind.BlazorServer project.

3. In Program.cs, import the namespace for working with the Northwind database context, as shown in the following code:

```csharp
using Packt.Shared; // AddNorthwindContext extension method
```

4. In the section that configures services, add a statement to register the Northwind database context in the dependency services collection, as shown in the following code:

```csharp
builder.Services.AddNorthwindContext();
```

5. Open _Imports.razor and import namespaces for working with the Northwind entities so that Blazor components that we build do not need to import the namespaces individually, as shown in the following markup:

```razor
@using Packt.Shared @* Northwind entities *@
```

> The _Imports.razor file only applies to .razor files. If you use code-behind .cs files to implement component code, then they must have namespaces imported separately or use global usings to implicitly import the namespace.

6. In the Pages folder, in Customers.razor, add statements to inject the Northwind database context and then use it to output a table of all customers, as shown in the following code:

```razor
@using Microsoft.EntityFrameworkCore  @* ToListAsync extension method *@
@page "/customers/{country?}"
@inject NorthwindContext db

<h3>Customers @(string.IsNullOrWhiteSpace(Country)
    ? "Worldwide" : "in " + Country)</h3>

@if (customers == null)
{
<p><em>Loading...</em></p>
}
else
{
<table class="table">
    <thead>
        <tr>
            <th>Id</th>
            <th>Company Name</th>
            <th>Address</th>
            <th>Phone</th>
            <th></th>
        </tr>
</thead>
<tbody>
@foreach (Customer c in customers)
{
    <tr>
        <td>@c.CustomerId</td>
        <td>@c.CompanyName</td>
        <td>
            @c.Address<br/>
            @c.City<br/>
            @c.PostalCode<br/>
            @c.Country
        </td>
        <td>@c.Phone</td>
        <td>
            <a class="btn btn-info" href="editcustomer/@c.CustomerId">
                <i class="oi oi-pencil"></i></a>
            <a class="btn btn-danger"
                href="deletecustomer/@c.CustomerId">
                <i class="oi oi-trash"></i></a>
        </td>
    </tr>
}
</tbody>
</table>
}
@code {
    [Parameter]
    public string? Country { get; set; }

    private IEnumerable<Customer>? customers;

    protected override async Task OnParametersSetAsync()
    {
        if (string.IsNullOrWhiteSpace(Country))
        {
            customers = await db.Customers.ToListAsync();
        }
        else
        {
            customers = await db.Customers
                .Where(c => c.Country == Country).ToListAsync();
        }
    }
}
```

- 7. Start the Northwind.BlazorServer project website.
- 8. Start Chrome.
- 9. Navigate to https://localhost:5001/.
- 10. In the left navigation menu, click Customers Worldwide, and note that the table of customers loads from the database and renders in the web page, as shown in Figure 17.5:

![](img/c63b00dca7192cc58cfb9e1f1e64f483_772_0.png)

Figure 17.5: The list of customers worldwide

- 11. In the left navigation menu, click Customers in Germany, and note that the table of customers is filtered to only show German customers.
- 12. In the browser address bar, change Germany to UK, and note that the table of customers is filtered to only show UK customers.
- 13. In the left navigation menu, click Home, and note that the customers component also works correctly when used as an embedded component on a page.
- 14. Click any of the edit or delete buttons and note that they return a message saying Sorry, there's nothing at this address. because we have not yet implemented that functionality.
- 15. Close the browser.
- 16. Shut down the web server.

## Abstracting a service for a Blazor component

Currently, the Blazor component directly calls the Northwind database context to fetch the customers. This works fine in Blazor Server since the component executes on the server. But this component would not work when hosted in Blazor WebAssembly.

We will now create a local dependency service to enable better reuse of the components:

- 1. In the Northwind.BlazorServer project, in the Data folder, add a new file named INorthwindService.cs. (The Visual Studio project item template is named Interface.)
- 2. Modify its contents to define a contract for a local service that abstracts CRUD operations, as shown in the following code:
- 3. In the Data folder, add a new file named NorthwindService.cs and modify its contents to implement the INorthwindService interface by using the Northwind database context, as shown in the following code:

```csharp
namespace Packt.Shared;

public interface INorthwindService
{
    Task<List<Customer>> GetCustomersAsync();
    Task<List<Customer>> GetCustomersAsync(string country);
    Task<Customer?> GetCustomerAsync(string id);
    Task<Customer> CreateCustomerAsync(Customer c);
    Task<Customer> UpdateCustomerAsync(Customer c);
    Task DeleteCustomerAsync(string id);
}
```

```csharp
using Microsoft.EntityFrameworkCore;

namespace Packt.Shared;

public class NorthwindService : INorthwindService
{
    private readonly NorthwindContext db;

    public NorthwindService(NorthwindContext db)
    {
        this.db = db;
    }

    public Task<List<Customer>> GetCustomersAsync()
    {
        return db.Customers.ToListAsync();
    }

    public Task<List<Customer>> GetCustomersAsync(string country)
    {
        return db.Customers.Where(c => c.Country == country).ToListAsync();
    }

    public Task<Customer?> GetCustomerAsync(string id)
    {
        return db.Customers.FirstOrDefaultAsync
            (c => c.CustomerId == id);
    }

    public Task<Customer> CreateCustomerAsync(Customer c)
    {
        db.Customers.Add(c);
        db.SaveChangesAsync();
        return Task.FromResult(c);
    }

    public Task<Customer> UpdateCustomerAsync(Customer c)
    {
        db.Entry(c).State = EntityState.Modified;
        db.SaveChangesAsync();
        return Task.FromResult(c);
    }

    public Task DeleteCustomerAsync(string id)
    {
        Customer? customer = db.Customers.FirstOrDefaultAsync
            (c => c.CustomerId == id).Result;

        if (customer == null)
        {
            return Task.CompletedTask;
        }
        else
        {
            db.Customers.Remove(customer);
            return db.SaveChangesAsync();
        }
    }
}
```

- 4. In Program.cs, in the section that configures services, add a statement to register NorthwindService as a transient service that implements the INorthwindService interface, as shown in the following code:

```csharp
builder.Services.AddTransient<INorthwindService, NorthwindService>();
```

- 5. In the Pages folder, open Customers.razor and replace the directive to inject the Northwind database context with a directive to inject the registered Northwind service, as shown in the following code:

```razor
@inject INorthwindService service
```

- 6. Modify the OnParametersSetAsync method to call the service, as shown highlighted in the following code:

```csharp
protected override async Task OnParametersSetAsync()
{
    if (string.IsNullOrWhiteSpace(Country))
    {
        customers = await service.GetCustomersAsync();
    }
    else
    {
        customers = await service.GetCustomersAsync(Country);
    }
}
```

- 7. Start the Northwind.BlazorServer website project and confirm that it retains the same functionality as before.

## Defining forms using the EditForm component

Microsoft provides ready-made components for building forms. We will use them to provide, create, and edit functionality for customers.

Microsoft provides the EditForm component and several form elements such as InputText to make it easier to use forms with Blazor.

EditForm can have a model set to bind it to an object with properties and event handlers for custom validation, as well as recognizing standard Microsoft validation attributes on the model class, as shown in the following code:

```
<EditForm Model="@customer" OnSubmit="ExtraValidation">
    <DataAnnotationsValidator />
    <ValidationSummary />
    <InputText id="name" @bind-Value="customer.CompanyName" />
    <button type="submit">Submit</button>
</EditForm>

@code {
    private Customer customer = new();

    private void ExtraValidation()
    {
        // perform any extra validation
    }
}
```

As an alternative to a ValidationSummary component, you can use the ValidationMessage component to show a message next to an individual form element.
