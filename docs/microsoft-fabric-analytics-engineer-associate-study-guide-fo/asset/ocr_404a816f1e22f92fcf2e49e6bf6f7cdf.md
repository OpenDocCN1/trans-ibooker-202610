# Choosing a Storage Mode

When we begin developing a semantic model in Power BI, we need to select the storage mode that best suits our needs. Making the right choice is important because each mode has its own advantages and limitations that directly affect performance, flexibility, and how up-to-date our data is. Power BI has four storage mode options: Import, DirectQuery, Direct Lake, and Dual. Let’s look at each storage mode and delve into its strengths and weaknesses.

> **NOTE**
> When you store your data in DirectQuery mode, you can choose to switch to Import mode later. This change is permanent, and you can’t switch it back to DirectQuery afterward. Therefore, you need to be sure about the change before making it.

# Import Mode

*Import mode* should always be our first choice if the semantic model allows it. In Import mode, we load the data directly into Power BI and store it there. This usually provides the best trade-off between performance and flexibility, as Power BI doesn’t need to reach out to the source every time we interact with the report. However, the data is only as current as the last scheduled or manual refresh, so it’s not real-time.

# Strengths

The strength of Import mode lies in the fact that it lets our report respond very quickly, even with large amounts of data. This is because Power BI already stores all the data in memory and doesn’t need to continuously send queries to the original data source.