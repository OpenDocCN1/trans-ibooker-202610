# Quotas and limits

BigQuery comes with predefined quotas. These default quotas can be changed via the Navigation menu and IAM & Admin | Quotas. From this menu, we can review the current quotas and request an increase to these limits. We recommend that you are aware of the limits for each service, as this can have an impact on your scalability. For BigQuery, we should be aware of the following limits:

+   - There is a limit of 100 concurrent queries.
+   - There is a query execution time limit of 6 hours.
+   - 1,500 maximum number of table operations are allowed per day.
+   - There is a maximum of 100,000 streamed rows of data per second, per project. This is 500,000 in the US and EU multi-regions.
+   - A maximum of 300 concurrent API requests per user is allowed.

# Pricing

Storage costs for BigQuery are based on the amount of data we store. We can be charged for both active and long-term storage. Of course, long-term storage will be a lower monthly change. Additionally, we are charged for running queries. We are offered two pricing models for queries:

+   - **On-demand**: This charges based on the amount of data that's processed by each query.
+   - **Flat-rate**: This allows us to purchase dedicated resources for processing, so we are not charged for individual queries.

Pricing is also determined by the location of our BigQuery dataset. Now, let's look at Dataproc.

# Dataproc

Dataproc is GCP's big-data-managed service for running Hadoop and Spark clusters. Hadoop and Spark are open source frameworks that handle data processing for big data applications in a distributed manner. Essentially, they provide massive storage for data, while also providing enormous processing power to handle concurrent processing tasks.

If we refer to the *End-to-end big data solution* section of this chapter, Dataproc is also part of the processing stage. It can be compared to Dataflow; however, Dataproc requires us to provision servers, whereas Dataflow is serverless.