We will now create a local dependency service to enable better reuse of the components:

+   1. In the Northwind.BlazorServer project, in the Data folder, add a new file named INorthwindService.cs. (The Visual Studio project item template is named Interface.)
+   2. Modify its contents to define a contract for a local service that abstracts CRUD operations, as shown in the following code:
+   3. In the Data folder, add a new file named NorthwindService.cs and modify its contents to implement the INorthwindService interface by using the Northwind database context, as shown in the following code:

```
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
```

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
}
```
```