

当前快照的标识符。更多细节可参见 [示例 1-3](#ch01.html#ch02_er_example_3_1776772288055109) 中展示的快照信息。

[![2](assets/2.png)](#ch01.html#co_ch1_4_2)

分支和/或标签引用的列表。

[![3](assets/3.png)](#ch01.html#co_ch1_4_3)

此单一引用表示快照存在于名为 `branch` 的 `main` 上。

###### Note

存在额外的元数据；请参考 [Table Metadata and Snapshots](https://iceberg.apache.org/spec/#table-metadata-and-snapshots) 以获取此 JSON 文件完整模式定义。

### Manifest list

如 [示例 1-3](#ch01.html#ch02_er_example_3_1776772288055109) 所示，表元数据文件还标识了每个列出快照的 *manifest list* 文件名。每个快照指向单个 manifest list。此文件有时被称为 *snapshot manifest*，以 [Apache Avro](https://avro.apache.org/) 格式存储。它存储在 `/metadata` 文件夹中，命名约定为 `snap-*.avro`。manifest list 包含的就是——构成当前快照内容的 *manifest files* 列表。manifest list 还跟踪哪些 manifest files 属于每个 *partition*。

###### Note

数据湖表 *partitioning* 是根据特定列值（如日期、区域或类别）将大型数据集物理组织成更小、更易管理的目录。分区表的优点在第 3 章中介绍，第 4 章确定了旨在优化性能的最佳实践。

manifest 文件的列表不仅仅是简单的文件指针列表。每个条目都显示摘要信息，例如引用的实际数据文件数量、数据大小、关键字段的上下界，以及其他可以帮助计算引擎尽可能高效地处理查询的元数据。

### Manifest file

*manifest file* 指向一个或多个数据文件以及每个文件的汇总统计信息。就像 manifest list 为其相关的 manifest files 维护摘要信息一样，manifest file 为其相关的数据文件维护摘要信息。这些 AVRO 文件可通过其 `*-m<N>.avro` 命名约定识别。遍历 [图 1-2](#ch01.html#ch02_er_figure_2_1776772288044140) 中快照 0 的树结构，你可以看到此 Iceberg 表当前版本的 content 由四个数据文件组成。

元数据层和数据层中的所有文件都是不可变的，一旦创建就无法修改。[图 1-3](#ch01.html#ch02_er_figure_3_1776772288044162) 显示了在创建新快照时创建了额外的元数据层内容。请注意以下几点：

1. 目录指针已更改为新的表元数据文件。
2. 新的表元数据文件包含快照列表并将当前标识为快照 1。
3. 快照 1 指向新的 manifest list。
4. 新的 manifest list 包含之前的 2 个 manifest files 及其代表的 4 个数据文件。
5. 新的 manifest list 还包含一个新的 manifest file，该文件指向 2 个新的数据文件。

![](assets/ch02_er_figure_3_1776772288044162.png)

## Metadata Pruning

数据湖表最终由许多数据文件组成。无论使用的是 Apache Hive 还是 Apache Iceberg 等哪种表格式，最坏的情况是计算引擎需要读取所有底层数据文件。一般策略是寻找跳过或 *prune* 要访问文件的机会。

如 [图 1-4](#ch01.html#ch02_er_figure_4_1776772288044181) 所示，Parquet 等文件格式包含元数据，其中包括所包含记录的摘要信息。它可视化了 5 个数据文件，并显示了每个文件中所有客户标识符列值的上下界。

![](assets/ch02_er_figure_4_1776772288044181.png)

由于此信息存储在数据文件的页脚中，计算引擎可以快速检查每个数据文件，以确定查询条件是否值得进行更彻底的检查。[表 1-1](#ch01.html#ch02_er_table_1_1776772288049481) 标识了查询谓词以及从 [图 1-4](#ch01.html#ch02_er_figure_4_1776772288044181) 中需要更详尽读取以尝试为查询找到相关信息的数据文件。

表 1-1. 基于谓词的相关数据文件

| Query predicate || Relevant data files |
| --- | --- |
| `WHERE custID = 22` || Files 101, 103, 104 |
| --- | --- |
| `WHERE custID BETWEEN 82 AND 84` || Files 101, 102, 104, 105 |
| `WHERE custID = 12` || Files 103, 104 |
| `WHERE custID IN (2, 122, 132)` || No files |

如 [表 1-1](#ch01.html#ch02_er_table_1_1776772288049481) 中的第三个条目所示，如果查询搜索客户 ID 12，则只有五个数据文件中的两个值得比仅读取文件页脚中的摘要信息进行更详尽的查看。仅有五个数据文件时，在任何集群计算引擎上都不会看到太多好处，但如果这五个条目中的每一个代表 10,000 个文件，则在这种情况下，必须详尽读取 20,000 个文件而不是 50,000 个文件将带来显著的性能提升。

[表 1-1](#ch01.html#ch02_er_table_1_1776772288049481) 中的最后一个条目显示，对于此查询谓词，不需要考虑任何文件来获得其所属的总体查询的结果。这比其他 3 个示例提供的性能提升更显著。对于原始表格式 Apache Hive，计算引擎必须为每个查询访问全部 5 个文件。必须读取页脚信息以确定是否需要更详尽的读取。如前所述，对于只有 5 个数据文件来说不是问题，但在有 50,000 个数据文件时就是大量工作。

Apache Iceberg 通过改进这一点，因为数据文件的汇总统计元数据存储在引用数据文件的 manifest file 中，如 [图 1-5](#ch01.html#ch02_er_figure_5_1776772288044198) 所示。这允许计算引擎完全剔除与查询无关的任何数据文件。

![](assets/ch02_er_figure_5_1776772288044198.png)

通过基于元数据的 *pruning*，[表 1-1](#ch01.html#ch02_er_table_1_1776772288049481) 中确定的相同数据文件在范围内。好处是消除了访问所有数据文件的页脚信息的初始传递。在每个数据文件代表 10,000 个文件的夸张示例中，这将消除访问 50,000 个数据文件以读取其页脚以确定哪些文件可以 *pruned* 的需求。

###### Note

文件修剪是一种 *predicate pushdown*，定义为一种查询优化技术，将过滤条件下推到数据存储层，从而减少 I/O 并提高性能。它消除了 *table scan* 的需求——读取表中所有记录的需求。

## Inspecting Metadata

如 [示例 1-1](#ch01.html#ch02_er_example_1_1776772288055053) 所可视化，如果你有权访问 Iceberg 表存储的存储库位置，你可以在 `./metadata` 子目录中找到这些文件。检索和读取这些 JSON 和 Avro 文件可能会令人困惑，因为为表创建了新的快照。幸运的是，大多数与 Iceberg 集成的计算引擎都提供了更简单的方式来查看表的元数据。

官方 Iceberg 文档详细介绍了如何使用 Apache Spark 在 [Inspecting tables](https://iceberg.apache.org/docs/nightly/spark-queries/#inspecting-tables) 部分执行此操作。对于本节提供的动手示例，你将利用 Trino Iceberg 连接器文档中关于 [Metadata tables](https://trino.io/docs/current/connector/iceberg.html#metadata-tables) 的部分中介绍的功能，该功能提供对表元数据的 SQL 接口。

###### Tip

