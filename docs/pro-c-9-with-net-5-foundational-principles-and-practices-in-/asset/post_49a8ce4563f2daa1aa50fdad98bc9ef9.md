
Each of the repositories must implement the two constructors from the BaseRepo.

```
public CarRepo(ApplicationDbContext context) : base(context)
{
}
internal CarRepo(DbContextOptions<ApplicationDbContext> options) : base(options)
{
}
```

Add overrides for GetAll() and GetAllIgnoreQueryFilters() to include the MakeNavigation property and order by the PetName values.

```
public override IEnumerable<Car> GetAll()
    => Table
        .Include(c => c.MakeNavigation)
        .OrderBy(o => o.PetName);
```

```
public override IEnumerable<Car> GetAllIgnoreQueryFilters()
    => Table
        .Include(c => c.MakeNavigation)
        .OrderBy(o => o.PetName)
        .IgnoreQueryFilters();
```

Implement the GetAllBy() method. This method must set the query filter on the context before executing. Include the Make navigation property and sort by PetName value.

```
public IEnumerable<Car> GetAllBy(int makeId)
{
    return Table
        .Where(x => x.MakeId == makeId)
        .Include(c => c.MakeNavigation)
        .OrderBy(c => c.PetName);
}
```

Add an override for Find() to include the MakeNavigation property and ignore query filters.

```
public override Car? Find(int? id)
    => Table
        .IgnoreQueryFilters()
        .Where(x => x.Id == id)
        .Include(m => m.MakeNavigation)
        .FirstOrDefault();
```

Add the method to get a car's PetName value using the stored procedure.

```
public string GetPetName(int id)
{
    var parameterId = new SqlParameter
    {
        ParameterName = "@carId",
        SqlDbType = SqlDbType.Int,
        Value = id,
    };
```

```
var parameterName = new SqlParameter
{
    ParameterName = "@petName",
    SqlDbType = SqlDbType.NVarChar,
    Size = 50,
    Direction = ParameterDirection.Output
};

_ = Context.Database
    .ExecuteSqlRaw("EXEC [dbo].[GetPetName] @carId, @petName OUTPUT",parameterId,
        parameterName);
return (string)parameterName.Value;
}
```

## The CreditRisk Repository

Open the CreditRiskRepo.cs class and add the following using statements to the top of the file:

```
using AutoLot.Dal.EfStructures;
using AutoLot.Dal.Models.Entities;
using AutoLot.Dal.Repos.Base;
using AutoLot.Dal.Repos.Interfaces;
using Microsoft.EntityFrameworkCore;
```

Change the class to public, inherit from BaseRepo<CreditRisk>, implement ICreditRiskRepo, and add the two required constructors.

```
namespace AutoLot.Dal.Repos
{
    public class CreditRiskRepo : BaseRepo<CreditRisk>, ICreditRiskRepo
    {
        public CreditRiskRepo(ApplicationDbContext context) : base(context)
        {
        }
        internal CreditRiskRepo(
            DbContextOptions<ApplicationDbContext> options)
        : base(options)
        {
        }
    }
}
```

## The Customer Repository

Open the CustomerRepo.cs class and add the following using statements to the top of the file:

```
using System.Collections.Generic;
using System.Linq;
using AutoLot.Dal.EfStructures;
using AutoLot.Dal.Models.Entities;
using AutoLot.Dal.Repos.Base;
```

```
using AutoLot.Dal.Repos.Interfaces;
using Microsoft.EntityFrameworkCore;
```

Change the class to public, inherit from BaseRepo<Customer>, implement ICustomerRepo, and add the two required constructors.

```
namespace AutoLot.Dal.Repos
{
    public class CustomerRepo : BaseRepo<Customer>, ICustomerRepo
    {
        public CustomerRepo(ApplicationDbContext context)
            : base(context)
        {
        }
        internal CustomerRepo(
            DbContextOptions<ApplicationDbContext> options)
            : base(options)
        {
        }
    }
}
```

The final step is to add the method that returns all Customer records with their orders sorted by LastName. Add the following method to the class:

```
public override IEnumerable<Customer> GetAll()
    => Table
        .Include(c => c.Orders)
        .OrderBy(o => o.PersonalInformation.LastName);
```

## The Make Repository

Open the MakeRepo.cs class and add the following using statements to the top of the file:

```
using System.Collections.Generic;
using System.Linq;
using AutoLot.Dal.EfStructures;
using AutoLot.Dal.Models.Entities;
using AutoLot.Dal.Repos.Base;
using AutoLot.Dal.Repos.Interfaces;
using Microsoft.EntityFrameworkCore;
```

Change the class to public, inherit from BaseRepo<Make>, implement IMakeRepo, and add the two required constructors.

```
namespace AutoLot.Dal.Repos
{
    public class MakeRepo : BaseRepo<Make>, IMakeRepo
    {
        public MakeRepo(ApplicationDbContext context)
            : base(context)
```

```
{
}

internal MakeRepo(
    DbContextOptions<ApplicationDbContext> options)
    : base(options)
{
}
}
```

The final methods to override are the GetAll() methods, sorting the Make values by name.

```
public override IEnumerable<Make> GetAll()
    => Table.OrderBy(m => m.Name);
public override IEnumerable<Make> GetAllIgnoreQueryFilters()
    => Table.IgnoreQueryFilters().OrderBy(m => m.Name);
```

## The Order Repository

Open the OrderRepo.cs class and add the following using statements to the top of the file:

```
using AutoLot.Dal.EfStructures;
using AutoLot.Dal.Models.Entities;
using AutoLot.Dal.Repos.Base;
using AutoLot.Dal.Repos.Interfaces;
using Microsoft.EntityFrameworkCore;
```

Change the class to public, inherit from BaseRepo<Order>, and implement IOrderRepo.

```
namespace AutoLot.Dal.Repos
{
    public class OrderRepo : BaseRepo<Order>, IOrderRepo
    {
        public OrderRepo(ApplicationDbContext context)
            : base(context)
        {
        }

        internal OrderRepo(
            DbContextOptions<ApplicationDbContext> options)
            : base(options)
        {
        }
    }
}
```

The final method to implement is the GetOrderViewModel() method, which returns an IQueryable<CustomerOrderViewModel> from the database view.

```
public IQueryable<CustomerOrderViewModel> GetOrdersViewModel()
{
    return Context.CustomerOrderViewModels!.AsQueryable();
}
```

That completes all of the repositories. The next section will create the code to drop, create, and seed the database.

## Programmatic Database and Migration Handling

The Database property of DbContext provides programmatic methods to drop and create the database as well as run all of the migrations. Table 23-1 describes the methods related to these operations.

Table 23-1. Programmatically Working with the Database

| Member of Database | Meaning in Life |
|---|---|
| EnsureDeleted | Drops the database if it exists. Does nothing if it does not exist. |
| EnsureCreated | Creates the database if it doesn’t exist. Does nothing if it does. Creates the tables and columns based on the classes reachable from the DbSet<T> properties. Does not apply any migrations. Note: This should not be used in conjunction with migrations. |
| Migrate | Creates the database if it doesn’t exist. Applies all migrations to the database. |

As mentioned in the table, the EnsureCreated() method will create the database if it doesn’t exist and then creates the tables, columns, and indices based on the entity model. It does not apply any migrations. If you are using migrations (as we are), this will present errors when working with the database, and you will have to trick out EF Core (as we did earlier) to believe the migrations have been applied. You will also have to apply any custom SQL objects to the database manually. When you are working with migrations, always use the Migrate() method to programmatically create the database and not the EnsureCreated() method.

## Drop, Create, and Clean the Database

During development, it can be beneficial to drop and re-create the development database and then seed it with sample data. This creates an environment where testing (manual or automated) can be executed without fear of ruining other tests due to changing the data. Create a new folder named Initialization in the AutoLot.Dal project. In this folder, create a new class named SampleDataInitializer.cs. At the top of the file, update the using statements to the following:

```
using System;
using System.Collections.Generic;
using System.Linq;
using AutoLot.Dal.EfStructures;
using AutoLot.Models.Entities;
using AutoLot.Models.Entities.Base;
using Microsoft.EntityFrameworkCore;
using Microsoft.EntityFrameworkCore.Storage;
```
