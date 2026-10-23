# EXERCISE 8.2 (continued)

5. Click the + New Migration Project link at the top of the Overview blade and take a look at the listed database products in the Source Server Type drop-down list. Also, take a look at the listed database products in the Target Server Type drop-down list. Change the source server type to match the source version of your database. Select the target server types and notice that the options change. See Table 8.3 for those mappings. Select Choose Type Of Activity to view the list contents. The New Migration Project blade now resembles the screen shown in Figure 8.10.

# FIGURE 8.10 DMS New Migration Project blade

![](bbox=[0.243, 0.327, 0.743, 0.571])

You may have noticed during the creation of DMS that there are two DMS modes, Azure and Hybrid (Preview). When you select Azure from the drop-down list it means that the worker on which the target database will exist on resides on the Azure platform. In contrast, you can create a DMS for usage with hybrid models where the target database is hosted on-premise. When a feature is tagged with “(Preview),” it means that feature is in a beta or testing phase. You would not want to use any product or feature in this mode for live production scenarios. However, the purpose of making those features available for consumption is for testing and discovering new features.

The Azure Database Migration Service (DMS aka ADMS) is useful for moving a large number of DBMS source versions to a supported target on the Azure platform. Table 8.3 lists the supported sources and their recommended target instance on Azure.

As you navigate through the Source Server Type options in the drop-down, you may also see tips for performing the selected migration between the source and target. For example, in Figure 8.10, you see the source is SQL Server and the target is Azure SQL Database. Some tips and links are shown under the bolded text “To successfully use Database Migration Service (DMS) to migrate data, you need to.” Those are very helpful to read and learn from.