
## Table 21-6. Key Members of the DbParameter Type

| Property | Meaning in Life |
|---|---|
| DbType | Gets or sets the native data type of the parameter, represented as a CLR data type |
| Direction | Gets or sets whether the parameter is input-only, output-only, bidirectional, or a return value parameter |
| IsNullable | Gets or sets whether the parameter accepts null values |
| ParameterName | Gets or sets the name of the DbParameter |
| Size | Gets or sets the maximum parameter size of the data in bytes; this is useful only for textual data |
| Value | Gets or sets the value of the parameter |

Now let’s look at how to populate a command object’s collection of DbParameter-compatible objects by reworking the InventoryDal methods to use parameters.

## Update the GetCar Method

The original implementation of the GetCar() method used C# string interpolation when building the SQL string to retrieve the car data. To update this method, create an instance of SqlParameter with the appropriate values, as follows:

```csharp
SqlParameter param = new SqlParameter
{
    ParameterName = "@carId",
    Value = id,
    SqlDbType = SqlDbType.Int,
    Direction = ParameterDirection.Input
};
```

The ParameterName value must match the name used in the SQL query (you will update that next), the type must match the database column type, and the direction is dependent on whether the parameter is used to send data into the query (ParameterDirection.Input) or if it is meant to return data from the query (ParameterDirection.Output). Parameters can also be defined as input/output or as return values (e.g., from a stored procedure).

Next, update the SQL string to use the parameter name ("@carId") instead of the C# string interpolation construct ("{id}").

```csharp
string sql =
    @"SELECT i.Id, i.Color, i.PetName,m.Name as Make
      FROM Inventory i
      INNER JOIN Makes m on m.Id = i.MakeId
      WHERE i.Id = @CarId";
```

The final update is to add the new parameter to the Parameters collection of the command object.

```csharp
command.Parameters.Add(param);
```

## Update the DeleteCar Method

Likewise, the original implementation of the DeleteCar() method used C# string interpolation. To update this method, create an instance of SqlParameter with the appropriate values, as follows:

```csharp
SqlParameter param = new SqlParameter
{
    ParameterName = "@carId",
    Value = id,
    SqlDbType = SqlDbType.Int,
    Direction = ParameterDirection.Input
};
```

Next, update the SQL string to use the parameter name ("@carId").

```csharp
string sql = "Delete from Inventory where Id = @carId";
```

The final update is to add the new parameter to the Parameters collection of the command object.

```csharp
command.Parameters.Add(param);
```

## Update the UpdateCarPetName Method

This method requires two parameters, one for the car Id and the other for the new PetName. The first parameter is created just like the two previous examples (with the exception of a different variable name), and the second creates a parameter that maps to the database NVarChar type (the PetName field type from the Inventory table). Notice that a Size value is set. It is important this size matches your database field size so as to not create problems when executing the command.

```csharp
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

```csharp
string sql = $"Update Inventory Set PetName = @petName Where Id = @carId";
```

The final update is to add the new parameters to the Parameters collection of the command object.

```csharp
command.Parameters.Add(paramId);
command.Parameters.Add(paramName);
```

## Update the InsertAuto Method

Add the following version of the InsertAuto() method to leverage parameter objects:

```csharp
public void InsertAuto(Car car)
{
    OpenConnection();
    // Note the "placeholders" in the SQL query.
    string sql = "Insert Into Inventory" +
    "(MakeId, Color, PetName) Values" +
    "(@MakeId, @Color, @PetName)";

    // This command will have internal parameters.
    using (SqlCommand command = new SqlCommand(sql, _sqlConnection))
    {
        // Fill params collection.
        SqlParameter parameter = new SqlParameter
        {
            ParameterName = "@MakeId",
            Value = car.MakeId,
            SqlDbType = SqlDbType.Int,
            Direction = ParameterDirection.Input
        };
        command.Parameters.Add(parameter);

        parameter = new SqlParameter
        {
            ParameterName = "@Color",
            Value = car.Color,
            SqlDbType = SqlDbType.NVarChar,
            Size = 50,
            Direction = ParameterDirection.Input
        };
        command.Parameters.Add(parameter);

        parameter = new SqlParameter
        {
            ParameterName = "@PetName",
            Value = car.PetName,
            SqlDbType = SqlDbType.NVarChar,
            Size = 50,
            Direction = ParameterDirection.Input
        };
        command.Parameters.Add(parameter);
    }

    command.ExecuteNonQuery();
    CloseConnection();
}
}
```

While building a parameterized query often requires more code, the end result is a more convenient way to tweak SQL statements programmatically, as well as to achieve better overall performance. They also are extremely helpful when you want to trigger a stored procedure.

## Executing a Stored Procedure

Recall that a stored procedure is a named block of SQL code stored in the database. You can construct stored procedures so they return a set of rows or scalar data types or do anything else that makes sense (e.g., insert, update, or delete records); you can also have them take any number of optional parameters. The end result is a unit of work that behaves like a typical method, except that it is located on a data store rather than a binary business object. Currently, your AutoLot database defines a single stored procedure named GetPetName.

Now consider the following final method (for now) of the InventoryDal type, which invokes your stored procedure:

```csharp
public string LookUpPetName(int carId)
{
    OpenConnection();
    string carPetName;

    // Establish name of stored proc.
    using (SqlCommand command = new SqlCommand("GetPetName", _sqlConnection))
    {
        command.CommandType = CommandType.StoredProcedure;

        // Input param.
        SqlParameter param = new SqlParameter
        {
            ParameterName = "@carId",
            SqlDbType = SqlDbType.Int,
            Value = carId,
            Direction = ParameterDirection.Input
        };
        command.Parameters.Add(param);

        // Output param.
        param = new SqlParameter
        {
            ParameterName = "@petName",
            SqlDbType = SqlDbType.NVarChar,
            Size = 50,
            Direction = ParameterDirection.Output
        };
        command.Parameters.Add(param);

        // Execute the stored proc.
        command.ExecuteNonQuery();

        // Return output param.
        carPetName = (string)command.Parameters["@petName"].Value;
        CloseConnection();
    }
    return carPetName;
}
```

One important aspect of invoking a stored procedure is to keep in mind that a command object can represent a SQL statement (the default) or the name of a stored procedure. When you want to inform a command object that it will be invoking a stored procedure, you pass in the name of the procedure (as a constructor argument or by using the CommandText property) and must set the CommandType property to the value CommandType.StoredProcedure. (If you fail to do this, you will receive a runtime exception because the command object is expecting a SQL statement by default.)

Next, notice that the Direction property of the @petName parameter is set to ParameterDirection.Output. As before, you add each parameter object to the command object's parameters collection.

After the stored procedure completes with a call to ExecuteNonQuery(), you can obtain the value of the output parameter by investigating the command object's parameter collection and casting accordingly.

```csharp
// Return output param.
carPetName = (string)command.Parameters["@petName"].Value;
```

At this point, you have an extremely simple data access library that you can use to build a client to display and edit your data. You have not yet examined how to build graphical user interfaces, so next you will test your data library from a new console application.

## Creating a Console-Based Client Application

Add a new console application (named AutoLot.Client) to the AutoLot.Dal solution and add a reference to the AutoLot.Dal project. The dotnet CLI commands to accomplish this are as follows (assuming your solution is named Chapter21_AllProjects.sln):

```bash
dotnet new console -lang c# -n AutoLot.Client -o .\AutoLot.Client -f net5.0
dotnet sln .\Chapter21_AllProjects.sln add .\AutoLot.Client
dotnet add AutoLot.Client package Microsoft.Data.SqlClient
dotnet add AutoLot.Client reference AutoLot.Dal
```

If using Visual Studio, right-click your solution and select Add ► New Project. Set the new project as the startup project (by right-clicking the project in Solution Explorer and selecting Set as StartUp Project). This will run your new project when debugging in Visual Studio. If you are using Visual Studio Code, you need to navigate to the AutoLot.Test directory and run the project (when the time comes) by using dotnet run.

Clear out the generated code in Program.cs and add the following using statements to the top of Program.cs:

```csharp
using System;
using System.Linq;
using AutoLot.Dal;
using AutoLot.Dal.Models;
using AutoLot.Dal.DataOperations;
using System.Collections.Generic;
```
