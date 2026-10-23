
## FIGURE 8.7 Exporting a virtualized server in Hyper-V

![](img/95043b3df926bcdadadb082b49262037_596_0.png)

Once exported, if required, you can convert the VHDX to VHD by using the following PowerShell command:

```
Convert-VHD -Path "C:\Temp\Metasploitable.vhdx" `
    -DestinationPath "C:\Temp\Metasploitable.vhd" -VHDType Fixed
```

## Windows Server/Linux Workloads

A simple way to find the most common or most recommended versions of operating systems that are available by Microsoft for Azure VMs is to look at the default list of images when creating one. Table 8.1 is the current list as of this writing that you would find in that drop-down list.

Note that just below the Image drop-down list there is a link to browse all public and private images. There exists a huge number of choices, so the list provided in Table 8.1 is a summary and by no means a complete list. The list will contain third-party images as well, so the list isn’t static, and therefore this book is not a place to attempt the creation of a complete list. Just remember two things here. First, Linux is open source, which means anyone can create and customize a version of it. Second, those customizations may or may not work when you deploy them to an Azure virtual machine. This is why you’d want to use one of the versions already on the platform and/or use a version of Linux that is endorsed aka blessed. You will find a list of them here:

docs.microsoft.com/en-us/azure/virtual-machines/linux/endorsed-distros.

the licenses you have for running your on-premise data center to Azure. Software license charges on Azure are typically built into the overall consumption charge of the compute platform. If you already have a license, then you could expect some kind of discount by using that license you already have. Makes sense, right? The Azure Hybrid Benefit program exists for just this reason. If you already have Windows Server and/or SQL Server licenses, then you could expect compute cost savings of between 40% and 55% when migrating the workloads to Azure. You may be able to increase that savings to 80% when you combine this benefit program with Azure Reserved VM Instances (RI), discussed next.

## Azure Reserved VM Instances

The default pricing model you typically find in the cloud is based on your consumption. When you need a lot of compute or other Azure resource, it is there for you. When you don’t need it anymore, the hardware and infrastructure is still there, but you are not billed for it if you do not use it. That’s good for you and one of the great benefits of the cloud concepts. From the cloud service provider perspective, having infrastructure and compute resources idle is not cost effective. A solution for this is to kind of share the cost of idle hardware. The means for this sharing is realized with RIs. The resources that currently support this model can be seen in the Reservations blade in the Azure Portal. Your Reservations blade should be similar to that shown in Figure 8.8.

## FIGURE 8.8 Azure Reserved Instances

![](img/95043b3df926bcdadadb082b49262037_598_0.png)

Instead of incurring pay-as-you-go charges based on unplanned consumption, you commit to a specific spend amount over a 7-day, 30-day, 60-day, or longer time frame. It would need to be less than if you were charged solely based on usage but a bit more than if you had less than expected usage during the same time frame. The overall cost average would be less for the customer but more cost effective for the cloud provider. The infrastructure isn’t simply sitting idle doing nothing and generating no revenue. Take a look at the costs incurred when using RIs, compare that to the cost of dynamic pay-as-you-go consumption charges, and, if you save, then by all means take advantage of it.

## Containers

If you want to lift and shift one quick way is using a container. In case you forgot about this model, take a look at the container as a service (CaaS) visualized in Figure 4.1. Also, review the decision tree illustrated in Figure 4.2 to again refresh yourself on the rationale for choosing containers as a deployment or migration option. If you get into a bind or have exhausted all effort to migrate your workloads to Azure because of currently unsupported customization in your application, don’t ignore or forget this option. Seriously consider contacting Microsoft Support; seriously, now is the time to work with Microsoft Support. They want your business and want you to be successful on Azure.

In summary, a container lets you isolate your application completely away from a VM and an operating system. Either or both of those layers may be the reason your deployments or migration plans are not progressing as expected. Removing them may be the quickest solution. Take a warning here, though. Prior to making a decision like this, recognize the significant architectural changes and the impact on skillset required to migrate and support such architectures.

## Database Migration

The migration of your data stores and data storage needs to follow the structural process used to migrate your compute. Refer to Figure 8.1 where the phases of assessment, migration, optimization, and security and management are called out. As with assessing and migrating, specific tools exist for the assessment and migration of your database. You can, by all means, use Azure Migrate to help discover and inventory the machines that you want to migrate.

A significant decision point that dictates your approach has much to do with the data source.

Is the data source Microsoft SQL Server, or is it another of the many other data sources, like Oracle, MySQL, Cosmos DB, and Mongo?

Do you plan on migrating those non-Microsoft SQL Server databases to one of the SQL Server offerings on Azure?

Are you moving what you have to Azure, or are you moving to Azure and changing DBMS?

If you are moving from an on-premise Microsoft SQL Server installation to a SQL Server product on Azure, then a good place to start is with the *Database Management Assistant (DMA)*.

## Database Management Assistant

DMA is a tool that will help you identify compatibility issues between the currently running version of SQL Server and the one you are targeting for the migration. Table 8.2 provides an overview of supported source database versions and supported target database versions.

**TABLE 8.2  SQL Server–Supported Sources and Targets**

| Location | Version |
|---|---|
| Source | SQL Server 2005 |
| Source | SQL Server 2008 |
| Source | SQL Server 2008 R2 |
| Source | SQL Server 2012 |
| Source | SQL Server 2014 |
| Source | SQL Server 2016 |
| Source | SQL Server 2017 on Windows |
| Source | SQL Server 2019 |
| Target | SQL Server 2012 |
| Target | SQL Server 2014 |
| Target | SQL Server 2016 |
| Target | SQL Server 2017 Windows / Linux |
| Target | SQL Server 2019 |
| Target | Azure SQL single database |
| Target | SQL Server manages instance |
| Target | SQL Server on an Azure virtual machine |

You might agree that if your source database is SQL Server 2005 and the target is SQL Server 2019, there may have been some features that were deprecated along the way. DMA specifically calls out deprecated features, changes in behaviors, and breaking changes. Most of those would require redesign or rework to have a successful migration. DMA provides some tips on how to work around them, as well, and specifically shows additional recommendations in regard to security, performance, and storage. Notice in Table 8.2 that the oldest targetable version of SQL Server on Azure is SQL Server 2012; therefore, if you are running an older version than 2012, using DMA prior to migration is highly recommended. If you wanted to migrate to one of the targeted SQL Server versions from a DBMS other than SQL Server, then you would want to use a tool called SQL Server Migration Assistant (SSMA).

## SQL Server Migration Assistant

If you decided to move from a non-Microsoft SQL Server product to Azure and at the same time migrate the DMBS to SQL Server, then SSMA is the tool for helping you achieve that. The supported source data stores as of this writing are provided in the following list:

- Microsoft Access
- SAP ASE
- Oracle
- MySQL
- DB2

The SSMA tool can automate a comparison of many steps in a migration from discovery and assessment of the existing implementation and the targeted location. Because of the vast variety and customization of source DBMSs, there may be some issues that are not resolvable by a simple patch or code modification. There may be some issues found that may require a fundamental rethink about the design and purpose of an application’s implementation. The point is, don’t underestimate a migration like this. Make sure you have the right skilled specialist working on this kind of migration. Now that you are covered from SQL Server to SQL Server and non-SQL Server to SQL Server, take a look at DMS aka ADMS.

## Azure Data Migration Service

The DMS tool was introduced in Chapter 1, with some additional discussion (and rightly so) in Chapter 5. Chapter 5 is where you learned about data and storage, and a chapter on that couldn’t exist without a little bit about DMS. Some nice details about this service are accessible at datamigration.microsoft.com. DMS is optimal for scenarios where there are a large number of database to be migrated. The differences between DMA, SSMA, and DMS are as follows:

- DMA is used for small datasets, compatibility reports, and SQL Server to Azure SQL Server migrations.
- SSMA accommodates small to medium datasets, compatibility reports, and non-SQL Server to Azure SQL migrations.
