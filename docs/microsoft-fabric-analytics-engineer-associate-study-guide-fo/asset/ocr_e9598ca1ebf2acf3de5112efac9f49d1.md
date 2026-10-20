Even though we’ve shown you the way the SQL server database connection works, you’re not limited to using that specific connection. There are other data connectivity modes, and you need to understand them so you can have a full understanding of connections in Power BI. You must choose between two modes: Import and DirectQuery:

# Import

In this mode, Power BI reads all the data into memory and stores it in the underlying engine. This approach is slow at ingestion time, as the engine needs to read all the data, but it’s very fast at query time, as all the data is stored in memory in columnar format and is ready to be queried using DAX expressions. You’ll find out more about DAX expressions in Chapters 4 and 5.

# DirectQuery

In this mode, data is read at query time. When you filter a report or change anything that makes the engine need a new dataset, Power BI starts a query to get the data from the connected source. This option can be quite slow in some scenarios, like when you have a very large dataset in a SQL server database and the engine needs to filter that dataset based on a column from a different table. You’ll learn more about star schemas and relationships in Chapter 5.

Even though Direct Lake is not an option for this type of connection, it’s worth mentioning.

Direct Lake works by integrating the compute engine seamlessly with the structured data in OneLake. When a user interacts with a report or query, the connection first attempts to load necessary data chunks into the in-memory cache directly from the OneLake files. If the data is already