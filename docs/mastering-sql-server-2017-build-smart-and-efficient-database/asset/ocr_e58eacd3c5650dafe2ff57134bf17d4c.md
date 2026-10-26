+   Dropping temporal tables 191
+   Data manipulation in temporal tables 192
+     Inserting data in temporal tables 192
+     Updating data in temporal tables 193
+     Deleting data in temporal tables 196
+   Querying temporal data in SQL Server 2017 196
+     Retrieving temporal data at a specific point in time 197
+     Retrieving temporal data from a specific period 199
+     Retrieving all temporal data 201
+   Performance and storage considerations with temporal tables 202
+     History retention policy in SQL Server 2017 202
+       Configuring the retention policy at the database level 202
+       Configuring the retention policy at the table level 202
+     Custom history data retention 207
+     History table implementation 208
+     History table overhead 211
+   Temporal tables with memory-optimized tables 213
+   What is missing in SQL Server 2017? 217
+     SQL Server 2016 and 2017 temporal tables and data warehouses 217
+   Summary 221
+   Chapter 6: Columnstore Indexes 222
+     Analytical queries in SQL Server 223
+       Joins and indexes 225
+         Benefits of clustered indexes 229
+         Leveraging table partitioning 231
+         Nonclustered indexes in analytical scenarios 233
+         Using indexed views 235
+       Data compression and query techniques 236
+         Writing efficient queries 241
+   Columnar storage and batch processing 243
+     Columnar storage and compression 243
+       Recreating rows from columnar storage 245
+       Columnar storage creation process 246
+       Development of columnar storage in SQL Server 248
+     Batch processing 251
+   Nonclustered columnstore indexes 257
+     Compression and query performance 257
+       Testing the nonclustered columnstore index 260
+     Operational analytics 266
+   Clustered columnstore indexes 267
+     Compression and query performance 268
+       Testing the clustered columnstore index 269
+       Using archive compression 270
+     Adding B-tree indexes and constraints 272
+     Updating a clustered columnstore index 277
+       Deleting from a clustered columnstore index 281
+   Summary 283