offerings. This method depends greatly on the size of the database and whether it is realistic to transfer terabytes or more of data across a network connection. It might work, but it may not due to transient or network latency problems. Tools like Azure Storage Explorer or AzCopy are your best bet if you want to take that route.

If you do have a large database that you want migrated to an Azure database, consider a service called Import/Export. You can read about it here:

azure.microsoft.com/en-us/services/storage/import-export/

This service allows you to send actual physical, tape backups, or hard drives that contain your database to Microsoft. Microsoft then makes them available for importing into the DBMS. You also can use Azure Database Migration Services (DMS) to move data from an on-premise to a SQL managed instance. That method is more tolerant of databases that must remain available during the migration. Using the Import/Export option takes significantly longer and requires much more manual effort. The manual effort involves disk preparation and the application of transaction logs. Transaction logs capture data changes between the time you took the backup and when the database on Azure becomes the primary source of record. You would need to apply those changes to get the DB on Azure current.

Another option to move between two SQL Server databases is *SQL Server Integration Services (SSIS)*. The target in this scenario benefits the SQL virtual machine platform. See Table 5.6, which summarizes the contents of these methods in a more visual form. It is possible to use numerous approaches for migrations from on-premise to Azure; however, the following are the preferred approaches.

**TABLE 5.6**  Migration Options per Azure SQL Platform Type

|  | Azure SQL | SQL Managed Instance | SQL VM |
|---|---|---|---|
| BACPAC | ✓ | ✓ | ✓ |
| Import/Export | × | × | ✓ |
| ADMS | × | ✓ | × |
| SSIS | × | × | ✓ |

Complete the following Exercise 5.3 where you will import a database using a .BACPAC file and then encrypt it using the key stored in the Azure Key Vault you created earlier in Exercise 2.9.