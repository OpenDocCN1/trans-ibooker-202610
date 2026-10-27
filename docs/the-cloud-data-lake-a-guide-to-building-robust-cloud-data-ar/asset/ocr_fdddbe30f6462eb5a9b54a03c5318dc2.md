location, and earning potential, you get all the details, not just the age and location alone.

# Consistency

Ensures that all the appropriate data validation rules are followed, and only data that is permitted is written. If the validation is not successful, the database is rolled back to its previous state before the transaction. For example, if you want to add a new customer record to the database, and you have the right name and age but an invalid location, the whole transaction fails.

# Isolation

Ensures that when concurrent transactions are processed, one does not affect the other. For example, if two users try to add the same customer to the database, the first one succeeds, and the second user gets an error since the customer is already present.

# Durability

Ensures that after a successful transaction, there is a guarantee that the data is available. For example, when you successfully add customers to a database, even when there is a power failure or hardware outage, you can be assured that the customer data is intact.

# Optimized for SQL

Most of the BI and data analyst tooling and ecosystem are optimized around SQL, and data warehouses offer a query engine that is optimized for SQL supporting these scenarios.

Data lakes offer the following advantages:

# Ability to store and process unstructured data

Most of the emerging scenarios around advanced analytics, data science, and machine learning rely on processing unstructured data. Data lakes make no assumptions about the structure or schema of the data. When reading data from a data lake, you define a schema at the time of the read.

# Low cost

Data lakes are highly optimized storage systems that offer a low cost of ownership to the user and let you store any amount of data you want without worrying about the rising expenses.

# Rich data management

Data lakes offer a slew of capabilities to help manage the data, as we saw in the earlier sections. These include tiered storage, data replication, and data sharing capabilities.