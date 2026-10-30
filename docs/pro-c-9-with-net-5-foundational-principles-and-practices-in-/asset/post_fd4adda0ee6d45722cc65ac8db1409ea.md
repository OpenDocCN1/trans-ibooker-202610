
```csharp
private void CloseConnection()
{
    if (_sqlConnection?.State != ConnectionState.Closed)
    {
        _sqlConnection?.Close();
    }
}
```

For the sake of brevity, most of the methods in the InventoryDal class will not use try/catch blocks to handle possible exceptions, nor will they throw custom exceptions to report various issues with the execution (e.g., a malformed connection string). If you were to build an industrial-strength data access library, you would absolutely want to use structured exception handling techniques (as covered in Chapter 7) to account for any runtime anomalies.

## Adding IDisposable

Add the IDisposable interface to the class definition, like this:

```csharp
public class InventoryDal : IDisposable
{
    ...
}
```

Next, implement the disposable pattern, calling Dispose on the SqlConnection object.

```csharp
bool _disposed = false;
protected virtual void Dispose(bool disposing)
{
    if (_disposed)
    {
        return;
    }
    if (disposing)
    {
        _sqlConnection.Dispose();
    }
    _disposed = true;
}
public void Dispose()
{
    Dispose(true);
    GC.SuppressFinalize(this);
}
```

## Adding the Selection Methods

You start by combining what you already know about Command objects, DataReaders, and generic collections to get the records from the Inventory table. As you saw earlier in this chapter, a data provider's data reader object allows for a selection of records using a read-only, forward-only mechanism using the Read() method. In this example, the CommandBehavior property on the DataReader is set to automatically close the connection when the reader is closed. The GetAllInventory() method returns a List<CarViewModel> to represent all the data in the Inventory table.

```csharp
public List<CarViewModel> GetAllInventory()
{
    OpenConnection();
    // This will hold the records.
    List<CarViewModel> inventory = new List<CarViewModel>();

    // Prep command object.
    string sql =
        @"SELECT i.Id, i.Color, i.PetName,m.Name as Make
            FROM Inventory i
            INNER JOIN Makes m on m.Id = i.MakeId";
    using SqlCommand command =
        new SqlCommand(sql, _sqlConnection)
    {
        CommandType = CommandType.Text
    };
    command.CommandType = CommandType.Text;
    SqlDataReader dataReader =
        command.ExecuteReader(CommandBehavior.CloseConnection);
    while (dataReader.Read())
    {
        inventory.Add(new CarViewModel
        {
            Id = (int)dataReader["Id"],
            Color = (string)dataReader["Color"],
            Make = (string)dataReader["Make"],
            PetName = (string)dataReader["PetName"]
        });
    }
    dataReader.Close();
    return inventory;
}
```

The next selection method gets a single CarViewModel based on the CarId.

```csharp
public CarViewModel GetCar(int id)
{
    OpenConnection();
    CarViewModel car = null;
    //This should use parameters for security reasons
    string sql =
        $@"SELECT i.Id, i.Color, i.PetName,m.Name as Make
            FROM Inventory i
            INNER JOIN Makes m on m.Id = i.MakeId
            WHERE i.Id = {id}";
    using SqlCommand command =
        new SqlCommand(sql, _sqlConnection)
    {
        CommandType = CommandType.Text
    };
    SqlDataReader dataReader =
        command.ExecuteReader(CommandBehavior.CloseConnection);
    while (dataReader.Read())
    {
        car = new CarViewModel
        {
            Id = (int)dataReader["Id"],
            Color = (string)dataReader["Color"],
            Make = (string)dataReader["Make"],
            PetName = (string)dataReader["PetName"]
        };
    }
    dataReader.Close();
    return car;
}
```

> **Note:** It's generally a bad practice to accept user input into raw SQL statements as is done here. Later in this chapter, this code will be updated to use parameters.

## Inserting a New Car

Inserting a new record into the Inventory table is as simple as formatting the SQL INSERT statement (based on user input), opening the connection, calling ExecuteNonQuery() using your command object, and closing the connection. You can see this in action by adding a public method to your InventoryDal type named InsertAuto() that takes three parameters that map to the nonidentity columns of the Inventory table (Color, Make, and PetName). You use these arguments to format a string type to insert the new record. Finally, use your SqlConnection object to execute the SQL statement.

```csharp
public void InsertAuto(string color, int makeId, string petName)
{
    OpenConnection();
    // Format and execute SQL statement.
    string sql = $"Insert Into Inventory (MakeId, Color, PetName) Values ('{makeId}',
    '{color}', '{petName}')";
    // Execute using our connection.
    using (SqlCommand command = new SqlCommand(sql, _sqlConnection))
    {
        command.CommandType = CommandType.Text;
        command.ExecuteNonQuery();
    }
    CloseConnection();
}
```

This previous method takes three values for Car and works as long as the calling code passes the values in the correct order. A better method uses Car to make a strongly typed method, ensuring all the properties are passed into the method in the correct order.

## Create the Strongly Typed InsertCar() Method

Add another InsertAuto() method that takes Car as a parameter to your InventoryDal class, as shown here:

```csharp
public void InsertAuto(Car car)
{
    OpenConnection();
    // Format and execute SQL statement.
    string sql = "Insert Into Inventory (MakeId, Color, PetName) Values " +
        $"('{car.MakeId}', '{car.Color}', '{car.PetName}')";

    // Execute using our connection.
    using (SqlCommand command = new SqlCommand(sql, _sqlConnection))
    {
        command.CommandType = CommandType.Text;
        command.ExecuteNonQuery();
    }
    CloseConnection();
}
```

## Adding the Deletion Logic

Deleting an existing record is as simple as inserting a new record. Unlike when you created the code for InsertAuto(), this time you will learn about an important try/catch scope that handles the possibility of attempting to delete a car that is currently on order for an individual in the Customers table. The default INSERT and UPDATE options for foreign keys default to preventing the deletion of related records in linked tables. When this happens, a SqlException is thrown. A real program would handle that error intelligently; however, in this sample, you are just throwing a new exception. Add the following method to the InventoryDal class type:

```csharp
public void DeleteCar(int id)
{
    OpenConnection();
    // Get ID of car to delete, then do so.
    string sql = $"Delete from Inventory where Id = '{id}'";
    using (SqlCommand command = new SqlCommand(sql, _sqlConnection))
    {
        try
        {
            command.CommandType = CommandType.Text;
            command.ExecuteNonQuery();
        }
        catch (SqlException ex)
        {
            Exception error = new Exception("Sorry! That car is on order!", ex);
            throw error;
        }
    }
    CloseConnection();
}
```

## Adding the Update Logic

When it comes to the act of updating an existing record in the Inventory table, the first thing you must decide is what you want to allow the caller to change, whether it is the car’s color, the pet name, the make, or all of these. One way to give the caller complete flexibility is to define a method that takes a string type to represent any sort of SQL statement, but that is risky at best.

Ideally, you want to have a set of methods that allow the caller to update a record in a variety of ways. However, for this simple data access library, you will define a single method that allows the caller to update the pet name of a given automobile, like so:

```csharp
public void UpdateCarPetName(int id, string newPetName)
{
    OpenConnection();
    // Get ID of car to modify the pet name.
    string sql = $"Update Inventory Set PetName = '{newPetName}' Where Id = '{id}'";
    using (SqlCommand command = new SqlCommand(sql, _sqlConnection))
    {
        command.ExecuteNonQuery();
    }
    CloseConnection();
}
```

## Working with Parameterized Command Objects

Currently, the insert, update, and delete logic for the InventoryDal type uses hard-coded string literals for each SQL query. With parameterized queries, SQL parameters are objects, rather than simple blobs of text. Treating SQL queries in a more object-oriented manner helps reduce the number of typos (given strongly typed properties); plus, parameterized queries typically execute much faster than a literal SQL string because they are parsed exactly once (rather than each time the SQL string is assigned to the CommandText property). Parameterized queries also help protect against SQL injection attacks (a well-known data access security issue).

To support parameterized queries, ADO.NET command objects maintain a collection of individual parameter objects. By default, this collection is empty, but you can insert any number of parameter objects that map to a placeholder parameter in the SQL query. When you want to associate a parameter within a SQL query to a member in the command object’s parameters collection, you can prefix the SQL text parameter with the @ symbol (at least when using Microsoft SQL Server; not all DBMSs support this notation).

## Specifying Parameters Using the DbParameter Type

Before you build a parameterized query, you need to familiarize yourself with the DbParameter type (which is the base class to a provider’s specific parameter object). This class maintains a number of properties that allow you to configure the name, size, and data type of the parameter, as well as other characteristics, including the parameter’s direction of travel. Table 21-6 describes some key properties of the DbParameter type.
