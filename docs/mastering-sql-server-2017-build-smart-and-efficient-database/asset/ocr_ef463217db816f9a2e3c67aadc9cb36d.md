There is a service available in Azure that can do about as much as SSIS can do with Azure data. In fact, if your data is only in Azure, this is the service you should use in conjunction with SSIS.

# Azure Data Factory and SSIS

Azure Data Factory (ADF) is a service that orchestrate data integration using different services available in Azure. Like SSIS, it can move data from one location to another. The main differences are the following:

+   - SSIS needs a windows machine to run. Even to copy data, the service runs on a windows server. ADF doesn’t needs anything to accomplish copy data tasks since the service runs in Azure
+   - SSIS has a rich toolset integrated into it to transform data; the dataflow task. ADF has nothing that come close to it built-in.
+   - ADF relies on compute services like HDInsight clusters that can run Hive and Pig scripts to transform data.
+   - SSIS can transform data on without leaving the package and, without necessarily staging it. It can transform and load data immediately to the destination. ADF calls a service that will transform the data but this service might not be able to load the data directly in a destination.

For example, we need to load data from Oracle, transform and load it into SQL Server. SSIS can do it in one single package. ADF would have to copy the data to an intermediately storage, call a service to transform the data and finally load it into the destination. SSIS 2016 service runs on a Windows OS based machine; ADF runs in Azure where there’s no OS to consider.

Generally, when most of your enterprise data is in Azure ecosystem and you barely use data on premises, it makes sense to use ADF. In that case, you are probable making the ETL using ADF and/or TSQL be it stored procedure or DML statements.

But, if your enterprise data is mostly on premises and some of it is in Azure, it makes more sense to use SSIS. Same statement if you're using on-premises Big Data cluster as you saw earlier in this chapter. SSIS now has a lot of connectors to leverage big data clusters on premises as well as in Azure.