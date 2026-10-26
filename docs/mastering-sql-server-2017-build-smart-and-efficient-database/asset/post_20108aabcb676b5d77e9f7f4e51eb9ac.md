
4. Double-click on it to open the **Foreach Loop Editor**, and assign the properties in the **Collection** pane, as shown in the following screenshot:

![](img/07d1225e179d5df9f2b2d26b026386c6_577_0.png)

5. Now go to the **Variable Mappings** pane and add a string variable called `User::AzureAggregatedData`. Make sure the scope is at the package level.

6. Drag a **Data Flow Task** into the `felc_StgCloudSales` and rename it `dft_StgCloudSales`.

7. Go into the **Data Flow Task** and drag an **Azure Blob Source** from the SSIS toolbox. Rename it `azure_blob_src_ExportBlob`.

8. Click anywhere on the background of the data flow and go to the **Properties** pane. Select **Expressions**. Click on the ellipsis button (`...`) and select `[azure_blob_src_ExportBlob].[Blob Name]` from the list. Assign `@[User::AzureAggregatedData]` as the value.

10. Drag an **OLE DB Destination** to the **Data Flow Task** and rename it `ole_dst_Staging_StgCloudSales`. Attach it to `azure_blob_src_ExportBlob`.

11. Double-click on it and set the properties as follows:

    - **OLE DB connection manager:** `c_mgr_DW`
    - **Name of the table or view:** `[Staging].[StgCloudSales]`

12. Go into the **Mappings** pane and make sure that all columns are mapped. Click **OK** to close the editor. Your screen should look like the following screenshot:

![](img/07d1225e179d5df9f2b2d26b026386c6_579_0.png)

13. Go back to the control flow and right-click on `felc_StgCloudSales`. Select **Execute** from the contextual menu to execute the container.

14. Attach `felc_StgCloudSales` to both the `sql_truncate_Staging_StgCloudSales` and the `apt_AggregateData` tasks. Your final package should look like the following screenshot:

## Azure Data Factory and SSIS

There is a service available in Azure that can do about as much as SSIS can do with Azure data. In fact, if your data is only in Azure, this is the service you should use in conjunction with SSIS.

Azure Data Factory (ADF) is a service that orchestrates data integration using different services available in Azure. Like SSIS, it can move data from one location to another. The main differences are the following:

- SSIS needs a Windows machine to run. Even to copy data, the service runs on a Windows server. ADF doesn’t need anything to accomplish copy data tasks since the service runs in Azure.
- SSIS has a rich toolset integrated into it to transform data; the data flow task. ADF has nothing that comes close to it built-in.
- ADF relies on compute services like HDInsight clusters that can run Hive and Pig scripts to transform data.
- SSIS can transform data on the fly without leaving the package and, without necessarily staging it. It can transform and load data immediately to the destination. ADF calls a service that will transform the data but this service might not be able to load the data directly in a destination.

For example, we need to load data from Oracle, transform and load it into SQL Server. SSIS can do it in one single package. ADF would have to copy the data to an intermediate storage, call a service to transform the data and finally load it into the destination. SSIS 2016 service runs on a Windows OS-based machine; ADF runs in Azure where there’s no OS to consider.

Generally, when most of your enterprise data is in the Azure ecosystem and you barely use data on premises, it makes sense to use ADF. In that case, you are probably making the ETL using ADF and/or T-SQL, be it stored procedures or DML statements.

But, if your enterprise data is mostly on premises and some of it is in Azure, it makes more sense to use SSIS. Same statement if you're using an on-premises big data cluster as you saw earlier in this chapter. SSIS now has a lot of connectors to leverage big data clusters on premises as well as in Azure.

The most frequent uses of the custom component are custom transformations that either provide operations that are not provided by the built-in components, or they encapsulate a series of transformations within a single one. In the latter case, you might also implement the operations by using multiple built-in transformations, but you would prefer to reduce the complexity of the data flow by implementing them as a single component.

Script tasks and components are discussed in more detail in Chapter 11, Unleash the Power of SSIS Script Tasks and Components. In both cases, the custom code is embedded in the SSIS package definition; you can create and modify it during package development, and it is deployed together with the package.

When using custom tasks or components, the custom code is developed in a separate Visual Studio project. You should create a separate assembly for each custom task, or component; this allows you to develop, modify, and deploy them without having to redeploy any of the packages in which they are used, and independently of any other custom tasks or components.

To decide whether to use scripting, or whether to design a custom task or component, you can use the following rule: use scripting if the complete logic of the task or component can be encapsulated into a single script and if the same script does not need to be used in multiple packages. Otherwise, you are encouraged to consider using custom tasks or components, especially in situations where the same custom logic needs to be implemented in numerous packages.

## Designing a custom task

To design a custom control flow task, you create a .NET assembly based on the Class Library Visual Studio template; the task's definition must be placed in a class derived from the Task base class, of the Microsoft.SqlServer.Dts.Runtime namespace (located in the Microsoft.SqlServer.ManagedDTS.dll assembly). This class also needs to implement the DtsTaskAttribute that is used to identify the class as an SSIS task and provide the elementary properties used when implementing the custom task in SSIS control flows.

Optionally, you can provide a custom graphical user interface for the custom task, which will be used as the task editor when the task is configured during SSIS package development. If a custom editor is not provided, the custom task can be edited by using the built-in advanced editor.

The task base class provides two methods that you need to override, and in them provide your custom code to:

- Validate the configuration of the task. This method is called automatically by the SSIS control flow designer whenever the task settings are changed, and allows you to communicate with the SSIS package developers to help them understand the task’s configuration and help them configure it correctly before it can be used.
- You also need to provide the logic needed to execute the task. This method will be called at runtime, when the package execution reaches the task in the control flow sequence. This method also allows access to various SSIS package resources:
  - The SSIS variables can be read or written.
  - The connections accessible to the package can be used to connect to various data sources.
  - The custom task may send log entries to the event log.
  - You are encouraged to implement events in your custom task, so that its performance can be captured by SSDT at design time, and by the SSIS server after the packages implementing the task have been deployed.
  - In the custom task, you can also detect whether the operation is participating in a transaction, and use this information when connecting to a data source.

Both of the preceding methods return a DTSExecResult value specifying the result of the validation, or the execution; this communicates the outcome of each method to the SSIS package developer, as well as the SSIS execution engine. The following values are supported by the DTSExecResult enumeration:

- Success is used to specify that the validation, or the execution, completed successfully.
- Failure specifies that the validation, or the execution, has failed. Generally, in case of failure, additional events should be returned from the task to provide more information about the failure to the SSIS package developer, or to the administrator of the deployment environment.
- Completion can be used for executions when either Success or Failure are not relevant results, or not specific enough. For instance, if a task completes its work successfully it returns Success, if it fails, it returns Failure, but if no work was performed, even though the configuration was in order, the task might return Completion to communicate a specific result that is neither a success nor failure.
