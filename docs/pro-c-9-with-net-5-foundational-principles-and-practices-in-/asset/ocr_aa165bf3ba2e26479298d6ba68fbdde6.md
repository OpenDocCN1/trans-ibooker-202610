# Adding the IRepo Base Interface

The IRepo base interface exposes many of the common methods used in data access. Add a new folder in the AutoLot.Dal project named Repos, and in that folder, make a new folder named Base. Add a new interface into the Repos\Base folder named IRepo. Update the using statements to the following:

```
using System;
using System.Collections.Generic;
```

The full interface is listed here:

```
namespace AutoLot.Dal.Repos.Base
{
    public interface IRepo<T>: IDisposable
    {
        int Add(T entity, bool persist = true);
        int AddRange(IEnumerable<T> entities, bool persist = true);
        int Update(T entity, bool persist = true);
        int UpdateRange(IEnumerable<T> entities, bool persist = true);
        int Delete(int id, byte[] timeStamp, bool persist = true);
        int Delete(T entity, bool persist = true);
        int DeleteRange(IEnumerable<T> entities, bool persist = true);
        T? Find(int? id);
        T? FindAsNoTracking(int id);
        T? FindIgnoreQueryFilters(int id);
        IEnumerable<T> GetAll();
        IEnumerable<T> GetAllIgnoreQueryFilters();
        void ExecuteQuery(string sql, object[] sqlParametersObjects);
        int SaveChanges();
    }
}
```

# Adding the BaseRepo

Next, add class named BaseRepo to the Repos\Base directory. This class will implement the IRepo interface and provide the core functionality for type-specific repos (coming next). Update the using statements to the following:

```
using System;
using System.Collections.Generic;
using System.Linq;
using AutoLot.Dal.EfStructures;
using AutoLot.Dal.Exceptions;
using AutoLot.Models.Entities.Base;
using Microsoft.EntityFrameworkCore;
```

Make the class generic with type T and constrain the type to BaseEntity and new(), which limits the types to classes that have a parameterless constructor. Implement the IRepo<T> interface, as follows:

```
public abstract class BaseRepo<T> : IRepo<T> where T : BaseEntity, new()
```