Microsoft has included JSON as a native data type in SQL Server 2016 and provided a set of functions to accompany the data type.

![](bbox=[0.12, 0.19, 0.2, 0.27])

Further details of JSON can be found in Chapter 3, JSON Support in SQL Server.

# In-Memory OLTP

In-Memory OLTP (codename Hekaton) was introduced in SQL Server 2014. The promise of ultra-high performance data processing inside SQL Server was a major feature when SQL Server 2014 was released. As expected with version-1 features, there were a wide range of limitations in the initial release and this prevented many customers from being able to adopt the technology. With SQL Server 2017, a great number of these limitations have been either raised to a higher threshold or completely removed. In-Memory OLTP has received the required maturity and extension in feature set to make it viable for prime production deployment.

# SQL Server Tools

Accessing or managing data inside SQL Server and developing data solutions are two separate disciplines, each with their own specific focus on SQL Server. As such, Microsoft has created two different tools, each tailored towards the processes and facets of these disciplines.

SQL Server Management Studio (SSMS), as the name suggests, is the main management interface between DBAs/developers and SQL Server. The studio was originally released with SQL Server 2005 as a replacement and consolidation of the old Query Analyzer and Enterprise Manager tools. As with any non-revenue-generating software, SSMS only received minimal attention over the years, with limitations and missing tooling for many of the newer features in SQL Server. With SQL Server 2016, the focus of Microsoft has shifted and SSMS has been de-coupled from the release cycle of SQL Server itself. This decoupling allows both SSMS and SQL Server to be developed without having to wait for each other or for release windows. New releases of SSMS are created on top of more recent versions of Visual Studio and have seen almost monthly update releases since SQL Server 2016 was released into the market.