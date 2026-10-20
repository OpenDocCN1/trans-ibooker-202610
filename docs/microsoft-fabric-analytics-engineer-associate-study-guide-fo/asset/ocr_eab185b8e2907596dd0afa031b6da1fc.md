When you select this connector, it will bring up a dialog box where you can enter your cluster name and the database name, and you’ll have the option to paste a KQL query and use that as a source for your analysis work (see Figure 2-33).

# Azure Data Explorer (Kusto)

![](bbox=[0.24, 0.26, 0.76, 0.71])

Figure 2-33. Azure Data Explorer (Kusto) connection dialog box

Next, scroll down and select the Direct Query data connectivity mode (see Figure 2-34).

You need to select the Direct Query mode because the KQL engine is much better at handling data queries from the database than Power BI is at handling the same amount of data. The KQL engine is built for very large datasets and