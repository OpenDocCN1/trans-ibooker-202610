+   2. In the same Power BI Desktop file containing the SSAS counters, create an additional query to the parent folder of the gateway counter files and apply the same M query transformations to filter the files imported, adjust data types, rename columns, and add **Date** and **Time** columns to support relationships to the **Dates** and **Time** dimension tables
+   3. Build basic aggregation measures against the different gateway counter columns
+   4. Build additional DAX measures that apply or remove filter contexts from the **Dates** and **Time** tables, following the same expression patterns as the **SSAS Memory** DAX measures
+   5. Design a dedicated gateway report page for the gateway performance counters:

![](bbox=[0.2, 0.285, 0.87, 0.695])

Figure 10.16: Gateway performance report

In this example, the organization is using an SSAS 2014 Tabular Server as a primary data source for Power BI report and dashboard content. Therefore, measures based on the ADOMD gateway counters are used to expose the volume of this workload (bottom chart). The **# of all queries executed / sec** performance counter is used by the top chart, as well as the average and maximum card visuals above the line chart. Though less common, the organization also uses this gateway to support certain import refreshes of Power BI datasets (Mashup counters) and DirectQuery datasets (ADO.NET counters).