
# MATCHBY

This function was introduced to enhance the performance and precision of window functions like INDEX, OFFSET, WINDOW, RANK, and ROWNUMBER. It helps avoid circular dependency and duplicate row errors.

More importantly, MATCHBY allows you to specify a unique identifier (such as an ID or key) to determine the “current row” in your window function calculation, instead of using all columns in the table. This is particularly useful when working with large datasets. Rather than having Power BI scan all the columns, MATCHBY narrows them down to the specified unique identifier, significantly improving performance. For example, if you’re working with a large sales table, using MATCHBY on a unique SalesID column ensures that each row is treated individually, even when calculating window functions across other columns like Sales. This makes the calculation more efficient and accurate.

# Direct Lake

Previously, we examined how to analyze performance metrics and enhance DAX measures to improve the performance of Power BI reports. This section concentrates on a storage mode called Direct Lake, which is especially useful for enterprise-scale models. But what is it?

*Direct Lake* is a Power BI semantic model storage mode in Microsoft Fabric. It lets you work with large datasets stored in Delta tables, typically in lakehouses or warehouses, without importing the data into your model. Instead of refreshing or loading everything in advance,

Power BI reads the data directly from storage when needed.

To see how this fits in, let’s compare it to Import and DirectQuery:

- Import mode loads all data into memory and caches it locally. However, the downside is that refreshes can be slow and resource intensive, particularly with large datasets.
- DirectQuery bypasses loading data into memory and sends each query directly to the data source. While this minimizes memory load, it can result in slower performance and limited modeling features.
- Direct Lake strikes a balance between Import mode and DirectQuery. It uses the VertiPaq engine for rapid query performance (like Import mode) but only retrieves the data it needs when it needs it (like DirectQuery). This results in faster development, improved scalability, and reduced memory usage, making Direct Lake an ideal solution for environments with frequent data changes or massive datasets that are impractical to replicate.

# Implementing and Configuring Direct Lake

To use Direct Lake, you must work within a Microsoft Fabric workspace and connect to supported sources—specifically, Delta tables from Fabric lakehouses or warehouses. When you create a semantic model in Power BI within Fabric and link it to a lakehouse, Direct Lake is applied automatically if the connection and data meet the requirements.

The setup process is straightforward. You connect to your lakehouse or warehouse through Power BI, and Direct Lake will be used as long as the data is in Delta format. You don’t need to manually enable it unless you’re switching from another storage mode.

# DirectQuery as a Fallback for Direct Lake

You also need to understand that Direct Lake isn’t always available. Certain scenarios can cause Power BI to use DirectQuery mode as a fallback, even when the underlying data is still in Delta format. For instance, if a semantic model queries a view in the SQL analytics endpoint or a table that enforces row-level security (RLS), Direct Lake will not be applied and DirectQuery mode will be applied instead. Additionally, Power BI may use DirectQuery as a fallback if the Delta table exceeds capacity guardrails (e.g., if it has too many parquet files or row groups). This can occur even if the data is in Delta format and the semantic model is within the same workspace.

The use of DirectQuery as a fallback occurs automatically and without notice, so end users may not immediately recognize it. Indications that it has happened include increased latency and reduced data modeling options.

# Refreshing Direct Lake Models

A key feature distinguishing Direct Lake from Import mode is how refreshes operate. In Import mode, a refresh operation involves copying all relevant data into Power BI, which can be resource intensive and slow, especially with large datasets. Direct Lake manages refreshes differently. Instead of transferring data, it refreshes the metadata. This is often referred to as *framing*.

During a refresh, Power BI updates its reference to the latest version of the Delta tables in OneLake. The actual data isn’t moved or reloaded; only the structure and references are updated. This process typically completes in seconds and puts minimal load on the system, and that’s a significant advantage for organizations that deal with constantly updating data. It means new data becomes available to users much faster, without waiting for scheduled refresh cycles or triggering complex data movement operations.

**NOTE**
There are two different Direct Lake modes: Direct Lake on OneLake and Direct Lake on SQL. While both provide fast, query-over-files access, Direct Lake on OneLake allows tables from multiple Fabric items in a single model, whereas Direct Lake on SQL is limited to one Fabric item. You can create models that use Direct Lake on OneLake in Power BI Desktop, but you must create Direct Lake on SQL models in the Fabric interface first. This distinction can impact how you structure your data and develop your models.

# Incremental Refresh for Semantic Models

While improving DAX query performance is a key optimization strategy in Power BI, optimizing the semantic model itself is equally important. Performance might not be a concern for smaller datasets, but keeping the model fast and responsive becomes increasingly challenging as the dataset grows. For example, if you’re working with a large dataset that grows by millions of rows every night, reloading all the data during each refresh can slow down both the refresh process and the performance of your Power BI report.

One effective way to address this is by implementing *incremental refresh*, which involves only refreshing specific

data instead of reloading the entire dataset. This significantly reduces refresh times and improves resource utilization, which becomes crucial as your semantic model scales. By implementing incremental refresh, you can ensure that even large, enterprise-scale models remain efficient and performant.

Let’s look at how incremental refresh works and how to set it up in Power BI.

Incremental refresh works by partitioning data into two categories: data that needs frequent refreshing and data that doesn’t require frequent updates. In Power BI, this is done with Power Query, where the table is filtered based on date/time parameters, specifically the RangeStart and RangeEnd parameters. Only the rows within the defined refresh period are updated with each refresh. The data outside this period becomes part of the historical data and is no longer refreshed, and that ensures more efficient use of resources and faster refresh times, especially for large datasets.

# Requirements

Before you can set up incremental refresh in Power BI, you need to understand the prerequisites:

# Licenses

You need to use Fabric stock-keeping units (FSKUs) for incremental refresh in Fabric workspaces. You can also use Power BI Pro or Power BI embedded analytics, but incremental refresh for real-time data (using DirectQuery) is only supported in FSKUs and Power BI embedded models.

# Data source

The data source you want to load incrementally must support filtering data by date/time.

# Implementing Incremental Refresh in Power BI

To implement incremental refresh in Power BI, we must start by defining two parameters: RangeStart and RangeEnd. We initially use these parameters, which are specified in the Manage Parameters dialog box in Power Query Editor, to filter the data loaded into the Power BI Desktop model table to include only those rows with a date/time that’s within the time period we are interested in. RangeStart represents the earliest date/time, and RangeEnd represents the most recent date/time. After we publish the model to the service, the service automatically overrides RangeStart and RangeEnd to query data defined by the refresh period specified in the incremental refresh policy settings.

For example, say your Sales table gets new rows every night and you want to limit the number of rows loaded into the model to the last seven days. Here’s what you do:

- 1. Open Power Query (Transform Data)
- 2. Go to Home
- 3. Click on Manage Parameters
- 4. Click on New Parameter

Figure 6-25 shows what the Manage Parameters pop-up looks like.
