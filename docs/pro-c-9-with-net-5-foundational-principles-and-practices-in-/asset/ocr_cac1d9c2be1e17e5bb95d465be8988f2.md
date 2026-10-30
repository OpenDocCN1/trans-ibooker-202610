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

# The Make Repository

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