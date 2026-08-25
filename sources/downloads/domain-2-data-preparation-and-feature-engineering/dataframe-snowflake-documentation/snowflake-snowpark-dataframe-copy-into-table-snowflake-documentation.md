---
title: "snowflake.snowpark.DataFrame.copy_into_table | Snowflake Documentation"
source: https://docs.snowflake.com/en/developer-guide/snowpark/reference/python/latest/snowpark/api/snowflake.snowpark.DataFrame.copy_into_table.html
cert_domain: domain-2-data-preparation-and-feature-engineering
crawl_depth: 1
crawled: 2026-08-23
---

# snowflake.snowpark.DataFrame.copy_into_table¶

DataFrame.copy_into_table(_table_name : Union[str, Iterable[str]]_, _*_ , _files : Optional[Iterable[str]] = None_, _pattern : Optional[str] = None_, _validation_mode : Optional[str] = None_, _target_columns : Optional[Iterable[str]] = None_, _transformations : Optional[Iterable[Union[[Column](snowflake.snowpark.Column.html#snowflake.snowpark.Column "snowflake.snowpark.column.Column"), str]]] = None_, _format_type_options : Optional[Dict[str, Any]] = None_, _statement_params : Optional[Dict[str, str]] = None_, _iceberg_config : Optional[dict] = None_, _** copy_options: Any_) → List[[Row](snowflake.snowpark.Row.html#snowflake.snowpark.Row "snowflake.snowpark.row.Row")][[source]](https://github.com/snowflakedb/snowpark-python/blob/v1.54.0/src/snowflake/snowpark/dataframe.py#L4739-L5009)¶
    

Executes a [COPY INTO <table>](https://docs.snowflake.com/en/sql-reference/sql/copy-into-table.html) command to load data from files in a stage location into a specified table.

It returns the load result described in [OUTPUT section of the COPY INTO <table> command](https://docs.snowflake.com/en/sql-reference/sql/copy-into-table.html#output). The returned result also depends on the value of `validation_mode`.

It’s slightly different from the `COPY INTO` command in that this method will automatically create a table if the table doesn’t exist and the input files are CSV files whereas the `COPY INTO <table>` doesn’t.

To call this method, this DataFrame must be created from a [`DataFrameReader`](snowflake.snowpark.DataFrameReader.html#snowflake.snowpark.DataFrameReader "snowflake.snowpark.DataFrameReader").

Example:
[code] 
    >>> # Create a CSV file to demo load
    >>> import tempfile
    >>> with tempfile.NamedTemporaryFile(mode="w+t") as t:
    ...     t.writelines(["id1, Product A", "\n" "id2, Product B"])
    ...     t.flush()
    ...     create_stage_result = session.sql("create or replace temp stage test_stage").collect()
    ...     put_result = session.file.put(t.name, "@test_stage/copy_into_table_dir", overwrite=True)
    >>> # user_schema is used to read from CSV files. For other files it's not needed.
    >>> from snowflake.snowpark.types import StringType, StructField, StringType
    >>> from snowflake.snowpark.functions import length
    >>> user_schema = StructType([StructField("product_id", StringType()), StructField("product_name", StringType())])
    >>> # Use the DataFrameReader (session.read below) to read from CSV files.
    >>> df = session.read.schema(user_schema).csv("@test_stage/copy_into_table_dir")
    >>> # specify target column names.
    >>> target_column_names = ["product_id", "product_name"]
    >>> drop_result = session.sql("drop table if exists copied_into_table").collect()  # The copy will recreate the table.
    >>> copied_into_result = df.copy_into_table("copied_into_table", target_columns=target_column_names, force=True)
    >>> session.table("copied_into_table").show()
    ---------------------------------
    |"PRODUCT_ID"  |"PRODUCT_NAME"  |
    ---------------------------------
    |id1           | Product A      |
    |id2           | Product B      |
    ---------------------------------
    
[/code]

The arguments of this function match the optional parameters of the [COPY INTO <table>](https://docs.snowflake.com/en/sql-reference/sql/copy-into-table.html#optional-parameters).

Parameters:
    

  * **table_name** – A string or list of strings representing table name. If input is a string, it represents the table name; if input is of type iterable of strings, it represents the fully-qualified object identifier (database name, schema name, and table name).

  * **files** – Specific files to load from the stage location.

  * **pattern** – The regular expression that is used to match file names of the stage location.

  * **validation_mode** – A `str` that instructs the `COPY INTO <table>` command to validate the data files instead of loading them into the specified table. Values can be “RETURN_n_ROWS”, “RETURN_ERRORS”, or “RETURN_ALL_ERRORS”. Refer to the above mentioned `COPY INTO <table>` command optional parameters for more details.

  * **target_columns** – Name of the columns in the table where the data should be saved.

  * **transformations** – A list of column transformations.

  * **format_type_options** – A dict that contains the `formatTypeOptions` of the `COPY INTO <table>` command.

  * **statement_params** – Dictionary of statement level parameters to be set while executing this action.

  * **iceberg_config** – 

A dictionary that can contain the following iceberg configuration values:

    * partition_by: specifies one or more partition expressions for the Iceberg table.
    

Can be a single Column, column name, SQL expression string, or a list of these. Supports identity partitioning (column names) as well as partition transform functions like bucket(), truncate(), year(), month(), day(), hour().

    * external_volume: specifies the identifier for the external volume where
    

the Iceberg table stores its metadata files and data in Parquet format

    * catalog: specifies either Snowflake or a catalog integration to use for this table

    * base_location: the base directory that snowflake can write iceberg metadata and files to

    * target_file_size: specifies a target Parquet file size for the table.
    

Valid values: ‘AUTO’ (default), ‘16MB’, ‘32MB’, ‘64MB’, ‘128MB’

    * catalog_sync: optionally sets the catalog integration configured for Polaris Catalog

    * storage_serialization_policy: specifies the storage serialization policy for the table

    * iceberg_version: Overrides the version of iceberg to use. Defaults to 2 when unset.

  * **copy_options** – The kwargs that is used to specify the `copyOptions` of the `COPY INTO <table>` command.
