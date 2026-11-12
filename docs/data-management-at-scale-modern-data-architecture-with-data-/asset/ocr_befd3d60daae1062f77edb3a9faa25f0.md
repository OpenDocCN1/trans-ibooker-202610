which focuses on the coordination of data-related process steps. In the context of building data products or use cases, it converts the raw input data into meaningful information, thus delivering value to the business. The processes related to managing data pipelines are generally owned by the data engineers of the respective domain teams. Depending on the design, a data pipeline can be a long sequence with many steps. Figure 12-6 shows an example of what this might look like in practice.

![](bbox=[0.14, 0.2, 0.85, 0.61])

Figure 12-6. DataOps in practice: an example of how a data pipeline stitches several activities together

Here's a breakdown of the steps in this data pipeline from end to end:

+   1. Collect data. This step is usually automatically triggered whenever new data is ingested into the data landing area. For example, in Figure 12-6, a function invokes the data pipeline to run. This same function also submits a record to the central observability store.
+   
+   2. Validate the source location.
+   
+   3. Copy the source data from the intermediate landing area to the Bronze layer.
+   
+   4. Trigger a notebook to update schemas and external tables using the latest location where data is stored.