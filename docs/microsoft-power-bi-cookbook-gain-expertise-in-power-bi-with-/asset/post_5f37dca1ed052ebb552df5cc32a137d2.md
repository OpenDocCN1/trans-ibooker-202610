
## Enhancing and Optimizing Existing Power BI Solutions

For import mode models, an expensive column is one with many unique values (high cardinality), such as the **Order Number** columns used as examples in *Chapter 2, Accessing and Retrieving Data*. Likewise, a **DateTime** column with multiple time values per date will consume more memory than two separate **Date** and **Time** columns. Preferably, only the **Date** column or only the **Date** and **Time** columns should be imported, rather than the **DateTime** column.

Also, per the *Selecting and Renaming Columns* recipe, DAX measures that execute simple arithmetic against low-cardinality columns, such as **Unit Price** and **Quantity**, can eliminate the need to import more expensive derived columns such as **Sales Amount** and **Sales Amount with Taxes**. Furthermore, though counter-intuitive, the SUMX measure with arithmetic across multiple columns often outperforms the simple SUM measure.

- 3. Identify columns that are stored as decimal number data types with a high scale (number of digits to the right of the decimal point). If this level of precision is not required, consider rounding off these columns in the SQL view or via the M import query to reduce the cardinality (unique values) and thus improve compression. If a (19,4) column will provide sufficient size and precision, apply the fixed decimal number type in the model.

- 4. Replace any DAX calculated columns on large fact tables since imported columns achieve much better compression than calculated columns. Calculated columns on fact tables can often be addressed with DAX measures without sacrificing performance but if DAX measures are not an option, move the column's logic to the SQL view or M query of the fact table, or within the data source itself. If the M query is revised, ensure that the logic is folded to the source system.

- 5. Remove or replace DAX calculated columns on any large dimension tables as well by moving this logic to the data retrieval process or leveraging the source system.

Look for calculated columns with a RELATED or LOOKUPVALUE function, which, like an Excel VLOOKUP function, simply retrieves column values from a table on the one side of a many-to-one relationship with a fact table. Business users often utilize the RELATED and LOOKUPVALUE functions to flatten or de-normalize a fact table as they would in standard Excel worksheets, but this duplication is rarely necessary in Power BI, and calculated columns are not compressed like standard imported columns. Additionally, look to migrate the logic of calculated column expressions, such as calculated dates, differences in dates, and derived numerical columns, into DAX measures.

- 6. In this example, the current state of the dataset is 334 MB of compressed disk space (the size of the PBIX file converted from KB) and 674 MB of total memory per the memory report introduced in *Chapter 10, Administering and Monitoring Power BI*.

Following the steps outlined, several quick wins are identified on the Reseller Sales fact table (11.7M rows), including the following:

- Only the last four characters of the CarrierTrackingNumber are needed for analysis.
- The Order Date, Ship Date, and Due Date columns in YYYYMMDD format can be removed, as they are redundant with the date data types for these columns, and only the date data types are used for relationships.
- Four calculated columns can be removed (Days between Due Date, Order Days, Reseller, and Product Name) as a DATEDIFF DAX measure, and existing dimension columns can be used instead.
- The Sales Amount, Extended Amount, and Total Product Cost columns can be removed, as simple DAX measures can compute their values.

![](img/0b67f3eabca89f1e12f62eac78fb9ffb_511_0.png)

After making the revisions, the dataset is now 429 MB in memory and the Power BI Desktop file (PBIX) is 221 MB on disk, representing 33%+ savings in memory and disk space.

Large-dimension tables (approximately 1 million+ rows), with their high-cardinality relationships to fact tables, are a major performance bottleneck with Power BI and SSAS Tabular import models. Consider the following dimension table with attributes describing both resellers and promotions:

![](img/0b67f3eabca89f1e12f62eac78fb9ffb_511_1.png)

- 7. The consolidated table contains 10,520 rows and the relationship column on the **Reseller Sales** table is 19.4 MB in size.

| Column | Total Column Size (MB) | Data Size (MB) | Dictionary Size (MB) | VERTIPAQ_STATE |
|---|---:|---:|---:|---|
| CarrierTrackingSuffix | 23.9 | 22.3 | 1.6 | COMPLETED |
| ResellerPromoKey | 19.4 | 19.2 | 0.2 | COMPLETED |
| OrderDate | 16.7 | 16.5 | 0.2 | COMPLETED |

Figure 11.3: Reseller promo key, approximately 20 MB in size

- 8. We can split (normalize) this table into smaller **Reseller** (701 rows) and **Promotion** (16 rows) dimension tables and drop the consolidated **Reseller Promotion** dimension table and the expensive ResellerPromoKey column on the fact table.

![](img/0b67f3eabca89f1e12f62eac78fb9ffb_512_0.png)

Figure 11.4: Reseller and Promotion Tables Replace Consolidated Reseller Promotion

Smaller relationships improve the performance of queries accessing the **Promotion** and **Reseller** columns. Additionally, the size of the dataset will be reduced by removing the ResellerPromoKey relationship column.

In this particular example, the row counts are small enough that little impact is observed, but consider splitting large dimension tables over 200,000 rows into smaller tables (lower granularity) as query workloads increase. For example, a 1 million-row customer table could possibly be split into two tables for the data model based only on common query patterns such as customer regions or geographies.

### How it works...

Remember that DAX queries executed against import mode models access and scan the memory associated with individual columns. Therefore, several very expensive columns with millions of unique values could be present on a fact table but may not negatively impact the performance of a query that does not reference these columns. Removing these expensive columns or replacing them with less expensive columns reduces the overall size of the dataset but you should not expect query performance to improve.

When data is loaded into a Power BI Desktop model (import mode), the VertiPaq storage engine applies compression algorithms to each column to reduce the memory and thus improve performance. VertiPaq first stores all unique values of a column (either via value encoding or hash encoding), and then, more importantly, applies **run-length encoding (RLE)** to store a repeated value only once for a set of contiguous rows in which it appears. Therefore, columns with few unique values, such as month names, are highly compressed, while primary key and GUID columns are not compressed at all.

The data models in Power BI Desktop (and Power Pivot for Excel) are stored in column segments of 1 million rows. For example, a 20 million-row sales fact table will contain approximately 20 distinct segments. If the data required of report queries is spread across all 20 segments, then more resources (and a longer duration) are required to access each segment and consolidate these results to resolve the query. However, if the segments are ordered by date or perhaps by a given dimension (for example, **StoreID**) and a report query contains a filter that uses this order, such as fiscal year or store region, then only a subset of the segments is queried.

As a simple example, assume a 20 million-row fact table is ordered by date when importing to Power BI and each calendar year represents 1 million rows. A report query that is filtered on only 2 years will therefore need to access only two of the 20 column segments as the other 18 segments contain dates outside the scope of the query.

VertiPaq Analyzer is now integrated into DAX Studio under the **Advanced** menu as well as in Tabular Editor 3. VertiPaq Analyzer is an easy way to quickly identify the columns consuming the most memory. This and the Best Practice Analyzer in Tabular Editor are two of the most common and easy-to-use tools to analyze the health of a Power BI dataset.

### There's more...

While analyzing the data model and making optimizations is important, optimizations can also be made within the Power Query Editor. For example, avoid loading tables that are only used for data retrieval/transformation logic, such as staging queries, to a data model. Even if hidden from the **Fields** list, these tables consume processing and storage resources like all other tables of the model and add unnecessary complexity. Right-click on these queries in the Power Query Editor and disable **Enable load** to remove the table from the data model. Keep in mind that data models with many M queries, whether loaded or not, can overwhelm the available threads/resources of the source system during a refresh as all queries are submitted simultaneously.

Also, identify tables that rarely change and consider disabling the default **Include in report refresh** property (by right-clicking the query in the Power Query Editor). The table can still be loaded to the data model and thus be available for relationships and DAX measures, but its source query will no longer be executed with each refresh. Typical candidates for this include an annual budget or plan table that is only updated once a year, a Currency table, and possibly a geographic or demographic table.
