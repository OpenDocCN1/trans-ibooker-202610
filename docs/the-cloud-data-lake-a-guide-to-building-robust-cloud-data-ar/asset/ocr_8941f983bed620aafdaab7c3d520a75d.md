*Ten objects, 10 MB in size*  
To read 100 MB of data, you need to perform metadata operations on 10 objects.

*One object, 100 MB in size*  
To read 100 MB of data, you need to perform metadata operations on one object.

Most read operations involve conditionally selective queries. You have multiple options to accomplish this, and each involves sending data between storage and compute over the wire. As you can see, improvements in selections of the data minimize the data transferred over the wire:

+   - Read 500 MB of data from storage into compute, and perform the filtering in the compute engine to select the relevant 100 MB: this is inefficient because you are sending more data over the wire, and you are spending additional compute to do the filtering.
+   - Organize the 500 MB efficiently, so you can find the 100 MB you need with that organization and send it over the wire: this is a more efficient approach.

*Data partitioning* is the process of effectively organizing data in your objects in a way that makes retrieval easy. Whether the organization is based on AWS or GCP buckets or Azure folders and containers, you are optimizing it for retrieval. In a tabular data structure, partitions are typically based on columns that are most the commonly queried.

# Optimal data organization strategy for Klodars Corporation

Alice and her team wanted to explore various ways to partition their data. They took a closer look at their sales data, which most of their customers queried on, and brainstormed various options to organize it, as shown in Figure 5-10:

+   - *Option 1:* Organize data by regions followed by salesperson; this option is optimized for queries where regional sales patterns are most commonly used, followed by individual performance.
+   - *Option 2:* Organize data by time, followed by regions; this option is optimized for queries where trend over time is most commonly used, followed by regional pivots.
+   - *Option 3:* Organize data by regions, followed by time; this option is optimized for queries where regional sales patterns are most commonly used, followed by trends over time.