another well-known schema and show why the star schema is our schema of choice for use in Fabric.

# Star Schema versus Snowflake Schema

You may have also heard about the *snowflake schema*, which is a variation in which dimension tables are normalized into subdimensions. For example, a Product dimension table might be divided into Product and Product detail tables (see Figure 3-35).

![](bbox=[0.16, 0.33, 0.86, 0.81])

Figure 3-35. A snowflake schema

This approach reduces data redundancy, which can be helpful in certain transactional or storage-optimized