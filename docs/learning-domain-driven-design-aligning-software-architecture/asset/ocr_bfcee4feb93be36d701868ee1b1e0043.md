Operational models are built around the various entities from the system’s business domain, implementing their lifecycles and orchestrating their interactions with one another. These models, depicted in Figure 16-1, are serving operational systems and hence have to be optimized to support real-time business transactions.

![](bbox=[0.15, 0.23, 0.88, 0.54])

Figure 16-1. A relational database schema describing the relationships between entities in an operational model

Analytical models are designed to provide different insights into the operational systems. Instead of implementing real-time transactions, an analytical model aims to provide insights into the performance of business activities and, more importantly, how the business can optimize its operations to achieve greater value.

From a data structure perspective, OLAP models ignore the individual business entities and instead focus on business activities by modeling fact tables and dimension tables. We’ll take a closer look at each of these tables next.

# Fact Table