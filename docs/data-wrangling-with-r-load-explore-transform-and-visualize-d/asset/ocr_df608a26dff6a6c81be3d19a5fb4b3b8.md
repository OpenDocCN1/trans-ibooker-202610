We have only made a small change to show the capability of Athena in restructuring data and also to perform ETL operations. You can play around with this more and create powerful data transformations as part of this exercise.

# Enriching data from multiple sources using Athena

In this section, we will explore how to enrich data using Athena SQL and also using an Athena federation setup for enriching data from other supported data sources.

# Enriching data using Athena SQL joins

In the previous section, we saw various ways through which we can explore data in Amazon Athena. Now, we will focus more on ways to enrich data with additional information through Athena queries.

In this phase, we can enrich raw data further by joining with other data sources. We will continue to use the same data source that we used in earlier sections. Let us assume a scenario where we want to identify the maximum recorded temperature of this century (after the year 2000) from a specific US state (Connecticut) for a specific year (2022). We will get the data for readings from the Parquet table (`noaa_data_parquet`) that was created using a CTAS statement in the previous section.

We can filter and get records from `Country code =US` based on our partition schema. But in order to get the records from the state of Connecticut, we need to join the `noaa_data_parquet` table with the stations data file ([http://noaa-ghcn-pds.s3.amazonaws.com/ghcn-d-stations.txt](http://noaa-ghcn-pds.s3.amazonaws.com/ghcn-d-stations.txt)), which has the state information for all stations within the US and Canada.

We will follow the steps here to enrich our dataset with state data:

+   1. Load the state data file using the regex format. We will follow the same approach that we used for loading the inventory table in an earlier section. We will first design a regex for loading the table using a regex validator and sample records, as illustrated here: