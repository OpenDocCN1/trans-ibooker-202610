![](bbox=[0.09, 0.12, 0.91, 0.47])

Figure 21-1. ADO.NET data providers provide access to a given DBMS

A data provider will supply you with other types beyond the objects shown in Figure 21-1; however, these core objects define a common baseline across all data providers.

# ADO.NET Data Providers

As with all of .NET Core, data providers ship as NuGet packages. There are several supported by Microsoft as well as a multitude of third-party providers available. Table 21-2 documents some of the data providers supported by Microsoft.

Table 21-2. Some of the Microsoft-Supported Data Providers

| Data Provider | Namespace/NuGet Package Name |
|---|---|
| Microsoft SQL Server | Microsoft.Data.SqlClient |
| ODBC | System.Data.Odbc |
| OLE DB (Windows only) | System.Data.OleDb |

The Microsoft SQL Server data provider offers direct access to Microsoft SQL Server data stores—and only SQL Server data stores (including SQL Azure). The Microsoft.Data.SqlClient namespace contains the types used by the SQL Server provider.