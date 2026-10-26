# Amazon DocumentDB

During re:Invent 2018, AWS released Amazon DocumentDB with the intent of meeting customer demands around managed document databases. I will try to introduce the benefits of a Document Database, before looking at the architecture and the interfaces for the database.

# Why a Document Database?

The data world today is pretty complex, and we have a variety of data types and file formats. Gone are the days when companies would retrofit their use cases into a single tool, as now you have the option to choose the tool that fits your specific use cases. The common categories of data and use cases are depicted in the following table, Table 3.5.

TABLE 3.5 Common data categories and use cases

| Data Category | Key Features | Use Case |
|---|---|---|
| Relational | Referential integrity
ACID transactions
schema-on-write | Databases, data warehouses
ERP
Finance
CRMs
Lift and shift from other systems |
| Key/value | High-throughput
Low latency (typically milliseconds) read and writes
Unlimited scale | Real-time bidding
Shopping cart
Social
Product catalog
Customer preferences
Recommendations
Adverts |
| Document | Raw JSON documents
Quick access
Native reads | Content management
Personalization
Mobile
Catalog
Retail and marketing |
| In-memory | Microsecond latency | Leaderboards
Real-time analytics
Caching |