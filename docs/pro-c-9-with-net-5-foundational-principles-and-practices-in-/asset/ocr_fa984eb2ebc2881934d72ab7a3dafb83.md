# Update the DeleteCar Method

Likewise, the original implementation of the DeleteCar() method used C# string interpolation. To update this method, create an instance of SqlParameter with the appropriate values, as follows:

```
SqlParameter param = new SqlParameter
{
    ParameterName = "@carId",
    Value = id,
    SqlDbType = SqlDbType.Int,
    Direction = ParameterDirection.Input
};
```

Next, update the SQL string to use the parameter name ("@carId").

```
string sql = "Delete from Inventory where Id = @carId";
```

The final update is to add the new parameter to the Parameters collection of the command object.

```
command.Parameters.Add(param);
```

# Update the UpdateCarPetName Method

This method requires two parameters, one for the car Id and the other for the new PetName. The first parameter is created just like the two previous examples (with the exception of a different variable name), and the second creates a parameter that maps to the database NVarChar type (the PetName field type from the Inventory table). Notice that a Size value is set. It is important this size matches your database field size so as to not create problems when executing the command.

```
SqlParameter paramId = new SqlParameter
{
    ParameterName = "@carId",
    Value = id,
    SqlDbType = SqlDbType.Int,
    Direction = ParameterDirection.Input
};
SqlParameter paramName = new SqlParameter
{
    ParameterName = "@petName",
    Value = newPetName,
    SqlDbType = SqlDbType.NVarChar,
    Size = 50,
    Direction = ParameterDirection.Input
};
```

Next, update the SQL string to use the parameters.

```
string sql = $"Update Inventory Set PetName = @petName Where Id = @carId";
```