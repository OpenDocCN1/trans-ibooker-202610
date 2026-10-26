
3. Double-click on it to open the Hadoop Hive Task Editor, as shown in the following screenshot:

![](img/07d1225e179d5df9f2b2d26b026386c6_558_0.png)

## Transferring data between Hadoop and Azure

Now that we have some data created by Hadoop Hive on-premises, we're going to transfer this data to a cloud storage on Azure. Then, we'll do several transformations to it using Hadoop Pig Latin. Once done, we'll transfer the data to an on-premises table in the staging schema of our `AdventureWorksLT DW2016` database.

In this recipe, we're going to copy the data processed by the local Hortonworks cluster to an Azure Blob storage. Once the data is copied over, we can transform it using Azure compute resources, as we'll see in the following recipes.

## Getting ready

This recipe assumes that you have created a storage space in Azure as described in the previous recipe.

## How to do it...

- 1. Open the `ETL.Staging SSIS` project and add a new package to it. Rename it `StgAggregateSalesFromCloud.dtsx`.
- 2. Add a Hadoop connection manager called `cmgr_Hadoop_Sandbox` like we did in the previous recipe.
- 3. Add another connection manager, which will connect to the Azure storage like the `cmgr_AzureStorage_ssiscookbook` we did in a previous recipe in this chapter.
- 4. Add a **Foreach Loop** container to the control flow and rename it `felc_HDP_Export`.

- 5. Double-click on it to open the **Foreach Loop** editor. Set the **Collection** properties, as shown in the following screenshot:

![](img/07d1225e179d5df9f2b2d26b026386c6_562_0.png)

- 6. Click on the **Variable Mappings**, create a new variable at the package level, and name it `HDPFileNamePath`, as shown in the following screenshot:

![](img/07d1225e179d5df9f2b2d26b026386c6_563_0.png)

- 7. Click **OK** to close the **Foreach Loop** editor.
- 8. Go to the **Variables** pane and set the value of the `HDPFileNamePath` to `/SSISCookBook/Export/000000_0`.
- 9. From the SSIS toolbox, drag and drop a HDFS file source into the `felc_HDP_Export` Foreach Loop container. Rename it `hfs_src_SSISCookBook_Export`.
- 10. Click anywhere in the data flow task background and go to the **Properties** pane (or press F4 to display it). Scroll down to **Expressions** and click on the ellipsis (...) button.
- 11. From the drop-down list, click on the ellipsis button to the right of the `hfs_src_SSISCookBook_Export.FilePath` expression. Fill the **Expression Builder**, as shown in the following screenshot. Click on OK to close the **Expression Builder**:

![](img/07d1225e179d5df9f2b2d26b026386c6_564_0.png)

- 12. Close and reopen the package to force the expression to be considered.
- 13. Double-click on it to open the **HDF File Source** editor and assign the following:
  - **Hadoop Connection Manager:** Select the `cmgr_Hadoop_Sandbox` from the drop-down list.
  - **File Path:** It should be `/SSISCookBook/Export/000000_0`. This is the value of the variable expression we set earlier.
  - **File format:** Leave the default, `Text`.
  - **Column delimiter character:** Use the vertical bar (`|`).

14. Click on the **Columns** tab and set the columns at the left of the editor and rename them, as shown in the following screenshot. Once done, click on **OK** to close the **HDFS File Source Editor**:

![](img/07d1225e179d5df9f2b2d26b026386c6_565_0.png)

15. Add a **Derived Column** to the data flow task and link it to `hfs_src_SSISCookBook_Export.FilePath`. Rename it `der_LoadExecutionId`. Open the **Derived Column Editor** and add a column called `LoadExecutionId`. Set the value to the **Package::LoadExecutionId** package parameter.

19. The final data flow task should look like the following screenshot:

![](img/07d1225e179d5df9f2b2d26b026386c6_567_0.png)

20. Execute the data flow task. Once completed, go to the Azure portal (portal.azure.com), go into your HDInsight cluster | Storage accounts | Blob service, and click on Import Blob. You should have a screen similar to the following screenshot:

![](img/07d1225e179d5df9f2b2d26b026386c6_567_1.png)

3. Your connection manager should look like the one in the following screenshot:

![](img/07d1225e179d5df9f2b2d26b026386c6_569_0.png)

4. Drag and drop an Azure Create Cluster task from the SSIS toolbox on the control flow and attach it to the felc_HDP_Export Foreach Loop container. Rename the task acc_ssiscookbook for Azure Create Cluster as ssiscookbook.

5. Double-click on it to open the Azure HDInsight Create Cluster Editor. Fill the properties as shown in the following screenshot. For the SubscriptionId, use your Azure subscription ID. The location depends on where you created your storage account. To avoid extra fees, you should have your cluster created in the same region as the one you used for your storage account:

7. Now we'll test if the cluster creation works. Right-click on the task and select Execute Task. The cluster creation starts. This task might take several minutes to complete. In our case, it takes up to 15 minutes.

8. Once completed, open a bash terminal. We will use the one that comes with Windows 10. Go to the Azure portal and look for the HDInsight cluster that has been created. In the overview, there is an option to connect using SSH. Click on it and copy the SSH command. Paste it in the bash terminal. It should consist of a command similar to the following:

```
ssh User@yourcluster-ssh.azurehdinsight.net
```

It is also shown in the following screenshot:

![](img/07d1225e179d5df9f2b2d26b026386c6_571_0.png)

9. Next, we'll add a task to drop the cluster once we've finished with it. From the SSIS toolbox, drag and drop an Azure HDInsight Delete Cluster Task on the control flow. Rename it adc_ssiscookbook. Double-click on it and set the properties as shown in the following screenshot:

![](img/07d1225e179d5df9f2b2d26b026386c6_572_0.png)

10. Click **OK** to close the `adc_ssiscookbook` task. Right-click on it and select **Execute Task** from the contextual menu that appears. Once executed successfully, go to the Azure portal and verify that the cluster has been dropped.

## There's more...

That's it! We can now create and drop clusters on demand. The following recipe will show how we can use the cluster with Pig.

## Managing data with Pig Latin

Pig Latin is one of the programs available in big data clusters. The purpose of this program is to run scripts that can accept any type of data. "Pig can eat everything," as the mantra of the creators states.

This recipe is just meant to show you how to call a simple Pig script. No transformations are done. The purpose of the script is to show you how we can use an Azure Pig task with SSIS.

## Getting ready

This recipe assumes that you have created a HDInsight cluster successfully.

## How to do it...

- 1. In the `StgAggregatedSales.dtsx` SSIS package, drag and drop an **Azure Pig Task** onto the control flow. Rename it `apt_AggregateData`.
- 2. Double-click on it to open the **Azure HDInsight Pig Task Editor** and set the properties as shown in the following screenshot:

- 5. Right-click on the `apt_AggregateData` and select **Execute Task** from the contextual menu that appears to run the script.
- 6. Once done successfully, go to the Blob storage in the Azure portal to check that the file has been created.
- 7. If any error occurs, go to the log file located in the directory that you specified in `apt_AggregateData`.
- 8. Your package should now look like the following screenshot:

![](img/07d1225e179d5df9f2b2d26b026386c6_575_0.png)

### There's more...

You'll notice that the data hasn't been modified, as the purpose of the recipe was to show how to call a Pig script from SSIS.

### Importing Azure Blob storage data

So far, we've created and dropped a HDInsight cluster and called a Pig script using the Azure Pig task. This recipe will demonstrate how to import data from an Azure Blob storage to a table in the staging schema.

### Getting ready

This recipe assumes that you have completed the previous one.

### How to do it...

- 1. From the SSIS toolbox, drag and drop, and **Execute SQL Task** on the control flow, and rename it `sql_truncate_Staging_StgCloudSales`.
- 2. Double-click on it to open the **SQL Task Editor**. Set the properties as follows and click on OK:
  - **Connection:** `cmgr_DW`
  - **SQL Statement:** `TRUNCATE TABLE [Staging].[StgCloudSales];`
- 3. From the SSIS toolbox, drag a **Foreach Loop Container** and rename it `feIC_StgCloudSales`.
