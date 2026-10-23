
- DMS is intended for large datasets and many sources to many target migration scenarios.

Before you get too deep into DMS, complete Exercise 8.2 and create an Azure Data Migration Service.

## EXERCISE 8.2

### Creating an Azure Data Migration Service

- 1. Log in to the Azure Portal at `portal.azure.com`.
- 2. Select the menu button on the top-left corner of the web page and click the + Create A Resource link. Now, select Databases, and then select Azure Database Migration Service. (It is/was at the bottom of the list.)
- 3. Select the subscription and resource group into which you want to place DMS and select the location. Consider the location you choose as the location where you would migrate your database(s) to, as well. Enter a migration service name and click the Next : Networking button.
- 4. Select an existing virtual network/subnet or create a new one. The virtual network must exist in the same region as DMS. Click the Review + Create button, click the Create button, and once created navigate to the DMS Overview blade that would resemble Figure 8.9.

**FIGURE 8.9** The Azure Database Migration Service Overview blade as shown in the Azure Portal

![](img/95043b3df926bcdadadb082b49262037_602_0.png)

## EXERCISE 8.2 (continued)

5. Click the + New Migration Project link at the top of the Overview blade and take a look at the listed database products in the Source Server Type drop-down list. Also, take a look at the listed database products in the Target Server Type drop-down list. Change the source server type to match the source version of your database. Select the target server types and notice that the options change. See Table 8.3 for those mappings. Select Choose Type Of Activity to view the list contents. The New Migration Project blade now resembles the screen shown in Figure 8.10.

### FIGURE 8.10 DMS New Migration Project blade

![](img/95043b3df926bcdadadb082b49262037_603_0.png)

You may have noticed during the creation of DMS that there are two DMS modes, Azure and Hybrid (Preview). When you select Azure from the drop-down list it means that the worker on which the target database will exist on resides on the Azure platform. In contrast, you can create a DMS for usage with hybrid models where the target database is hosted on-premise. When a feature is tagged with “(Preview),” it means that feature is in a beta or testing phase. You would not want to use any product or feature in this mode for live production scenarios. However, the purpose of making those features available for consumption is for testing and discovering new features.

The Azure Database Migration Service (DMS aka ADMS) is useful for moving a large number of DBMS source versions to a supported target on the Azure platform. Table 8.3 lists the supported sources and their recommended target instance on Azure.

As you navigate through the Source Server Type options in the drop-down, you may also see tips for performing the selected migration between the source and target. For example, in Figure 8.10, you see the source is SQL Server and the target is Azure SQL Database. Some tips and links are shown under the bolded text “To successfully use Database Migration Service (DMS) to migrate data, you need to.” Those are very helpful to read and learn from.

## TABLE 8.3 Source/Target Database for DMS

| Source | Target |
|---|---|
| SQL Server | Azure SQL Virtual Machine |
|  | Azure SQL Managed Instance |
|  | Azure SQL Database |
| MongoDB | Azure Cosmos DB (MongoDB API) |
| MySQL | Azure Database for MySQL |
|  | SQL Server 2008-2017 |
|  | Azure SQL Database |
| AWS RDS for MySQL | Azure Database for MySQL |
| PostgreSQL | Azure Database for PostgreSQL |
| AWS RDS for PostgreSQL | Azure Database for PostgreSQL |
| Oracle | Azure Database for PostgreSQL |
| AWS RDS for SQL Server | Azure SQL Database |

The contents of the Choose Type Of Activity drop-down list change based on the selected source/target mapping. You may recall the definition of a schema in Chapter 5, illustrated in Figure 5.5. A schema is the database structure that contains the tables and the relationships between them; no data is migrated if you select a schema activity. Other activities include the following:

- Schema-only migration
- Offline data migration
- Online data migration
- Create project only

Deciding whether you can perform the migration while the database is still allowing CRUD operations is one that depends on your requirements. If you are performing a migration while the database is online, then you need to plan for handling disruptions and updating any tables that temporarily store data during the different data migration phases. Also note that DMS has different pricing tiers, and online migration is available only with Premium tier. You will see the SKU is Standard in Figure 8.9. Notice also that there is an

associated amount of compute allocated to the SKU. Recognize that DMS requires compute power to perform the migration. If you sense that the migration is slow, it is likely caused from the amount of compute you chose during the provisioning of DMS.

## Cosmos DB Data Migration Tool

If you choose Azure Cosmos DB as the location where you intend on storing JSON documents, content from an Azure Storage table, CSV files, or many other similar types of unstructured data, then consider using this tool. You can find more information about this here:

```
docs.microsoft.com/en-us/azure/cosmos-db/import-data
```

After installing the tool, you identify the source, which could be a directory full of JSON documents, and you select the target and execute the migration. Unstructured data, although it can be large, is in many cases a simple cut-and-paste exercise in contrast to an RDBMS, where replications can fail due to primary/foreign key violations.

## Azure Databox

This product includes some interesting hardware components. For more details, take a look here:

```
azure.microsoft.com/en-us/services/databox
```

Azure Databox devices are useful for moving large datasets to Azure. Azure Databox works in the context of Edge computing, where you typically have IoT devices capturing and streaming data that need real-time or near real-time ingestion, analysis, and alerting capabilities. The product comes in offline and online models where the offline model imitates the Import/Export product introduced in Chapter 5 and discussed in numerous other locations. With this tool, Microsoft sends you a hardware device to securely store your data. Once you have securely sent your data and the device to a Microsoft data center, the data is uploaded onto the Azure platform. In that scenario a massive amount of data needs to be migrated, and sending it over a network of any kind would be too great a task.

The online data transfer option includes a virtual device called a Data Box Gateway. This device helps to capture and transmit large datasets across the network. It is useful for archiving hundreds of terabytes of data, real-time ingestion of streaming data from the edge, and perhaps bulk data transfers of a large database migration, if the migration needs to happen online.

## Database Experimentation Assistant

There is a testing concept known as A/B testing where you send some of your users to the new version of your application and some to the current version. The Database Experimentation Assistant (DEA) provides this kind of testing scenario between different versions of SQL Server. DEA will execute and capture a defined scenario on your source SQL Server

instance and then replay them on the target instances. When both are completed, the tool will analyze the differences and provide a report. The report will help identify any compatibility issues or errors that are happening between the two SQL Server versions.

## Database Access Migration Toolkit

This tool is one that will analyze the source code and target the specific areas that can be impacted by database changes. This tool is in (preview) and currently supports only SQL Server and Oracle DBMSs. Java, C#, JSON, and SQL files are the supported coding languages and file types. You would use this tool in collaboration with DMA.

## Migrating Azure App Services

There is much focus on the migration of workloads to Azure Virtual Machines and data migration tools and methods on the Azure Solutions Architect Expert exam. Make sure you spend a lot of time in that area. One area not covered so much was Azure App Service (PaaS). As you know, Azure VMs and SQL Server on VMs are IaaS offerings. Sure, Azure SQL and SQL Server on Managed Instances is a bit more PaaS, but be certain that Azure App Service is a PaaS and dedicated to running websites. Azure App Service was introduced in Chapter 1, and you created your first one in Chapter 3. Lots of information was provided in Chapter 4, as well. Consider taking another look at Figure 4.2 (a decision tree) to see under which scenarios you would choose to provision an Azure App Service instance. The biggest difference between running a website on an Azure virtual machine versus an Azure App Service lies in the fundamental differences between IaaS and PaaS. In IaaS you have more flexibility, but more responsibility, while in PaaS you have less flexibility and less responsibility. That is the reason you need to utilize an App Service Migration Assistant for ASP.NET applications. You may have some configurations or dependencies that are not supported or allowed on Azure App Services. Recognize now that you can run Azure App Services with Containers; this will get you past most sandboxed restrictions. It may not be an option, however, due to the requirement of reskilling your IT support, deployment, and development staff. Containers are just an option and not necessarily the recommendation. Note that if you choose to proceed with what you have, then use the App Service Migration Assistant tool located at aka.ms/AppMigrate. The online tool will resemble that shown in Figure 8.11.

The figure calls out a few interesting points. First, the analysis happens when you enter the current URL of the web application you want to migrate to an Azure App Service. The App Service Migration Assistant tool will check the website and return the framework (like ASP.NET), how/where the website with the current URL you entered is being hosted, and the web server (IIS, for example). If there are any unsupported aspects found, then the report would call them out. Once the assessment is complete, you can download a migration assistant to help progress the movement. Also worthy of calling out is the link to Azure Migrate toward the bottom of the page. The more often you see specific tools, the more weight their usage and implementation should be.
