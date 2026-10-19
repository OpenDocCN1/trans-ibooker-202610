
| `4498...8325` || `1621...6722` || `snap-4498...8325-1...bbe8.avro` |

Run [Example 1-6](#ch01.html#ch02_er_example_6_1776772288055158)’s query again, whose results list 2 manifests as shown in [Table 1-8](#ch01.html#ch02_er_table_8_1776772288049621).

### Table 1-8. Second manifest file listing.

| `path` || `add_snap` || `add_file_cnt` || `add_row_cnt` |

| --- | --- | --- | --- |

| `7887ee42...bbe8-m0.avro` || `4498...8325` || `1` || `5` |

| --- | --- | --- | --- |

| `76849afc...5e27-m0.avro` || `1621...6722` || `1` || `3` |

The row listing an `add_snap` value of `4498...8325` shows the linkage back to the third snapshot in [Table 1-7](#ch01.html#ch02_er_table_7_1776772288049606). Adding up the values of the `add_file_cnt` and `add_row_cnt` columns shows that there are two data files present now with a total of eight rows.

Execute the query in [Example 1-7](#ch01.html#ch02_er_example_7_1776772288055174) again to see statistical details of these 2 files as displayed in [Table 1-9](#ch01.html#ch02_er_table_9_1776772288049637).

### Table 1-9. Second data files listing.

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

| `20...1e.parquet` || `5` || `{ `  
` 1 = 5, `  
` 2 = 5, `  
` 3 = 5 `  
`}` || `{ `  
` 1 = 0, `  
` 2 = 0, `  
` 3 = 1 `  
`}` || `{ `  
` 1 = 104, `  
` 2 = Feyd-..., `  
` 3 = Mentat `  
`}` || `{ `  
` 1 = 301, `  
` 2 = Vladimir,`  
` 3 = Savvy…`  
`}` |

The following details, by column, are represented for the last row in [Table 1-9](#ch01.html#ch02_er_table_9_1776772288049637).

* `recs` - The new data file has 5 records, which is also shown in [Table 1-8](#ch01.html#ch02_er_table_8_1776772288049621).
* `null_value_cnts` - One of the columns has a `null` value.
* `lower_bounds` - The id column shows `104` as the lower bound for this new file.
* `upper_bounds` - The id column shows `301` as the upper bound for this new file.

As mentioned at the beginning of this subsection, the logical metadata tables offer a SQL interface to the physical metadata layer’s content. The upper/lower bounds details from [Table 1-9](#ch01.html#ch02_er_table_9_1776772288049637) are representative of the types of metadata visualized in [Figure 1-5](#ch01.html#ch02_er_figure_5_1776772288044198). As stated earlier, values such as these allow the compute engine to perform metadata-based pruning without the expense of reading the actual footers of the data files.

# Key Strengths and Weaknesses

No architecture exists without pros & cons. This rich metadata-driven architecture offers multiple compute engines to function alongside each other on the same tables. This same metadata can end up being a consequence once many snapshots are created.

## Table Maintenance

The benefits of the enhanced metadata layer available in Apache Iceberg, coupled with the immutable nature of the table’s data and metadata layers on the data lake, create suboptimal situations over time. Metadata layer files benefit from consolidation and rebuilding periodically.

Differences in velocity & volume of data ingestion and content modifications often result in data files of widely varied numbers of rows contained within. Chapter 4 describes the “small files problem” that often plagues performance. Conceptually, consolidating and rewriting can resolve these issues. These issues can be kept well within control by appropriate `table maintenance`.

Table maintenance is an umbrella term for multiple optimization-oriented actions that can be triggered manually or scheduled for periodic execution. Each compute engine that integrates with Iceberg may have different mechanisms or syntax to initiate table maintenance activities, but the updated results are consumable by other compute engines.

Most table maintenance activities, or `jobs`, result in a new snapshot being created. This means that maintenance jobs could be running concurrently alongside other activities such as inserting, deleting, and/or updating records.

Iceberg supports concurrency, but it does not feature the feature richness that traditional RDBMS such as Oracle and PostgreSQL do in this area. It relies on an optimistic locking strategy and requires compute engines to implement a retry model when contention with that strategy occurs. More details can be found in the [concurrent write operations](https://iceberg.apache.org/docs/1.6.0/reliability/#concurrent-write-operations) Iceberg documentation.

The partitioning strategy defined for the table can also cause table maintenance to be difficult. Chapter 4 will identify partitioning guidelines that can lessen, or eliminate, this concern. While it is not always possible, one goal of the partition definition is to allow data within a given partition to eventually no longer need maintenance.

Lastly, data maintenance is not always easy on the wallet. You incur compute and storage costs as a result of these jobs. Finding a situation where the costs and the benefits of table maintenance are optimal is often highly dependent on multiple factors and will likely be unique for each of your largest tables.

Fortunately, as detailed in Chapter 7, most data platform providers have automated table maintenance features available to offload the responsibility of defining, tuning, and scheduling these jobs. Most data lakehouses have a multitude of tables that span a range of size, modifications, and access. I recommend using automated table maintenance services for the majority of your tables.

For the very largest tables and tables of any size that have the highest query & modification concurrent activity, it is advisable to compare an automated solution with your own scenario-specific configuration and scheduling. Fortunately, these automated services will continue to mature. The details they log become the context that will likely allow them to understand patterns better and ultimately offer optimal table maintenance for all tables.

Chapter 6 will provide a more exhaustive focus on table maintenance explanations and recommendations. As an introduction, [Table 1-10](#ch01.html#ch02_er_table_10_1776772288049655) lists the most widely known and utilized table maintenance activities.

### Table 1-10. Table maintenance types.

| Operation || Description || Trino function || Spark function |

| --- | --- | --- | --- |

| Compaction || Reads many small files and rewrites the contents into fewer, larger ones to allow compute engines to read from the table more efficiently. || `optimize` || `rewriteDataFiles` |

| --- | --- | --- | --- |

| Snapshot expiration || Deletes old table snapshots and their associated data & metadata layer files to free up storage space. || `expire_snapshots` || `expireSnapshots` |

| Orphan file deletion || Deletes any data files that are no longer referenced by any snapshot. || `remove_orphan_files` || `deleteOrphanFiles` |

| Rewriting manifests || Optimizes manifest files to speed up table planning and query performance. || `optimize_manifests` || `rewriteManifests` |

The majority of these fundamental issues existed in some form or another prior to Iceberg and other modern table formats. Iceberg fortunately offers us structured ways to solve these problems. Solving them in a standard way leads to the one of the most important benefits of Iceberg - allowing multiple compute engines to work on the same tables as others.

## Compute Engine Interoperability
