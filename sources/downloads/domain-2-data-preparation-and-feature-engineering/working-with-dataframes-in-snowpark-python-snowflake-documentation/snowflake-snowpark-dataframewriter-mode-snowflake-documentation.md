---
title: "snowflake.snowpark.DataFrameWriter.mode | Snowflake Documentation"
source: https://docs.snowflake.com/developer-guide/snowpark/reference/python/latest/api/snowflake.snowpark.DataFrameWriter.mode
cert_domain: domain-2-data-preparation-and-feature-engineering
crawl_depth: 1
crawled: 2026-08-23
---

# snowflake.snowpark.DataFrameWriter.mode¶

DataFrameWriter.mode(_save_mode : str_) → [DataFrameWriter](snowflake.snowpark.DataFrameWriter.html#snowflake.snowpark.DataFrameWriter "snowflake.snowpark.dataframe_writer.DataFrameWriter")[[source]](https://github.com/snowflakedb/snowpark-python/blob/v1.54.0/src/snowflake/snowpark/dataframe_writer.py#L103-L136)¶
    

Set the save mode of this [`DataFrameWriter`](snowflake.snowpark.DataFrameWriter.html#snowflake.snowpark.DataFrameWriter "snowflake.snowpark.DataFrameWriter").

Parameters:
    

**save_mode** – 

One of the following strings.

”append”: Append data of this DataFrame to the existing table. Creates a table if it does not exist.

”overwrite”: Overwrite the existing table by dropping old table.

”truncate”: Overwrite the existing table by truncating old table.

”errorifexists”: Throw an exception if the table already exists.

”ignore”: Ignore this operation if the table already exists.

Default value is “errorifexists”.

Returns:
    

The [`DataFrameWriter`](snowflake.snowpark.DataFrameWriter.html#snowflake.snowpark.DataFrameWriter "snowflake.snowpark.DataFrameWriter") itself.
