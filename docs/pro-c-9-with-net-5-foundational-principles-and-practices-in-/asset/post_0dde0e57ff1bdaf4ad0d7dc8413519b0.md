
The repo needs an instance of the ApplicationDbContext injected into a constructor. When used with the ASP.NET Core DI container, the container will handle the lifetime of the context. A second constructor will accept DbContextOptions and need to create an instance of the ApplicationDbContext. That context will need to be disposed. Because this class is abstract, both constructors are protected. Add the following code for the public ApplicationDbContext, the two constructors, and the Dispose pattern:

```
private readonly bool _disposeContext;
public ApplicationDbContext Context { get; }

protected BaseRepo(ApplicationDbContext context)
{
    Context = context;
    _disposeContext = false;
}

protected BaseRepo(DbContextOptions<ApplicationDbContext> options) : this(new ApplicationDbContext(options))
{
    _disposeContext = true;
}

public void Dispose()
{
    Dispose(true);
    GC.SuppressFinalize(this);
}
private bool _isDisposed;
protected virtual void Dispose(bool disposing)
{
    if (_isDisposed)
    {
        return;
    }

    if (disposing)
    {
        if (_disposeContext)
        {
            Context.Dispose();
        }
    }
    _isDisposed = true;
}

~BaseRepo()
{
    Dispose(false);
}
```

The DbSet<T> properties of ApplicationDbContext can be referenced by using the Context.Set<T>() method. Create a public property named Table of type DbSet<T> and set the value in the initial constructor, like this:

```
public DbSet<T> Table { get; }
protected BaseRepo(ApplicationDbContext context)
{
    Context = context;
    Table = Context.Set<T>();
    _disposeContext = false;
}
```

## Implement the SaveChanges Method

The BaseRepo has a SaveChanges() that calls into the overridden SaveChanges() method that demonstrates the custom exception pattern. Add the following code to the BaseRepo class:

```
public int SaveChanges()
{
    try
    {
        return Context.SaveChanges();
    }
    catch (CustomException ex)
    {
        //Should handle intelligently - already logged
        throw;
    }
    catch (Exception ex)
    {
        //Should log and handle intelligently
        throw new CustomException("An error occurred updating the database", ex);
    }
}
```

## Implement the Common Read Methods

The next series of methods returns records using LINQ statements. The Find() method takes the primary key value(s) and searches the ChangeTracker first. If the entity is already being tracked, the tracked instance is returned. If not, the record is retrieved from the database.

```
public virtual T? Find(int? id) => Table.Find(id);
```

The two additional Find() methods extend the Find() base method. The next method demonstrates retrieving a record but not adding it to the ChangeTracker using AsNoTrackingWithIdentityResolution(). Add the following code to the class:

```
public virtual T? FindAsNoTracking(int id) =>
    Table.AsNoTrackingWithIdentityResolution().FirstOrDefault(x => x.Id == id);
```

The next variation removes the query filters from the entity and then uses the shorthand version (skipping the Where() method) to get FirstOrDefault(). Add the following to the class:

```
public T? FindIgnoreQueryFilters(int id) =>
    Table.IgnoreQueryFilters().FirstOrDefault(x => x.Id == id);
```

The GetAll() methods return all of the records from the table. The first retrieves them in database order, and the second takes turns of any query filters.

```
public virtual IEnumerable<T> GetAll() => Table;
public virtual IEnumerable<T> GetAllIgnoreQueryFilters()
    => Table.IgnoreQueryFilters();
```

The ExecuteQuery() method is there to execute stored procedures:

```
public void ExecuteQuery(string sql, object[] sqlParametersObjects)
    => Context.Database.ExecuteSqlRaw(sql, sqlParametersObjects);
```

## The Add, Update, and Delete Methods

The next block of code to be added wraps the matching Add(), Update(), and Remove() methods on the specific DbSet<T> property. The persist parameter determines whether the repo executes SaveChanges() immediately when the Add()/Update()/Remove() repository methods are called. All of the methods are marked virtual to allow for downstream overriding. Add the following code to your class:

```
public virtual int Add(T entity, bool persist = true)
{
    Table.Add(entity);
    return persist ? SaveChanges() : 0;
}
public virtual int AddRange(IEnumerable<T> entities, bool persist = true)
{
    Table.AddRange(entities);
    return persist ? SaveChanges() : 0;
}
public virtual int Update(T entity, bool persist = true)
{
    Table.Update(entity);
    return persist ? SaveChanges() : 0;
}
public virtual int UpdateRange(IEnumerable<T> entities, bool persist = true)
{
    Table.UpdateRange(entities);
    return persist ? SaveChanges() : 0;
}
public virtual int Delete(T entity, bool persist = true)
{
    Table.Remove(entity);
    return persist ? SaveChanges() : 0;
}
public virtual int DeleteRange(IEnumerable<T> entities, bool persist = true)
{
    Table.RemoveRange(entities);
    return persist ? SaveChanges() : 0;
}
```

There is one more Delete() method that doesn’t follow the same pattern. This method uses EntityState to conduct the delete operation, which is used fairly often in ASP.NET Core operations to cut down on the network traffic. It is listed here:

```
public int Delete(int id, byte[] timeStamp, bool persist = true)
{
    var entity = new T {Id = id, TimeStamp = timeStamp};
    Context.Entry(entity).State = EntityState.Deleted;
    return persist ? SaveChanges() : 0;
}
```

This concludes the BaseRepo class, and now it’s time to build the entity-specific repos.

## Entity-Specific Repo Interfaces

Each entity will have a strongly typed repository derived from BaseRepo<T> and an interface that implements IRepo<T>. Add a new folder named Interfaces under the Repos directory in the AutoLot.Dal project. In this new directory, add five interfaces.

- ICarRepo.cs
- ICreditRiskRepo.cs
- ICustomerRepo.cs
- IMakeIRepo.cs
- IOrderRepo.cs

The next sections complete the interfaces.

## The Car Repository Interface

Open the ICarRepo.cs interface. Add the following using statements to the top of the file:

```
using System.Collections.Generic;
using AutoLot.Models.Entities;
using AutoLot.Dal.Repos.Base;
```

Change the interface to public and implement IRepo<Category> as follows:

```
namespace AutoLot.Dal.Repos.Interfaces
{
    public interface ICarRepo : IRepo<Car>
    {
        IEnumerable<Car> GetAllBy(int makeId);
        string GetPetName(int id);
    }
}
```

## The Credit Risk Interface

Open the ICreditRiskRepo.cs interface. This interface doesn’t add any functionality beyond what is provided in the BaseRepo. Update the code to the following:

```
using AutoLot.Models.Entities;
using AutoLot.Dal.Repos.Base;
namespace AutoLot.Dal.Repos.Interfaces
{
    public interface ICreditRiskRepo : IRepo<CreditRisk>
    {
    }
}
```

## The Customer Repository Interface

Open the ICustomerRepo.cs interface. This interface doesn’t add any functionality beyond what is provided in the BaseRepo. Update the code to the following:

```
using AutoLot.Models.Entities;
using AutoLot.Dal.Repos.Base;
namespace AutoLot.Dal.Repos.Interfaces
{
    public interface ICustomerRepo : IRepo<Customer>
    {
    }
}
```

## The Make Repository Interface

Open the IMakeRepo.cs interface. This interface doesn’t add any functionality beyond what is provided in the BaseRepo. Update the code to the following:

```
using AutoLot.Models.Entities;
using AutoLot.Dal.Repos.Base;
namespace AutoLot.Dal.Repos.Interfaces
{
    public interface IMakeRepo : IRepo<Make>
    {
    }
}
```

## The Order Repository Interface

Open the IOrderRepo.cs interface. Add the following using statements to the top of the file:

```
using System.Collections.Generic;
using System.Linq;
using AutoLot.Models.Entities;
using AutoLot.Dal.Repos.Base;
using AutoLot.Models.ViewModels;
```

Change the interface to public and implement IRepo<Order> as follows:

```
namespace AutoLot.Dal.Repos.Interfaces
{
    public interface IOrderRepo : IRepo<Order>
    {
        IQueryable<CustomerOrderViewModel> GetOrdersViewModel();
    }
}
```

This completes the interface as all of the necessary API endpoints are covered in the base class.

## Implement the Entity-Specific Repositories

The implemented repositories gain most of their functionality from the base class. This section covers the functionality added to or overridden from the base repository. In the Repos directory of the AutoLot.Dal project, add the five repo classes.

- CarRepo.cs
- CreditRiskRepo.cs
- CustomerRepo.cs
- MakeRepo.cs
- OrderRepo.cs

The next sections complete the repositories.

## The Car Repository

Open the CarRepo.cs class and add the following using statements to the top of the file:

```
using System.Collections.Generic;
using System.Data;
using System.Linq;
using AutoLot.Dal.EfStructures;
using AutoLot.Models.Entities;
using AutoLot.Dal.Repos.Base;
using AutoLot.Dal.Repos.Interfaces;
using Microsoft.Data.SqlClient;
using Microsoft.EntityFrameworkCore;
```

Change the class to public, inherit from BaseRepo<Car>, and implement ICarRepo.

```
namespace AutoLot.Dal.Repos
{
    public class CarRepo : BaseRepo<Car>, ICarRepo
    {
    }
}
```
