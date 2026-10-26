system performs the job automatically or it provides you with recommendations to achieve them using the Redshift Advisor in the Redshift console:

# Sort Keys

Ensure that sort keys exist to facilitate filter in the WHERE clause. Sort keys are important because Amazon Redshift stores the data on the disk in a sorted manner. The Redshift query optimizer uses sort orders to determine the optimal query plan for your data. For example, if you are frequently querying the most recent data, or data based on a time range, you can choose the time stamp column as your sort key. When you run a query with a time stamp as a filter, Redshift can skip entire blocks that would fall outside the range of your filter and thus provide you with optimal performance. You can use Redshift Advisor, which can recommend picking up appropriate sort keys.

# Compression

You can use compression in Redshift to reduce query I/O and thus improve overall performance. In Redshift, different columns can have different compression types, as Redshift has a columnar architecture. This allows you to identify the best possible compression type and most optimal performance. You can apply compression manually or use the COPY command, which will analyze the dataset and apply compression automatically. Using COPY command to apply automatic compression is highly recommended in Redshift.

# Distribution Style

You can ensure distribution keys are used to distribute the data across the cluster uniformly and to facilitate most common join types, thus reducing data movement during query execution. When you load data to a Redshift table, Redshift will distribute the table’s rows to the compute nodes and slices according to the distribution style you choose when you created the table. We’ll look into the distribution styles later in the chapter.

# Table Maintenance (Vacuum, Analyze)

Redshift does not delete the data when you delete a particular data item from a table; it only marks it as deleted. This means that as more data gets deleted, your cluster has space that is occupied by deleted rows, which is where a Vacuum operation can help you recoup the space. Furthermore, data stored on Redshift is stored in sorted order, which means that when new rows come in, they will be stored in an unsorted fashion unless they are arriving in the incremental order of sort key, which is generally not possible for data other than transactional data. A Vacuum sort is required to re-sort the data on disk, to provide better sorted-ness of data. Redshift now runs both VACUUM SORT and VACUUM DELETE automatically to improve your overall query performance.

In addition to VACUUM, ANALYZE helps you update table statistics, which helps the optimizer to provide an optimal cost assessment for different query plans. The table statistics increase sort key effectiveness, thus improving overall performance for the queries.

# Workload Management

Redshift uses ML algorithms to profile queries and then places them in an appropriate queue where they will be allocated appropriate resources. This is also done automatically, but the user has the option to manually override the AutoWLM settings or configure queues manually.