# Use cases for Stretch Database

With so many limitations, finding use cases for Stretch DB does not seem to be an easy task. You would need tables without constraints and rare data types that are not involved in relations with other tables and that don’t use some special SQL Server features. Where to find them? As potential candidates for stretching, you should consider historical or auditing and logging tables.

# Archiving of historical data

Historical or auditing data is commonly produced automatically by database systems and does not require constraints to guarantee data integrity. In addition to this, it is usually in large data sets. Therefore, historical and auditing data can be a candidate for using the Stretch DB feature. SQL Server 2016 introduced support for system-versioned temporal tables. They are implemented as a pair of tables: a current and a historical table. One of the requirements for historical tables is that they cannot have any constraints. Therefore, historical tables used in system-versioned temporal tables are ideal candidates for stretching. Temporal tables are covered in Chapter 5, Temporal Tables.

# Archiving of logging tables

Sometimes, developers decide to store application and service logging information in database tables. Such tables usually have no constraints, since writing to log must be as fast as possible. They are also possible candidates for using the Stretch DB feature.

# Testing Azure SQL database

Small or medium companies that are considering whether to use the cloud or to move data completely to it can use Stretch DB to start using Azure SQL database. They can learn about data management in Azure and collect experience and then decide whether they need to delegate more or their entire data to the cloud.