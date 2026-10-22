# Conclusion

In this chapter, you learned the different aspects of designing software systems, in particular, defining and managing analytical data. We discussed the predominant models for analytical data, including the star and snowflake schemas, and how the data is traditionally managed in data warehouses and data lakes.

The data mesh architecture aims to address the challenges of the traditional data management architectures. At its core, it applies the same principles as domain-driven design but to analytical data: decomposing the analytical model into manageable units and ensuring that the analytical data can be reliably accessed and used through its public interfaces. Ultimately, the CQRS and bounded context integration patterns can support implementing the data mesh architecture.

# Exercises

+   1. Which of the following statements is/are correct regarding the differences between transactional (OLTP) and analytical (OLAP) models?
+   
+   a. OLAP models should expose more flexible querying options than OLTP models.
+   
+   b. OLAP models are expected to undergo more updates than OLTP models, and thus have to be optimized for writes.
+   
+   c. OLTP data is optimized for real-time operations, whereas it’s acceptable to wait seconds or even minutes for an OLAP query’s response.
+   
+   d. A and C are correct.