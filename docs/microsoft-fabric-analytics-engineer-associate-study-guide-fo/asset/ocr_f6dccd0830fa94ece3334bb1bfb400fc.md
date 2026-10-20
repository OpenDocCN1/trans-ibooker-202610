With the Python code, you can create a table for the Customer dimension with a primary key that will be linked to the fact table. The Customer dimension table will include the customer’s name, category, and buying group (see Figure 3-40).

![](bbox=[0.14, 0.22, 0.86, 0.45])

Figure 3-40. Customer dimension table in a Fabric lakehouse

# Implementing with Dataflow Gen2

You can also perform the same implementation with Dataflow Gen2. This can be very convenient for developers (especially those with experience in transforming data in Power BI) who are looking for a low-code/no-code approach.

To transform the data using Dataflow Gen2, go to your Fabric lakehouse, click on Get Data, and then select New Dataflow Gen2 (see Figure 3-41).