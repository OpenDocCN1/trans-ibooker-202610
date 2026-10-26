
Always Encrypted was designed to bridge this security gap by removing the ability of an instance owner to gain access to the encryption components. The entirety of the encryption process was moved outside of SQL Server and resides on the client side. While a similar effect was possible using homebrew solutions, Always Encrypted provides a fully integrated encryption suite into both the .Net Framework and SQL Server. Whenever data is defined as requiring encryption, the data is encrypted within the .NET framework and only sent to SQL Server after encryption has occurred. This means that a malicious user (or even system administrator) will only ever be able to access encrypted information should they attempt to query data stored via Always Encrypted.

Microsoft has made some positive progress in this area of the product. While no system is completely safe and no single feature can provide an all-encompassing solution, all three features provide a further option in building up, or improving upon, any system's current security level.

## Engine features

The Engine features section is traditionally the most important, or interesting, for most DBAs or system administrators when a new version of SQL Server is released. However, there are also numerous engine feature improvements that have tangential meanings for developers too. So, if you are a developer, don't skip this section—or you may miss some improvements that could save you some trouble later on!

## Query Store

The Query Store is possibly the biggest new engine feature to come with the release of SQL Server 2016. DBAs and developers should be more than familiar with the situation of a query behaving reliably for a long period, which suddenly changed into a slow-running, resource-killing monster. Some readers may identify the cause of the issue as the phenomenon of *parameter sniffing* or similarly through *stale statistics*. Either way, when troubleshooting to find out why one unchanging query suddenly becomes slow, knowing the query execution plan(s) that SQL Server has created and used can be very helpful. A major issue when investigating these types of problems is the transient nature of query plans and their execution statistics. This is where Query Store comes into play; SQL Server collects and permanently stores information on query compilation and execution on a per-database basis. This information is then persisted inside each database that is being **monitored** by the Query Store functionality, allowing a DBA or developer to investigate performance issues after the fact.

One big step in achieving this approach is the new Stretch Database feature with SQL Server 2016. Stretch Database allows a DBA to categorize the data inside a database, defining which data is *hot* and which is *cold*. This categorization allows Stretch Database to then move the *cold* data out of the on-premises database and into Azure Cloud Storage.

The segmentation of data remains transparent to any user/application that queries the data, which now resides in two different locations. The idea behind this technology is to reduce storage requirements for the on-premises system by offloading large amounts of archive data onto cheaper, slower storage in the cloud.

This reduction should then allow the smaller *hot* data to be placed on smaller capacity, higher performance storage. The *magic* of Stretch Database is the fact that this separation of data requires no changes at the application or database query level. This is a purely storage-level change, which means the potential ROI of segmenting a database is quite large.

![](img/07d1225e179d5df9f2b2d26b026386c6_28_0.png)

Further details of Stretch Database can be found in `Chapter 4`, *Stretch Database*.

## Database scoped configuration

Many DBAs who support multiple third-party applications running on SQL Server can experience the difficulty of setting up their SQL Server instances per the application requirements or best practices. Many third-party applications have prerequisites that dictate how the actual instance of SQL Server must be configured. A common occurrence is a requirement of configuring the *Max Degree of Parallelism* to force only one CPU to be used for query execution. As this is an instance-wide setting, this can affect all other databases/applications in a multi-tenant SQL Server instance (which is generally the case). With Database Scoped Configuration in SQL Server 2016, several previously instance-level settings have been moved to a database-level configuration option. This greatly improves multi-tenant SQL Server instances, as the decision of, for example, how many CPUs can be used for query execution can be made at the database-level, rather than for the entire instance. This will allow DBAs to host databases with differing CPU usage requirements on the same instance, rather than having to either impact the entire instance with a setting or be forced to run multiple instances of SQL Server and possibly incur higher licensing costs.

Initially, only non-clustered columnstore indexes were possible. With SQL Server 2014, clustered columnstore indexes were introduced, expanding the usability of the feature greatly. Finally, with SQL Server 2016, updateable columnstore indexes and support for In-Memory columnstore indexes have been introduced. The potential performance improvements through these improvements are huge.

![](img/07d1225e179d5df9f2b2d26b026386c6_30_0.png)

Further details of columnstore indexes can be found in Chapter 6, Columnstore Indexes.

## Containers and SQL Server on Linux

For the longest time, SQL Server has run solely on the Windows operating system. This was a major roadblock for adoption in traditionally Unix/Linux based companies that used alternative RDBM systems instead. Containers have been around in IT for over a decade and have made a major impression in the application development world. The ability to now host SQL Server in a container provides developers with the ability to adopt the development and deployment methodologies associated with containers into database development. A second major breakthrough (and surprise) around SQL Server 2017 was the announcement of SQL Server being ported to Linux. The IT world was shocked at this revelation and what it meant for the other RDBM systems on the market. There is practically no other system with the same feature-set and support network available at the same price point. As such, SQL Server on Linux will open a new market and allow for growth in previously unreachable areas of the IT world.

This concludes the section outlining the engine features. Through Microsoft's heavy move into cloud computing and their Azure offerings, they have had increased need to improve their internal systems for themselves. Microsoft has been famous for its dogfooding approach of using their own software to run their own business and Azure is arguably their largest foray into this area. The main improvements in the database engine have been fueled by the need to improve their own ability to continue offering Azure database solutions at a scale and provide features to allow databases of differing sizes and loads to be hosted together.

## Programming

Without programming, a SQL Server isn't very useful. The programming landscape of SQL Server has continued to improve to adopt newer technologies over the years. SQL Server 2017 is no exception in this area. There have been some long-awaited general improvements and also some rather revolutionary additions to the product that changes the way SQL Server can be used in future projects. This section will outline what programming improvements have been included in SQL Server 2017.

## Transact-SQL enhancements

The last major improvements in the T-SQL language allowed for better processing of running totals and other similar window functions. This was already a boon and allowed developers to replace arcane cursors with high-performance T-SQL. These improvements are never enough for the most performance conscious developers among us, and as such there were still voices requesting further incorporation of the ANSI SQL standards into the T-SQL implementation.

Notable additions to the T-SQL syntax include the ability to finally split comma-separated strings using a single function call, STRING_SPLIT(), instead of the previous hacky implementations using loops or the Common Language Runtime (CLR).

The sensible opposing syntax for splitting strings is a function to aggregate values together, STRING_AGG(), which returns a set of values in a comma-separated string. This replaces similarly hacky solutions using the XML data type of one of a multitude of looping solutions.

Each improvement in the T-SQL language further extends the toolbox that we, as developers, possess to be able to manipulate data inside SQL Server. The ANSI SQL standards provide a solid basis to work from and further additions of these standards are always welcome.

## JSON

It is quite common to meet developers outside of the Microsoft stack who look down on products from Redmond. Web developers, in particular, have been critical of the access to modern data exchange structures, or rather the lack of it. JSON has become the de facto data exchange method for the application development world. It is similar in structure to the previous cool-kid XML, but for reasons beyond the scope of this book, JSON has overtaken XML and is the expected payload for application and database communications.
