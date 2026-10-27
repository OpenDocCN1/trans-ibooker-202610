# How it works...

Incremental refresh uses the reserved parameter names **RangeStart** and **RangeEnd** in executing incremental refresh. While initially set during the configuration of incremental refresh, these parameters are thereafter updated by the Power BI service to determine which rows within the dataset to refresh incrementally.

When configuring the filter rows for incremental refresh, it is important that the two conditions for **RangeStart** and **RangeEnd** do not both include an equal to (=) clause as this may result in duplicate data. Only configure one of the conditions to include an equal to (=) clause in order to avoid such circumstances.

Upon first refreshing the data in the service, Power BI refreshes the entire dataset per the **Store rows where column is in the last** setting in the **Incremental refresh** dialog. In other words, during the first refresh of a dataset configured for incremental refresh, the refresh process functions like a full reload of all of the data. All rows within the store period are included in the dataset unless other filters apply. Subsequent refreshes only refresh the new data per the **Refresh rows where column is in the last** setting.

It is important to remember that once incremental refresh is configured, the Power BI file can no longer be exported from the service. Thus, it is imperative that the original Power BI desktop file be preserved in the event that changes need to be made.

# There's more...

There are two optional incremental refresh settings in the **Incremental refresh** dialog. These optional settings are the following:

+   - Detect data changes
+   - Only refresh complete days

The **Detect data changes** setting can be used to further optimize the data refreshed by incremental refresh. A column of type **Date/Time** must be used for this setting and this column should not be the same column used for the **RangeStart** and **RangeEnd** parameters. The Power BI service evaluates the maximum value of this column when refreshing and if the value has not changed since the previous refresh then no data is refreshed within the dataset.

The **Only refresh complete days** setting is used to prevent partial days from being included in data refreshes. For example, in the event that the refresh operation detects that a day is incomplete, such as a scheduled refresh at 4 AM, then that day's data is not included in the refresh operation. This can be useful for organizations that only want complete daily information in their datasets for reporting purposes.