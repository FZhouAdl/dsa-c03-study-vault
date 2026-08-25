---
title: "snowflake.snowpark.DataFrame.create_or_replace_dynamic_table | Snowflake Documentation"
source: https://docs.snowflake.com/en/developer-guide/snowpark/reference/python/latest/snowpark/api/snowflake.snowpark.DataFrame.create_or_replace_dynamic_table.html
cert_domain: domain-2-data-preparation-and-feature-engineering
crawl_depth: 1
crawled: 2026-08-23
---

# snowflake.snowpark.DataFrame.create_or_replace_dynamic_table¶

DataFrame.create_or_replace_dynamic_table(_name : Union[str, Iterable[str]]_, _*_ , _warehouse : str_, _lag : str_, _comment : Optional[str] = None_, _mode : str = 'overwrite'_, _refresh_mode : Optional[str] = None_, _initialize : Optional[str] = None_, _clustering_keys : Optional[Iterable[Union[[Column](snowflake.snowpark.Column.html#snowflake.snowpark.Column "snowflake.snowpark.column.Column"), str]]] = None_, _is_transient : bool = False_, _data_retention_time : Optional[int] = None_, _max_data_extension_time : Optional[int] = None_, _statement_params : Optional[Dict[str, str]] = None_, _iceberg_config : Optional[dict] = None_, _copy_grants : bool = False_) → List[[Row](snowflake.snowpark.Row.html#snowflake.snowpark.Row "snowflake.snowpark.row.Row")][[source]](https://github.com/snowflakedb/snowpark-python/blob/v1.54.0/src/snowflake/snowpark/dataframe.py#L5572-L5733)¶
    

Creates a dynamic table that captures the computation expressed by this DataFrame.

For `name`, you can include the database and schema name (i.e. specify a fully-qualified name). If no database name or schema name are specified, the dynamic table will be created in the current database or schema.

`name` must be a valid [Snowflake identifier](https://docs.snowflake.com/en/sql-reference/identifiers-syntax.html).

Parameters:
    

  * **name** – The name of the dynamic table to create or replace. Can be a list of strings that specifies the database name, schema name, and view name.

  * **warehouse** – The name of the warehouse used to refresh the dynamic table.

  * **lag** – specifies the target data freshness

  * **comment** – Adds a comment for the created table. See [COMMENT](https://docs.snowflake.com/en/sql-reference/sql/comment).

  * **mode** – Specifies the behavior of create dynamic table. Allowed values are: \- “overwrite” (default): Overwrite the table by dropping the old table. \- “errorifexists”: Throw and exception if the table already exists. \- “ignore”: Ignore the operation if table already exists.

  * **refresh_mode** – Specifies the refresh mode of the dynamic table. The value can be “AUTO”, “FULL”, or “INCREMENTAL”.

  * **initialize** – Specifies the behavior of initial refresh. The value can be “ON_CREATE” or “ON_SCHEDULE”.

  * **clustering_keys** – Specifies one or more columns or column expressions in the table as the clustering key. See [Clustering Keys & Clustered Tables](https://docs.snowflake.com/en/user-guide/tables-clustering-keys) for more details.

  * **is_transient** – A boolean value that specifies whether the dynamic table is transient.

  * **data_retention_time** – Specifies the retention period for the dynamic table in days so that Time Travel actions can be performed on historical data in the dynamic table.

  * **max_data_extension_time** – Specifies the maximum number of days for which Snowflake can extend the data retention period of the dynamic table to prevent streams on the dynamic table from becoming stale.

  * **statement_params** – Dictionary of statement level parameters to be set while executing this action.

  * **iceberg_config** – 

A dictionary that can contain the following iceberg configuration values:

    * partition_by: specifies one or more partition expressions for the Iceberg table. Can be a single Column, column name, SQL expression string, or a list of these. Supports identity partitioning (column names) as well as partition transform functions like bucket(), truncate(), year(), month(), day(), hour().

    * external_volume: specifies the identifier for the external volume where the Iceberg table stores its metadata files and data in Parquet format.

    * catalog: specifies either Snowflake or a catalog integration to use for this table.

    * base_location: the base directory that snowflake can write iceberg metadata and files to.

    * target_file_size: specifies a target Parquet file size for the table. Valid values: ‘AUTO’ (default), ‘16MB’, ‘32MB’, ‘64MB’, ‘128MB’

    * catalog_sync: optionally sets the catalog integration configured for Polaris Catalog.

    * storage_serialization_policy: specifies the storage serialization policy for the table.

  * **copy_grants** – A boolean value that specifies whether to retain the access permissions from the original view when a new view is created. Defaults to False.




Note

See [understanding dynamic table refresh](https://docs.snowflake.com/en/user-guide/dynamic-tables-refresh). for more details on refresh mode.
