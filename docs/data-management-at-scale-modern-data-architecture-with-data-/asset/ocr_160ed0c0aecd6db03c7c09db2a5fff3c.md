+   - Metadata capabilities for documenting, discovering, monitoring, securing, and governing data
+   - Data engineering capabilities for developing, deploying, and running the data product's code
+   - Data quality and master data management capabilities
+   - Data marketplace capabilities for delivering a unified data shopping experience

Figure 4-5 is a logical design of a data product architecture, demonstrating how data is captured, transformed, and served to other domains. We'll go into more detail on each individual component, but first I'll provide a high-level overview. At the top, there's a metadata layer that provides insights into the available data, its properties, and relationships to other data management areas. Within this layer there are metadata repositories for orchestration, observability, contract management, and so on. These are the linking pins between your data marketplace and data product architecture.

![](bbox=[0.142, 0.387, 0.861, 0.739])

# Figure 4-5. Example data product architecture

When you use shared infrastructure, different data product architectures are isolated and not directly shared between domains. Each data product architecture, and accountability, remains with the data provider. A data provider, and thus a domain, has its own data product architecture instance and its own pipeline and takes ownership of the quality and integrity of the data.