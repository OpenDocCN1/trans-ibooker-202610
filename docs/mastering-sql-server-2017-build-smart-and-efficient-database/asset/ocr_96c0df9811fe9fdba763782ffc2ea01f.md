Here is the output:

| remote_database_id | remote_database_name | data_source_id | federated_service_account |
|--------------------|----------------------|----------------|---------------------------|
| 65536 | RDA\MilaA58713B7-0A91-4237-91C4-1173D113AD10 | 65536 | 0 |

By using the following command, you can get information about archive tables on the Azure side :

```
USE Mila;
SELECT * FROM sys.remote_data_archive_tables;
```

And here is the output:

| object_id | remote_database_id | remote_table_name | filter_predicate |
|-----------|--------------------|-------------------|------------------|
| 917578307 | 65536 | dbo_T1_917578307_74B91309-3B... | ([dbo].[StretchFilter]([c2])) |
| migration_direction | migration_direction_desc | is_migration_paused | is_reconciled |
|---------------------|--------------------------|---------------------|----------------|
| 0 | OUTBOUND | 0 | 1 |

Finally, to see how much space a stretch-enabled table is using in Azure, run the following statement:

```
USE Mila;
EXEC sp_spaceused 'dbo.T1', 'true', 'REMOTE_ONLY';
```

Here is the result of this command:

| name | rows | reserved | data | index_size | unused |
|------|------|----------|------|------------|--------|
| dbo.T1 | 2 | 288 KB | 16 KB | 48 KB | 224 KB |

In the next sections, you will see how to pause or disable the Stretch DB feature.