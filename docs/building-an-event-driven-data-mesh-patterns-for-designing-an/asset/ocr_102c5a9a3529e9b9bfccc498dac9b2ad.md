Event schemas are automatically inferred from the format of the query results. If the upstream database definition is changed, the schema of the events may also change if included as part of the query results.

![](bbox=[0.178, 0.162, 0.253, 0.233])

Ensure that the ownership and responsibilities of the connector platform, the connector execution, the connector code, and the resultant events’ owners are clear. You and your peers should be able to easily identify who to talk to when an issue arises.

There are several major pros for using query-based polling:

# Query flexibility

Databases are purpose-built to serve queries in an efficient manner, so querying the database is an easy and low-effort way to get started moving data from rest into an event-driven data product.

# Period flexibility

You can query as frequently as necessary to ensure your SLAs are met.

# Data model isolation

You gain isolation between the internal data model and the data product model published to the event stream. You can create views and materialized views to denormalize the data model, selecting only the data that needs to be emitted externally.

There are also several major cons for using query-based polling:

# Database resource usage

A database query may be quite extensive in terms of complexity and performance usage. You may find that you cannot get sufficient performance to meet your SLAs due to query complexity, volume of results, or regular operational load. While you may be able to mitigate this issue to some extent with read-only replicas, it does introduce additional costs and complexity, and not all database systems may have this option.

# Will miss hard deletes

Records that have been hard-deleted from a database will not be returned in a query (they’re deleted, after all). You may need to convert your database to using soft-deletes to track deletions. You will then have to convert the soft delete events to tombstones or else your consumers will need to account for soft deletes on their own. This complexity is overcome by using CDC.

# Will miss intermittent changes

Since we’re polling at a specific interval, multiple changes that occur between one poll and another will show up as only a single result in the query. If you’re trying