
Implement the GetSchemaTable() method next. This retrieves the SQL Server information regarding the target table.

```
public DataTable GetSchemaTable()
{
    using var schemaCommand = new SqlCommand($"SELECT * FROM {_schema}.{_tableName}",
        connection);
    using var reader = schemaCommand.ExecuteReader(CommandBehavior.SchemaOnly);
    return reader.GetSchemaTable();
}
```

Update the constructor to use the SchemaTable to construct the dictionary that contains the fields of the target table in database order.

```
public MyDataReader(List<T> records, SqlConnection connection, string schema, string tableName)
{
    ...
    DataTable schemaTable = GetSchemaTable();
    for (int x = 0; x < schemaTable?.Rows.Count; x++)
    {
        DataRow col = schemaTable.Rows[x];
        var columnName = col.Field<string>("ColumnName");
        _nameDictionary.Add(x, columnName);
    }
}
```

Now, the following methods can be implemented generically, using the reflected information:

```
public int FieldCount => _propertyInfos.Length;
public object GetValue(int i)
    => _propertyInfos
        .First(x => x.Name.Equals(_nameDictionary[i], StringComparison.OrdinalIgnoreCase))
        .GetValue(Records[_currentIndex]);
```

The remainder of the methods that must be present (but not implemented) are listed here for reference:

```
public string GetName(int i) => throw new NotImplementedException();
public int GetOrdinal(string name) => throw new NotImplementedException();
public string GetDataTypeName(int i) => throw new NotImplementedException();
public Type GetFieldType(int i) => throw new NotImplementedException();
public int GetValues(object[] values) => throw new NotImplementedException();
public bool GetBoolean(int i) => throw new NotImplementedException();
public byte GetByte(int i) => throw new NotImplementedException();
public long GetBytes(int i, long fieldOffset, byte[] buffer, int bufferoffset, int length) => throw new NotImplementedException();
public char GetChar(int i) => throw new NotImplementedException();
public long GetChars(int i, long fieldoffset, char[] buffer, int bufferoffset, int length) => throw new NotImplementedException();
public Guid GetGuid(int i) => throw new NotImplementedException();
public short GetInt16(int i) => throw new NotImplementedException();
public int GetInt32(int i) => throw new NotImplementedException();
public long GetInt64(int i) => throw new NotImplementedException();
public float GetFloat(int i) => throw new NotImplementedException();
public double GetDouble(int i) => throw new NotImplementedException();
public string GetString(int i) => throw new NotImplementedException();
public decimal GetDecimal(int i) => throw new NotImplementedException();
public DateTime GetDateTime(int i) => throw new NotImplementedException();
public IDataReader GetData(int i) => throw new NotImplementedException();
public bool IsDBNull(int i) => throw new NotImplementedException();
object IDataRecord.this[int i] => throw new NotImplementedException();
object IDataRecord.this[string name] => throw new NotImplementedException();
public void Close() => throw new NotImplementedException();
public DataTable GetSchemaTable() => throw new NotImplementedException();
public bool NextResult() => throw new NotImplementedException();
public int Depth { get; }
public bool IsClosed { get; }
public int RecordsAffected { get; }
```

### Executing the Bulk Copy

Add a new public static class named ProcessBulkImport.cs to the BulkImport folder. Add the following using statements to the top of the file:

```
using System;
using System.Collections.Generic;
using System.Data;
using System.Linq;
using Microsoft.Data.SqlClient;
```

Add the code to handle opening and closing connections (like the code in the InventoryDal class), as follows:

```
private const string ConnectionString =
    @"Data Source=.,5433;User Id=sa;Password=P@ssw0rd;Initial Catalog=AutoLot";
private static SqlConnection _sqlConnection = null;

private static void OpenConnection()
{
    _sqlConnection = new SqlConnection
    {
        ConnectionString = ConnectionString
    };
    _sqlConnection.Open();
}

private static void CloseConnection()
{
    if (_sqlConnection?.State != ConnectionState.Closed)
    {
        _sqlConnection?.Close();
    }
}
```

The SqlBulkCopy class requires the name (and schema, if different than dbo) to process the records. After creating a new SqlBulkCopy instance (passing in the connection object), set the DestinationTableName property. Then, create a new instance of the custom data reader holding the list to be bulk copied, and call WriteToServer(). The ExecuteBulkImport method is shown here:

```
public static void ExecuteBulkImport<T>(IEnumerable<T> records, string tableName)
{
    OpenConnection();
    using SqlConnection conn = _sqlConnection;
    SqlBulkCopy bc = new SqlBulkCopy(conn)
    {
        DestinationTableName = tableName
    };
    var dataReader = new MyDataReader<T>(records.ToList(), _sqlConnection,
        "dbo", tableName);
    try
    {
        bc.WriteToServer(dataReader);
    }
    catch (Exception ex)
    {
        //Should do something here
    }
    finally
    {
        CloseConnection();
    }
}
```

### Testing the Bulk Copy

Back in the AutoLot.Client project, add the following using statements to Program.cs:

```
using AutoLot.Dal.BulkImport;
using System.Collections.Generic;
```

Add a new method to Program.cs named DoBulkCopy(). Create a list of Car objects and pass that (and the name of the table) into the ExecuteBulkImport() method. The rest of the code displays the results of the bulk copy.

```
void DoBulkCopy()
{
    Console.WriteLine(" ............ Do Bulk Copy ............ ");
    var cars = new List<Car>
    {
        new Car() {Color = "Blue", MakeId = 1, PetName = "MyCar1"},
        new Car() {Color = "Red", MakeId = 2, PetName = "MyCar2"},
        new Car() {Color = "White", MakeId = 3, PetName = "MyCar3"},
        new Car() {Color = "Yellow", MakeId = 4, PetName = "MyCar4"}
    };
    ProcessBulkImport.ExecuteBulkImport(cars, "Inventory");
    InventoryDal dal = new InventoryDal();

    List<CarViewModel> list = dal.GetAllInventory();
    Console.WriteLine(" ********** All Cars **********");
    Console.WriteLine("CarId\tMake\tColor\tPet Name");
    foreach (var itm in list)
    {
        Console.WriteLine(
            $"{itm.Id}\t{itm.Make}\t{itm.Color}\t{itm.PetName}");
    }
    Console.WriteLine();
}
```

While adding four new cars does not show the merits of the work involved in using the SqlBulkCopy class, imagine trying to load thousands of records. I have done this with customers, and the load time has been mere seconds, where looping through each record took hours! As with everything in .NET Core, this is just another tool to keep in your toolbox to use when it makes the most sense.

### Summary

ADO.NET is the native data access technology of the .NET Core platform. In this chapter, you began by learning the role of data providers, which are essentially concrete implementations of several abstract base classes (in the System.Data.Common namespace) and interface types (in the System.Data namespace). You also saw that it is possible to build a provider-neutral code base using the ADO.NET data provider factory model.

You also learned that you use connection objects, transaction objects, command objects, and data reader objects to select, update, insert, and delete records. Also, recall that command objects support an internal parameter collection, which you can use to add some type safety to your SQL queries; these also prove quite helpful when triggering stored procedures.

Next, you learned how to safeguard your data manipulation code with transactions and wrapped up the chapter with a look at using the SqlBulkCopy class to load large amounts of data into SQL Server using ADO.NET.

## PART VII: Entity Framework Core

### CHAPTER 22: Introducing Entity Framework Core

The previous chapter examined the fundamentals of ADO.NET. ADO.NET has enabled .NET programmers to work with relational data (in a relatively straightforward manner) since the initial release of the .NET platform. Building on ADO.NET, Microsoft introduced a new component of the ADO.NET API called the Entity Framework (or simply, EF) in .NET 3.5 Service Pack 1.

The overarching goal of EF is to allow you to interact with data from relational databases using an object model that maps directly to the business objects (or domain objects) in your application. For example, rather than treating a batch of data as a collection of rows and columns, you can operate on a collection of strongly typed objects termed entities. These entities are held in specialized collection classes that are LINQ aware, enabling data access operations using C# code. The collection classes provide querying against the data store using the same LINQ grammar you learned about in Chapter 13.

Like the .NET Core framework, Entity Framework Core is a complete rewrite of Entity Framework 6. It is built on top of the .NET Core framework, enabling EF Core to run on multiple platforms. Rewriting EF Core has enabled the team to add new features and performance improvements to EF Core that couldn’t be reasonably implemented in EF 6.

Re-creating an entire framework from scratch requires a hard look at which features will be supported in the new framework and which features will be left behind. One of the features of EF 6 that is not in EF Core (and not likely to ever be added) is support for the Entity Designer. EF Core only supports the code first development paradigm. If you are currently using code first, you can safely ignore the previous sentence.

> Note EF Core can be used with existing databases as well as blank and/or new databases. Both mechanisms are called code first, which is probably not the best name. Entity classes and the derived DbContext can be scaffolded from an existing database, and databases can be created and updated from entity classes. You will learn both approaches in the EF Core chapters.

With each release, EF Core has added more features that existed in EF 6 as well as new features that never existed in EF 6. The 3.1 release significantly shortened the list of essential features that are missing from EF Core (as compared to EF 6), and 5.0 closed the gap even more. In fact, for most projects, EF Core has everything you need.

This chapter and the next will introduce you to data access using Entity Framework Core. You will learn about creating a domain model, mapping entity classes and properties to the database tables and columns, implementing change tracking, using the EF Core command-line interface (CLI) for scaffolding and migrations, and the role of the DbContext class. You will also learn about relating entities with navigation properties, transactions, and concurrency checking, just to name a few of the features explored.

By the time you complete these chapters, you will have the final version of the data access layer for our AutoLot database. Before we get into EF Core, let’s talk about object-relational mappers in general.
