
The SQL used in this section is available in the GitHub repository identified in Chapter 1 under the [`./Chapter2`](https://github.com/starburstdata/optimizing-iceberg-book/blob/main/Chapter2/README.md) folder. All output presented in this chapter is discussed thereby allowing you the same learnings if you do not perform the hands-on exercises.

In your exploratory environment described in Chapter 1, create a schema and an empty Iceberg table.

```
CREATE SCHEMA optimize_ice.ch2;
USE optimize_ice.ch2;

CREATE TABLE my_iceberg_tbl (
  id integer,
  name varchar(55),
  description varchar(255)
) WITH (type='iceberg', format='parquet');
```

Use the query in [Example 1-5](#ch01.html#ch02_er_example_5_1776772288055142) to review the contents of the [`$snapshots`](https://trino.io/docs/current/connector/iceberg.html#snapshots-table) metadata table aligned to the newly created `my_iceberg_tbl` table.

### Example 1-5. Querying the `$snapshots` metadata table.

```
SELECT
 snapshot_id, parent_id,
 substring(manifest_list,
           position('/metadata/' IN manifest_list) + 10)
  AS manifest_list
FROM
 "my_iceberg_tbl$snapshots";
```

The results from [Example 1-5](#ch01.html#ch02_er_example_5_1776772288055142)’s query are shown in [Table 1-2](#ch01.html#ch02_er_table_2_1776772288049520), and identify that a single snapshot is present, which aligns with the table creation operation.

Table 1-2. The first snapshot.

| `snapshot_id` || `parent_id` || `mainifest_list` |

| --- | --- | --- |

| `9195...4485` || `NULL` || `snap-9195...4485-1...bc1b.avro` |

| --- | --- | --- |

#### Note

The snapshot identifiers and names of data and metadata layer files that you will see in your exploratory environment will be different from those displayed in this section. They are independent across environments as well as for each iteration of these steps within the same environment.

Insert a few records into the table.

```
INSERT INTO my_iceberg_tbl (id, name, description)
VALUES
  (101, 'Leto', 'Ruler of House Atreides'),
  (102, 'Jessica', 'Consort of the Duke'),
  (103, 'Paul', 'Son of Leto (aka Dale Cooper)');
```

Verify that 3 rows are present as shown in [Table 1-3](#ch01.html#ch02_er_table_3_1776772288049540) as a result of executing the following query.

```
SELECT * FROM my_iceberg_tbl;
```

Table 1-3. The initial 3 records.

| `id` || `name` || `descriptions` |

| --- | --- | --- |

| `101` || `Leto` || `Ruler of House Atreides` |

| --- | --- | --- |

| `102` || `Jessica` || `Consort of the Duke` |

| `103` || `Paul` || `Son of Leto (aka Dale Cooper)` |

Run the query again in [Example 1-5](#ch01.html#ch02_er_example_5_1776772288055142) to see that there are now two snapshots present as seen in [Table 1-4](#ch01.html#ch02_er_table_4_1776772288049558).

Table 1-4. The second snapshot.

| `snapshot_id` || `parent_id` || `mainifest_list` |

| --- | --- | --- |

| `9195...4485` || `NULL` || `snap-9195...4485-1...bc1b.avro` |

| --- | --- | --- |

| `1621...6722` || `9195...4485` || `snap-1621...6722-1...5e27.avro` |

The second row in [Table 1-4](#ch01.html#ch02_er_table_4_1776772288049558) shows that a new `snapshot_id` was created for the initial three rows added. This row’s `parent_id` value indicates that its parent is the original snapshot. The `manifest_list` column identifies the manifest list file name for each snapshot.

The [`$manifests`](https://trino.io/docs/current/connector/iceberg.html#manifests-and-all-manifests-tables) metadata table provides a detailed overview of the manifests used in the current snapshot. This metadata table has multiple columns, but let’s focus on those that help us understand characteristics of the data files that each manifest references.

Run the query in [Example 1-6](#ch01.html#ch02_er_example_6_1776772288055158) to see details about the manifest files that are linked to the latest manifest list. The results are shown in [Table 1-5](#ch01.html#ch02_er_table_5_1776772288049574).

### Example 1-6. Querying the `$manifests` metadata table.

```
SELECT
 substring(path,
           position('/metadata/' IN path) + 10)
  AS path,
 added_snapshot_id AS add_snap,
 added_data_files_count AS add_file_cnt,
 added_rows_count AS add_row_cnt
FROM
 "my_iceberg_tbl$manifests";
```

Table 1-5. Initial manifest file listing.

| `path` || `add_snap` || `add_file_cnt` || `add_row_cnt` |

| --- | --- | --- | --- |

| `76849afc...5e27-m0.avro` || `1621...6722` || `1` || `3` |

| --- | --- | --- | --- |

The following details, by column, are represented in [Table 1-5](#ch01.html#ch02_er_table_5_1776772288049574).

*   `path` - The file name of the manifest file that was created.
*   `add_snap` - The snapshot identifier that was created when this particular manifest file was created. Notice that it is the same identifier from the second row in [Table 1-4](#ch01.html#ch02_er_table_4_1776772288049558).
*   `add_file_cnt` - This manifest file only has a single data file associated with it.
*   `add_row_cnt` - Aligns to the 3 rows that were inserted into this table for the given snapshot.

Run the query in [Example 1-7](#ch01.html#ch02_er_example_7_1776772288055174) to see details of all referenced files for the current snapshot. The results are shown in [Table 1-6](#ch01.html#ch02_er_table_6_1776772288049592).

### Example 1-7. Querying the [`$files`](https://trino.io/docs/current/connector/iceberg.html#files-table) metadata table.

```
SELECT
  substring(file_path,
            position('/data/' IN file_path) + 6)
    AS file_path,
  record_count as recs, 
  value_counts AS value_cnts, 
  null_value_cnts AS null_value_cnts, 
  lower_bounds, upper_bounds
FROM
  "my_iceberg_tbl$files";
```

Table 1-6. Initial data files listing.

| `file_path` || `recs` || `value_cnts` || `null...cnts` || `lower_bounds` || `upper_bounds` |

| --- | --- | --- | --- | --- | --- |

| `20...aa.parquet` || `3` || `{ `  

` 1 = 3, `  

` 2 = 3, `  

` 3 = 3 `  

`}` || `{ `  

` 1 = 0, `  

` 2 = 0, `  

` 3 = 0 `  

`}` || `{ `  

` 1 = 101, `  

` 2 = Jessica, `  

` 3 = Consor... `  

`}` || `{ `  

` 1 = 103, `  

` 2 = Paul, `  

` 3 = Son of... `  

`}` |

| --- | --- | --- | --- | --- | --- |

The following details, by column, are represented in [Table 1-6](#ch01.html#ch02_er_table_6_1776772288049592).

*   `file_path` - The name of the single data file linked to the manifest file in [Table 1-5](#ch01.html#ch02_er_table_5_1776772288049574). The remainder of the columns relate to this particular file.
*   `recs` - Identifies this data file has three records which is also shown as `add_row_cnt` in [Table 1-5](#ch01.html#ch02_er_table_5_1776772288049574).
*   `value_cnts` - The number of values in each column. Each of the columns has three values.
*   `null_value_cnts` - The number of `null` values in each column. The count is zero for each column.
*   `lower_bounds` - The lowest value in each column. For example, the first column, `id`, has `101` as a lower bound.
*   `upper_bounds` - The highest value in each column. Staying with the same example, the `id` column has `103` as an upper bound.

Add additional records to the table.

```
INSERT INTO my_iceberg_tbl (id, name, description)
VALUES
  (104, 'Thufir', 'Mentat'),
  (201, 'Vladimir', 'Ruler of House Harkonnen'),
  (202, 'Rabban', 'Ruthless nephew of Vladimir'),
  (203, 'Feyd-Rautha', 'Savvy nephew of Vladimir'),
  (301, 'Reverend Mother Gaius Helen Mohiam', null);
```

After verifying 5 more rows are present in the table for a total of 8, run the query from [Example 1-5](#ch01.html#ch02_er_example_5_1776772288055142) to see the third snapshot listed as the last row in [Table 1-7](#ch01.html#ch02_er_table_7_1776772288049606).

Table 1-7. The third snapshot.

| `snapshot_id` || `parent_id` || `mainifest_list` |

| --- | --- | --- |

| `9195...4485` || `NULL` || `snap-9195...4485-1...bc1b.avro` |

| --- | --- | --- |

| `1621...6722` || `9195...4485` || `snap-1621...6722-1...5e27.avro` |
