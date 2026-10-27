As it turns out, solving for these requirements also has a really nice benefit, which is that this data tends to be highly compressible, resulting in both faster performance and lower cost, which means you could have your cake and eat it too.

Specialized formats like Delta Lake, Apache Iceberg, and Apache Hudi are used in the data lake architectures. We will discuss them more in Chapter 6. They all derive from a fundamental data format, Apache Parquet, which is a columnar data storage format used by the Apache Hadoop ecosystem.

Let’s do a small detour to understand what columnar format means. Remember, we are talking about tabular data, where data is organized in rows and columns, as shown in Figure 2-12. When it comes to how this data is stored in the data lake, the intuitive thinking is you store one record—that is, one row—together. In columnar formats, the data is stored in a column-oriented fashion, where data with similar values for a column is stored together. It’s this bundling of similar data that enables columnar formats like Apache Parquet to be highly compressible. Figure 2-12 provides a representation of the same data stored in a row-oriented as well as a column-oriented structure.

![](bbox=[0.13, 0.46, 0.84, 0.71])

Figure 2-12. Row-oriented versus column-oriented data formats

We will address Apache Parquet in more detail in Chapter 4. Open data technologies use Apache Parquet as their underlying data format so that they can leverage the optimizations for Apache Parquet to optimize for queries.