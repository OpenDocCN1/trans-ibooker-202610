If you want to read or write to multiple items at the same time, you can use the BatchGetItem and BatchWriteItem operations. These are essentially wrappers around individual GetItem and PutItem/DeleteItem/UpdateItem APIs respectively. The advantage of using these batch versions is that all the requests are submitted in a single API call. This reduces the number of network round trips from your client to DynamoDB. Your performance also benefits because DynamoDB executes each individual read or write operations in parallel.

The more recently available alternative API, known as PartiQL, is an SQL-derived dialect. You submit SQL statements using the ExecuteStatement and BatchExecuteStatement APIs. DynamoDB translates your SQL statements into individual APIs calls as defined in the classic API.

You also have ACID transaction capabilities using the ExecuteTransaction API. This enables you to group multiple CRUD operations to multiple items both within and across tables, with guarantees that all will succeed, or none will. Under the hood, DynamoDB uses the two phase commit algorithm to coordinate transactions across distributed partitions.

Transactions have an impact on capacity provisioning.¹³ In provisioned mode, each transaction will incur two reads or writes to each data item accessed in the transaction. This means you have to plan your read and write capacity units accordingly. If sufficient provisioned capacity is not available for any of the tables accessed in the transaction, the transactions may fail.

# Distribution and Replication

As a managed service, DynamoDB simplifies data distribution and replication from the application’s perspective. You define a partition key for items, and DynamoDB hashes the key to store three copies of every item. To enhance availability, the nodes which host each partition are in different availability zones within a single AWS region. Availability zones are designed to fail independently of others within each AWS region.