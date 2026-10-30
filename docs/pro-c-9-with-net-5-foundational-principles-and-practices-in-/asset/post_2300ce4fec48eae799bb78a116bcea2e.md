
## Understanding ADO.NET Data Providers

ADO.NET does not provide a single set of objects that communicate with multiple database management systems (DBMSs). Rather, ADO.NET supports multiple data providers, each of which is optimized to interact with a specific DBMS. The first benefit of this approach is that you can program a specific data provider to access any unique features of a particular DBMS. The second benefit is that a specific data provider can connect directly to the underlying engine of the DBMS in question without an intermediate mapping layer standing between the tiers.

Simply put, a data provider is a set of types defined in a given namespace that understand how to communicate with a specific type of data source. Regardless of which data provider you use, each defines a set of class types that provide core functionality. Table 21-1 documents some of the core base classes and the key interfaces they implement.

**Table 21-1. The Core Objects of an ADO.NET Data Provider**

| Base Class | Relevant Interfaces | Meaning in Life |
|---|---|---|
| DbConnection | IDbConnection | Provides the ability to connect to and disconnect from the data store. Connection objects also provide access to a related transaction object. |
| DbCommand | IDbCommand | Represents a SQL query or a stored procedure. Command objects also provide access to the provider’s data reader object. |
| DbDataReader | IDataReader, IDataRecord | Provides forward-only, read-only access to data using a server-side cursor. |
| DbDataAdapter | IDataAdapter, IDbDataAdapter | Transfers DataSets between the caller and the data store. Data adapters contain a connection and a set of four internal command objects used to select, insert, update, and delete information from the data store. |
| DbParameter | IDataParameter, IDbDataParameter | Represents a named parameter within a parameterized query. |
| DbTransaction | IDbTransaction | Encapsulates a database transaction. |

Although the specific names of these core classes will differ among data providers (e.g., SqlConnection versus OdbcConnection), each class derives from the same base class (DbConnection, in the case of connection objects) that implements identical interfaces (e.g., IDbConnection). Given this, you would be correct to assume that after you learn how to work with one data provider, the remaining providers prove quite straightforward.

> **Note** When you refer to a connection object under ADO.NET, you’re actually referring to a specific DbConnection-derived type; there is no class literally named Connection. The same idea holds true for a command object, data adapter object, and so forth. As a naming convention, the objects in a specific data provider are prefixed with the name of the related DBMS (e.g., SqlConnection, SqlCommand, and SqlDataReader).

Figure 21-1 shows the big picture behind ADO.NET data providers. The client assembly can be any type of .NET Core application: console program, Windows Forms, Windows Presentation Foundation, ASP.NET Core, .NET Core code library, and so on.

![](img/587021f4d5ba039be70a11107130377b_803_0.png)

Figure 21-1. ADO.NET data providers provide access to a given DBMS

A data provider will supply you with other types beyond the objects shown in Figure 21-1; however, these core objects define a common baseline across all data providers.

### ADO.NET Data Providers

As with all of .NET Core, data providers ship as NuGet packages. There are several supported by Microsoft as well as a multitude of third-party providers available. Table 21-2 documents some of the data providers supported by Microsoft.

**Table 21-2. Some of the Microsoft-Supported Data Providers**

| Data Provider | Namespace/NuGet Package Name |
|---|---|
| Microsoft SQL Server | Microsoft.Data.SqlClient |
| ODBC | System.Data.Odbc |
| OLE DB (Windows only) | System.Data.OleDb |

The Microsoft SQL Server data provider offers direct access to Microsoft SQL Server data stores—and only SQL Server data stores (including SQL Azure). The Microsoft.Data.SqlClient namespace contains the types used by the SQL Server provider.

> **Note** While System.Data.SqlClient is still supported, all development effort for interaction with SQL Server (and SQL Azure) is focused on the new Microsoft.Data.SqlClient provider library.

The ODBC provider (System.Data.Odbc) provides access to ODBC connections. The ODBC types defined within the System.Data.Odbc namespace are typically useful only if you need to communicate with a given DBMS for which there is no custom .NET Core data provider. This is true because ODBC is a widespread model that provides access to several data stores.

The OLE DB data provider, which is composed of the types defined in the System.Data.OleDb namespace, allows you to access data located in any data store that supports the classic COM-based OLE DB protocol. Due to the dependence on COM, this provider will only work on the Windows operating system and should be considered deprecated in the cross-platform world of .NET Core.

### The Types of the System.Data Namespace

Of all the ADO.NET namespaces, System.Data is the lowest common denominator. This namespace contains types that are shared among all ADO.NET data providers, regardless of the underlying data store. In addition to a number of database-centric exceptions (e.g., NoNullAllowedException, RowNotInTableException, and MissingPrimaryKeyException), System.Data contains types that represent various database primitives (e.g., tables, rows, columns, and constraints), as well as the common interfaces implemented by data provider objects. Table 21-3 lists some of the core types you should be aware of.

**Table 21-3. Core Members of the System.Data Namespace**

| Type | Meaning in Life |
|---|---|
| Constraint | Represents a constraint for a given DataColumn object |
| DataColumn | Represents a single column within a DataTable object |
| DataRelation | Represents a parent-child relationship between two DataTable objects |
| DataRow | Represents a single row within a DataTable object |
| DataSet | Represents an in-memory cache of data consisting of any number of interrelated DataTable objects |
| DataTable | Represents a tabular block of in-memory data |
| DataTableReader | Allows you to treat a DataTable as a fire-hose cursor (forward-only, read-only data access) |
| DataView | Represents a customized view of a DataTable for sorting, filtering, searching, editing, and navigation |
| IDataAdapter | Defines the core behavior of a data adapter object |
| IDataParameter | Defines the core behavior of a parameter object |
| IDataReader | Defines the core behavior of a data reader object |
| IDbCommand | Defines the core behavior of a command object |
| IDbDataAdapter | Extends IDataAdapter to provide additional functionality of a data adapter object |
| IDbTransaction | Defines the core behavior of a transaction object |

Your next task is to examine the core interfaces of System.Data at a high level; this can help you understand the common functionality offered by any data provider. You will also learn specific details throughout this chapter; however, for now it is best to focus on the overall behavior of each interface type.

### The Role of the IDbConnection Interface

The IDbConnection type is implemented by a data provider's *connection object*. This interface defines a set of members used to configure a connection to a specific data store. It also allows you to obtain the data provider's transaction object. Here is the formal definition of IDbConnection:

```csharp
public interface IDbConnection : IDisposable
{
    string ConnectionString { get; set; }
    int ConnectionTimeout { get; }
    string Database { get; }
    ConnectionState State { get; }

    IDbTransaction BeginTransaction();
    IDbTransaction BeginTransaction(IsolationLevel il);
    void ChangeDatabase(string databaseName);
    void Close();
    IDbCommand CreateCommand();
    void Open();
    void Dispose();
}
```

### The Role of the IDbTransaction Interface

The overloaded BeginTransaction() method defined by IDbConnection provides access to the provider's *transaction object*. You can use the members defined by IDbTransaction to interact programmatically with a transactional session and the underlying data store.

```csharp
public interface IDbTransaction : IDisposable
{
    IDbConnection Connection { get; }
    IsolationLevel IsolationLevel { get; }

    void Commit();
    void Rollback();
    void Dispose();
}
```

### The Role of the IDbCommand Interface

Next up is the IDbCommand interface, which will be implemented by a data provider's *command object*. Like other data access object models, command objects allow programmatic manipulation of SQL statements, stored procedures, and parameterized queries. Command objects also provide access to the data provider's data reader type through the overloaded ExecuteReader() method.
