![](bbox=[0.12, 0.08, 0.86, 0.31])

Figure 16-11. Multiple versions of the same ETL script accommodating different versions of the operational model

Furthermore, since data lakes are schema-less—there is no schema imposed on the incoming data—and there is no control over the quality of the incoming data, the data lake’s data becomes chaotic at certain levels of scale. Data lakes make it easy to ingest data but much more challenging to make use of it. Or, as is often said, a data lake becomes a data swamp. The data scientist’s job becomes orders of magnitude more complex to make sense of the chaos and to extract useful analytical data.

# Challenges of Data Warehouse and Data Lake Architectures

Both data warehouse and data lake architectures are based on the assumption that the more data that is ingested for analytics, the more insight the organization will gain. Both approaches, however, tend to break under the weight of “big” data. The transformation of operational to analytical models converges to thousands of unmaintainable, ad hoc ETL scripts at scale.

From a modeling perspective, both architectures trespass the boundaries of the operational systems and create dependencies on their implementation details. The resultant coupling to the implementation models creates friction between the operational and