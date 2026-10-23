# EXERCISE 5.5 *(continued)*

9. Create the files `cosmos.py`, `csharp.py`, and `guitar.py` using the code provided on GitHub (see step 4). In the `cosmos.py` file, change the endpoint and key to match your Azure Cosmos DB, which were called out in step 3. Then save the files using Ctrl+K+S (Save All). Execute the following command to create the database, container, partition key, and items:

```
```python
python .\\cosmos.py
```
```

Let’s go over what just happened in Exercise 5.5. Let’s also view some of the other Azure Cosmos DB concepts, such as the following:

+   - Azure Cosmos DB API models
+   - Databases, containers, items, and partition keys
+   - The cosmos.py code
+   - Azure Cosmos DB migration options
+   - Querying an Azure Cosmos DB
+   - Global replication concepts

# Azure Cosmos DB API Models

In step 2 of Exercise 5.5, you may have noticed a list of Azure Cosmos DB APIs in the drop-down list. Table 5.7 lists those APIs and data model.

**TABLE 5.7**  Azure Cosmos DB APIs

| API Type | Data Model | Container Item |
|---|---|---|
| Core (SQL) | Document database | Document |
| Azure Cosmos DB for MongoDB API | Document database | Document |
| Cassandra | Relations (Apache/CQL) | Row |
| Azure Table | Key/value pair | Item |
| Azure Cache for Redis | Key/value pair | Item |
| Gremlin | Graph database | Node or edge |