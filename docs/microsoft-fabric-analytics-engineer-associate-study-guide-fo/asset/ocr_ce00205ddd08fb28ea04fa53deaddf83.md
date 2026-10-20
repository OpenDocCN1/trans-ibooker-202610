example, we may import updated sales data once a day while using a live connection for real-time inventory data.

A composite model allows a dataset to have two or more connections from different source groups. These can be as follows:

+   - One or more DirectQuery connections combined with Import mode
+   - Multiple DirectQuery connections from different data sources
+   - Any other combination of supported sources

# Creating a Composite Model

It only takes a few steps to create a composite model in Power BI. Let’s imagine that we have an Import mode semantic model in Power BI. We realize that we need one table from one of our Fabric eventhouses and that we need a live connection to the data. We can load this data into Power BI, and once we do that, we’ll see a message from Power BI asking us how we want the data to connect: via Import mode or via DirectQuery. In this case, because we want to have a live connection to the data, we choose DirectQuery. Power BI will then add the new table, and you’ll recognize it by the fact that its top bar will be bolded and in a different color (see the Tutorial table in Figure 5-36).