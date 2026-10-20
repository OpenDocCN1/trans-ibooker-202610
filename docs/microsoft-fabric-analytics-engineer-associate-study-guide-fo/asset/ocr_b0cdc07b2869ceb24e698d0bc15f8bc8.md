```
ProductName VARCHAR,
Category VARCHAR,
Price DECIMAL(10, 2)
);
```

The Fabric warehouse supports a wide variety of schema modifications. You’re not limited to just adding new columns; you can also rename existing columns, change data types, and even remove columns when they are no longer needed.

# Enriching Data in the Fabric Lakehouse with Notebooks

The Fabric lakehouse stores data in *Delta tables*, which are files saved in a special format (Parquet) that also supports transformation. These tables live in OneLake and are well suited to handling large, analytical datasets. You enrich your data in a Fabric lakehouse by adding new columns or creating new tables, just like in a Fabric warehouse. However, some things work a bit differently.

The most common way to enrich data is by using a notebook with Spark SQL, but other methods exist, such as dataflows and Hive queries. We’ll focus on Spark SQL, but it is good to know that alternatives are available.

Let’s say you have a table called `customer_data` that was created by loading a CSV file. It includes columns such as `Timestamp` and `CustomerName`. Later, you decide to enrich this data by adding a new column to classify each customer based on their region.

In a Microsoft Fabric notebook, you can use Spark SQL to do this. If you don’t have a sample table you can work with, go ahead and create one by pasting this code into a Fabric lakehouse notebook: