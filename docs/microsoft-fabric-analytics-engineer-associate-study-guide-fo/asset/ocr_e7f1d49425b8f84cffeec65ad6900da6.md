# Direct Lake Mode

*Direct Lake mode* lets us query data directly from a Microsoft Fabric lakehouse or warehouse, without storing it in the semantic model. It’s optimized to quickly load data into memory from large Delta tables stored in Microsoft Fabric, and queries are processed by the VertiPaq engine, just as in Import mode. Also, unlike DirectQuery, Direct Lake doesn’t require a separate compute engine to serve queries. Instead, it loads only the necessary columns into memory on demand, making it efficient for large-scale models when properly optimized.

However, this mode requires a lakehouse or warehouse setup and may require tuning for optimal performance. In specific scenarios, such as when memory pressure prevents column loading or when we’re using unsupported features like SQL views, Direct Lake can fall back to DirectQuery mode, which may impact performance.

# Strengths

Direct Lake mode provides us with quick access to large amounts of data, thus eliminating the need to import it into Power BI. This is particularly helpful when we aim to avoid loading times or storage limits.

# Weaknesses

The weakness of Direct Lake mode is that it only works with Microsoft Fabric lakehouses or warehouses, which means we must set up and maintain such an environment. Also, if the Delta tables in our lakehouse are not well-optimized, we may still experience slow performance.