You can query the table using the datasource.database.tablename format and verify the working of the connector, like so:

![](bbox=[0.12, 0.15, 0.88, 0.49])

Figure 7.34: Querying the country table in RDS MySQL through the Athena console and registered data source

Now, we will explore how to use an Athena query to combine the country data in MySQL with the noaa_paraquet table, which is available in Amazon S3.

# Enriching data with data federation

In order to demonstrate the benefits of Athena federation, let us consider a situation where your manager has asked you (as the data analyst in the company) to find the lowest temperature of this century from Antarctica and 10 other countries as soon as possible. You as a data analyst in the company know that you have to query the table noaa_data_parquet table, but you are not sure about the country code value for these countries. You also know there is a table in MySQL that has this information and don’t want to wait for the data engineering team to move this table to Amazon S3 and create a data catalog on top of it to query in Amazon Athena.

Let us create a federated query that joins data from Amazon S3 and RDS MySQL and provides us with the output that was required for our use case. We will execute the following query to combine required datasets without data migration across data sources:

```
select min(data_value) as min_temp from AwsDataCatalog.wranglerathena.noaa_data_parquet t1
inner join wranglermysql.noaa.noaa_country t2 on t1.countrycode=t2.countrycode where
```