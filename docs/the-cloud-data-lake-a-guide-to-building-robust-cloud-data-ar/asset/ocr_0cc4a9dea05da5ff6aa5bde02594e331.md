the format offered a very high degree of compression, they were able to demonstrate savings in data storage costs that were well appreciated by the finance teams and executive leadership. Alice and her team are also evaluating the use of Delta Lake or Apache Iceberg as the first step to running the data lakehouse pattern they are going to explore.

# Data Organization and Partitioning

In the previous section, we saw how important the data formats are, as they are in effect how data is organized inside a file. In addition, when loading data into the data lake or writing data with big data processing tools, it’s important to understand how to organize the data storage itself. For example, think of organizing your closet, where you have one section for activewear, another for special occasions, and another for work clothes. Within those sections, you organize even further, such as sorting your summer wear and winter wear separately. Similarly, provisioning sections of your data lake to store data and how to organize the data storage within those sections is what we will focus on.

Why is this data organization important? Two factors contribute to this:

+   - Reading an object or reading a file involves two operations:
+   
+     *Metadata operations*
+   
+     Finding the file within the storage by listing the contents, making the access checks to ensure that the caller has access to the file/object, checking the integrity of the file/object, and so forth. While metadata operations are very important, these are more like overheads to get to the actual data.
+   
+     *Data operations*
+   
+     Actually working with the contents of the file/object—the read and write operations. These are the high-value operations that are relevant to the business aspect.
+   
+   - Data transfer from storage to compute involves data transfer over the network.

Let’s now put into perspective how this affects performance and scale.

Metadata operations are an overhead to get to the actual data operations. So in your big data processing, you need to ensure that you minimize the metadata operations compared to the data read/written. Consider the following scenarios that illustrate how larger file sizes make for efficient read operations, minimizing the metadata overhead:

*One hundred objects, each 1 MB in size*

To read 100 MB of data, you need to perform metadata operations on 100 objects.