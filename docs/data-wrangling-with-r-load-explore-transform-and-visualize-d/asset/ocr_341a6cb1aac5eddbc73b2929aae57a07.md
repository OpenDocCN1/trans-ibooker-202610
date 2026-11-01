| | zstd | LZO |
| | | snappy |

Table 5.1 – File formats and compression types

Now we have a fair understanding of columnar/splittable and row-based/unsplittable file formats, let’s consolidate this with an example, where we will be comparing how performance and cost will be affected based on your file type.

In this example, we will be using the *Amazon Customer Reviews* dataset, which you can find here:
https://registry.opendata.aws/amazon-reviews/

This dataset contains 130 million records of customer reviews since 1995, in both **tab-separated values (TSV)** and Parquet file formats.

```
(base) navn is@38f9d36a27bd ~ % aws s3 ls s3://amazon-reviews-pds/
PRE parquet/
PRE tsv/
```

Figure 5.2 – Data in Amazon Customer Reviews open dataset

**TSV** is similar to **CSV**. It is a simple file format for storing data in a tabular format—for example, a database table or spreadsheet data—with each row of the table represented as a separate line in the file and each cell in the table separated by a tab character.

**Parquet** is a column-oriented file format for storing large amounts of data in a file. It is used for storing data in a distributed environment, such as in a Hadoop cluster. Parquet is optimized for efficient querying and storage of data and is designed to work with data processing frameworks such as Apache Spark and Impala.

To start with, we create two tables—table 1 (`amazon_reviews_tsv`), which is built on the TSV dataset, and table 2 (`amazon_reviews_parquet`), which is built on the Parquet dataset. The `amazon_reviews_parquet` table is not only using the Parquet file format but is also partitioned by `product_category`, as illustrated in the following screenshot:

```
(base) % aws s3 ls s3://amazon-reviews-pds/parquet/
PRE product_category=Apparel/
PRE product_category=Automotive/
PRE product_category=Baby/
PRE product_category=Beauty/
PRE product_category=Books/
PRE product_category=Camera/
PRE product_category=Digital_Ebook_Purchase/
PRE product_category=Digital_Music_Purchase/
PRE product_category=Digital_Software/
PRE product_category=Digital_Video_Download/
PRE product_category=Digital_Video_Games/
PRE product_category=Electronics/
PRE product_category=Furniture/
PRE product_category=Gift_Card/
PRE product_category=Grocery/
PRE product_category=Health_&_Personal_Care/
PRE product_category=Home/
PRE product_category=Home_Entertainment/
```

Figure 5.3 – Parquet dataset with partitions

This SQL code is for the `amazon_reviews_tsv` table definition:

```
CREATE EXTERNAL TABLE amazon_reviews_tsv( marketplace string,
  customer_id string,
  review_id string,
```