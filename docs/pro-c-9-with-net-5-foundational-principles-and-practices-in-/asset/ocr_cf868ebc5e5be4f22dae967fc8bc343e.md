Now that you can drop and create the database and clear the data, it’s time to create the methods that will add the sample data.

# Data Initialization

We are going to build our own data seeding system that can be run on demand. The first step is to create the sample data and then add the methods into the SampleDataInitializer used to load the sample data into the database.

# Create the Sample Data

Add a new file named SampleData.cs to the Initialization folder. Make the class public and static and update the using statements to the following:

```
using System.Collections.Generic;
using AutoLot.Dal.Entities;
using AutoLot.Dal.Entities.Owned;

namespace AutoLot.Dal.Initialization
{
    public static class SampleData
    {
    }
}
```

The file consists of five static methods that create the sample data.

```
public static List<Customer> Customers => new()
{
    new() {Id = 1, PersonalInformation = new() {FirstName = "Dave", LastName = "Brenner"}},
    new() {Id = 2, PersonalInformation = new() {FirstName = "Matt", LastName = "Walton"}},
    new() {Id = 3, PersonalInformation = new() {FirstName = "Steve", LastName = "Hagen"}},
    new() {Id = 4, PersonalInformation = new() {FirstName = "Pat", LastName = "Walton"}},
    new() {Id = 5, PersonalInformation = new() {FirstName = "Bad", LastName = "Customer"}},
};
```

```
public static List<Make> Makes => new()
{
    new() {Id = 1, Name = "VW"},
    new() {Id = 2, Name = "Ford"},
    new() {Id = 3, Name = "Saab"},
    new() {Id = 4, Name = "Yugo"},
    new() {Id = 5, Name = "BMW"},
    new() {Id = 6, Name = "Pinto"},
};
```

```
public static List<Car> Inventory => new()
{
    new() {Id = 1, MakeId = 1, Color = "Black", PetName = "Zippy"},
    new() {Id = 2, MakeId = 2, Color = "Rust", PetName = "Rusty"},
```