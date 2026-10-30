```
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

# Entity-Specific Repo Interfaces

Each entity will have a strongly typed repository derived from BaseRepo<T> and an interface that implements IRepo<T>. Add a new folder named Interfaces under the Repos directory in the AutoLot.Dal project. In this new directory, add five interfaces.

+   - ICarRepo.cs
+   - ICreditRiskRepo.cs
+   - ICustomerRepo.cs
+   - IMakeIRepo.cs
+   - IOrderRepo.cs

The next sections complete the interfaces.

# The Car Repository Interface

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