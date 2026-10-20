![](bbox=[0.14, 0.09, 0.86, 0.62])

Figure 3-46. The Product and Product detail tables have a relationship to each other

You can denormalize directly in Power BI, specifically in Power Query. By following the steps in this section, you can merge the Product detail table with the Product table to create a single table that lets you maintain a star schema when connecting the Product table to the fact table while keeping all the necessary data available.

To accomplish this, start by loading all the tables into Power BI and going to the Power Query Editor by clicking Transform data. Then, select the Product table and click on