| Query |  |  |  |  |  |  |
|---|---|---|---|---|---|---|
| `WHERE marketplace = 'US' limit 1000;` |  |  |  |  |  |  |
| `select distinct product_category from "<table_name>"` | 1 min 16 sec | 1.38 sec | 32.22 GB | 0 |  | 0 |
| `SELECT product_id, product_title, count(*) as num_reviews, avg(star_rating) as avg_stars FROM <table_name> where product_category='Toys' GROUP BY 1, 2 ORDER BY 3 DESC limit 100;` | 1 min 17 sec | 2.27 sec | 32.22 GB | 215 MB | $0.15 | $0.00102 |

Table 5.2 – Table comparing execution time, data scan, and cost between TSV and Parquet files

If you review the table stats around execution time, data scan, and cost, you will get a fair idea of why you want to have data stored in an optimized columnar file format such as Parquet.

For example, when you have data stored in TSV without partition, in most of the queries, you need to scan either the entire dataset (for example, running count(*) or distinct) or a chunk of the dataset.

Similarly, when you have data stored in Parquet with partitions in cases such as count(*) and distinct, it doesn’t require you to scan the dataset as it maintains the stats about the data. If you have to run a filter statement such as a WHERE clause in TSV, it has to scan the entire dataset, and if data is partitioned, it has only gone through a specific partition, which eventually makes your query faster as it scans a lot less data.

# Selecting the right compression method for a data lake

Selecting the right compression method for a data lake in Amazon S3 is an important decision that can impact the performance and efficiency of the data lake. Compression can help reduce the storage space required for the data, as well as improve the speed of data transfer and query processing.

One of the most popular compression methods for a data lake in Amazon S3 is the Snappy compression algorithm. This algorithm is designed for high-speed data compression and decompression, and it can provide a good balance between compression ratio and performance. Snappy is supported by many popular big data frameworks, such as Apache Spark and Apache Flink, and it can be easily integrated into data pipelines and applications.