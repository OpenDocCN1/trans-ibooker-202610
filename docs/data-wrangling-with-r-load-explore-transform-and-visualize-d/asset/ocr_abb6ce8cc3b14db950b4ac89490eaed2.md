advantageous to frontend applications. However, they consume more size and memory to process compared to CSV files.

The main reason to choose a readable file format would be for use cases where the users would want to download and use the file for different use cases (e.g., sharing a user list to follow up with a phone call from another team):

+   - You can choose the CSV file format if the data structure is relatively simple and doesn’t have hierarchical fields as it is easily readable and lightweight. CSV files combined with Excel will provide good visualizations for lightweight data analysis.
+   - Choose the JSON file format if the data has more complex data types and you need to store different data formats. In the preceding example, we moved from the CSV to JSON file format as our data had commas in it and processing that data using the pandas library provided more challenges.
+   - **Binary file formats:** Parquet, Avro, and ORC are some common file formats that are not human-readable and are used in the data pipeline during the analysis phase. These file formats are stored in a machine-readable binary format and can be split and stored across multiple disks, thereby supporting parallel processing. These file formats are widely used in the big data processing landscape. They are self-described, meaning they store the schema and data together, so the processing systems can read the data easily.

What is row versus column storage? Parquet and ORC file formats store data in a columnar format and Avro stores data in a record-level format. A columnar format stores data from the same column together for a chunk of records. This is helpful in analytical systems where the data is aggregated and visualized for specific columns. Storing data from columns nearby helps to read the data faster and skip reading other columns that are not used in the analytical query. For example, see the following:

| Name | stat1 | stat2 |
|---|---|---|
| Test_user1 | 12 | 13 |
| Test_user2 | 14 | 15 |
| Test_user3 | 16 | 17 |

Table 9.1: Raw data (with a record chunk – 3 records)

Here is record-level storage:

| Test_user1 | 12 | 13 | Test_user2 | 14 | 15 | Test_user3 | 16 | 17 |

Table 9.2: Record-level storage for the table

Here is columnar storage:

| Test_user1 | Test_user2 | Test_user3 | 12 | 14 | 16 | 13 | 15 | 17 |