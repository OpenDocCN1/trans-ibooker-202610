Also, It provides many helpful modules, a couple of which are listed here:

+   - **Repair table**: This runs the `MSCK REPAIR` command to update the table metadata with the most recent data from the storage layer. This function executes Hive’s metastore consistency check – `MSCK REPAIR TABLE <tablename>`. The command recovers partitions and data associated with partitions and updates the table definition. This is needed to query the data that is available under those partitions. This is useful to execute when new partitions are created from an ETL job that doesn’t update the Glue catalog.
+   - **Wait query**: This iterates through the query status every 250 ms until the query moves to either FAILED, SUCCEEDED, or CANCELLED status (the final status for a query execution). You have to customize your code if you want to verify query execution status, using a shorter or longer polling mechanism.

The `awswrangler` package takes care of data type conversion internally. The Parquet files from the resulting folder are read using S3 read functions (which in turn uses `pyarrow` to read the S3 object). The data type conversion of the Parquet files – arrow – `athena` are handled internally within the `awswrangler` package. The following snippet from the `data_types.py` module shows the conversion of the `pyarrow` and `athena` data types.

```
def pyarrow2athena(  # pylint: disable=too-many-branches,too-many-return-statements
    dtype: pa.DataType, ignore_null: bool = False
) -> str:
    """Pyarrow to Athena data types conversion."""
    if pa.types.is_int8(dtype):
        return "tinyint"
    if pa.types.is_int16(dtype) or pa.types.is_uint8(dtype):
        return "smallint"
    if pa.types.is_int32(dtype) or pa.types.is_uint16(dtype):
        return "int"
    if pa.types.is_int64(dtype) or pa.types.is_uint32(dtype):
        return "bigint"
    if pa.types.is_uint64(dtype):
        raise exceptions.UnsupportedType("There is no support for uint64, please conside
    if pa.types.is_float32(dtype):
        return "float"
    if pa.types.is_float64(dtype):
```

Figure 3.55: Athena integration – pyarrow data type conversions

# RDS databases

There is a good amount of integration functionalities available within `awswrangler` to connect with relational databases, such as Postgres, Oracle, SQL Server, and MySQL. In addition to this, `awswrangler` supports DataAPI-based access to Redshift and Aurora Serverless databases.