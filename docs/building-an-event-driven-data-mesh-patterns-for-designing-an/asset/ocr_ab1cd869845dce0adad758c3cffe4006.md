# # Data Products Are Multimodal

There is no definitive “right way” to create a data product, nor is there a single expected form, format, or mode that it is expected to take. Data products are intrinsically multimodal, and precisely which modes you use for sharing data is open to negotiation. We'll return to this subject of mode selection in more detail in Chapter 4.

In the meantime, the same data product can be served up in a number of different formats and APIs. For example, a single data product may be:

+   - Produced to an event stream and updated as changes occur
+   - Composed into a set of Parquet files, updated daily to a cloud storage bucket
+   - Remain stored behind a REST API, provided to clients on demand

Figure 2-3 illustrates the composition of a multimodal data product. The data product code extracts the data from the operational database and composes it into a form of internal intermediate state. A secondary executable then converts the intermediate state into a format suitable for the output modes, such as events for an event stream or batch files for cloud storage.

![](bbox=[0.143, 0.441, 0.863, 0.666])

Figure 2-3. A multimodal data product, with data available through three different ports: a REST API, an event stream, and a cloud storage bucket

Batch-computed data products do have a place in a data mesh. After all, data warehousing and data lake architectures rely extensively on periodically computed batches of data to power their periodically executed jobs. The vast majority of analytical workloads have been built extensively on batch-computed data sources.