# Azure SQL Database

A decade or so ago, there really were only two mainstream data stores you could choose from. One was Microsoft Access and the other Oracle. Microsoft Access wasn’t scalable and was therefore useful only for small kinds of applications. If you needed a large data store, your only option was Oracle. There were others, but for the sake of argument only those two are mentioned here as both are still relevant today. The first release of SQL Server was in 1989. It took some years to get to a point where it was an acceptable alternative to Oracle. These days it can be said they are about on par with each other. SQL Server is a world-class DBMS system capable of running any kind of workload in today’s IT world. The largest decision factor between the two is based on the ecosystem in which you want to run the workload. If you are determined (or required) to run a large percentage of your IT solutions using Microsoft technologies, then a valid choice for a relational database is SQL Server. An alternative ecosystem to Microsoft is perhaps open source with Java as the development language, Oracle as the database, and either Linux or Windows as the underlying operating system. All of those options are available on Azure; however, the remainder of this section is focused on SQL Server and the following topics:

+   - Selecting an Azure SQL database
+   - Pricing models and limits
+   - Configuring and securing
+   - Migrating data
+   - Monitoring
+   - Partitioning data
+   - SQL managed instance and SQL VM

Before you get into the details, you might consider reading the portion about Azure SQL from Chapter 1. There is good, high-level information there. Perhaps read it just to get into the context a bit. You should recognize that this is an area where a detailed level of understanding is necessary for the Azure Solutions Architect Expert exam. The exam focuses on scenarios in which you need to identify which Azure SQL options to select: the most cost-effective solution, the best method for migrating data, redundancy, and monitoring strategies. Migration, monitoring, and recovery are covered in more detail in Chapter 8 and Chapter 9; however, that information is more general in nature. This chapter and section focuses on Azure SQL databases.

# Selecting an Azure SQL Database

When it comes to selecting the right Azure SQL database to run on, you would start with choosing the platform that consists of Azure SQL, SQL managed instances, and SQL virtual machines; see Figure 5.16. Each has a specific use case based on cost, migration options, maintenance, and service level agreements (SLAs). Once you decide the platform, the next step is to determine the tier, for example general purpose, business critical, hyperscale, basic, standard, or premium. Each tier, like the platform, comes with a set of features with an