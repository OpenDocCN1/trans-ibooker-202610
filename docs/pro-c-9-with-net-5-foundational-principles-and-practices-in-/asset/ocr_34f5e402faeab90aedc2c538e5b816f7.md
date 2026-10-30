Now that you have a proper appsettings.json, you can read in the provider and connectionString values using .NET Core configuration. Start by updating the using statements at the top of Program.cs to the following:

```
using System;
using System.Data.Common;
using System.Data.Odbc;
#if PC
    using System.Data.OleDb;
#endif
using System.IO;
using Microsoft.Data.SqlClient;
using Microsoft.Extensions.Configuration;
```

Clear all code from the Program.cs file, and add the following:

```
using System;
using System.Data.Common;
using System.Data.Odbc;
#if PC
    using System.Data.OleDb;
#endif
using System.IO;
using Microsoft.Data.SqlClient;
using Microsoft.Extensions.Configuration;
using DataProviderFactory;

Console.WriteLine("***** Fun with Data Provider Factories ****\n");
var (provider, connectionString) = GetProviderFromConfiguration();
DbProviderFactory factory = GetDbProviderFactory(provider);
// Now get the connection object.
using (DbConnection connection = factory.CreateConnection())
{
    if (connection == null)
    {
        Console.WriteLine($"Unable to create the connection object");
        return;
    }

    Console.WriteLine($"Your connection object is a: {connection.GetType().Name}");
    connection.ConnectionString = connectionString;
    connection.Open();

    // Make command object.
    DbCommand command = factory.CreateCommand();
    if (command == null)
    {
        Console.WriteLine($"Unable to create the command object");
        return;
    }
```