---
title: "snowflake.snowpark.DataFrame.cache_result | Snowflake Documentation"
source: https://docs.snowflake.com/en/developer-guide/snowpark/reference/python/latest/snowpark/api/snowflake.snowpark.DataFrame.cache_result.html
cert_domain: domain-2-data-preparation-and-feature-engineering
crawl_depth: 1
crawled: 2026-08-23
---

# snowflake.snowpark.DataFrame.cache_result¶

DataFrame.cache_result(_*_ , _statement_params : Optional[Dict[str, str]] = None_) → [Table](snowflake.snowpark.Table.html#snowflake.snowpark.Table "snowflake.snowpark.Table")[[source]](https://github.com/snowflakedb/snowpark-python/blob/v1.54.0/src/snowflake/snowpark/dataframe.py#L6410-L6570)¶
    

Caches the content of this DataFrame to create a new cached Table DataFrame.

All subsequent operations on the returned cached DataFrame are performed on the cached data and have no effect on the original DataFrame.

You can use [`Table.drop_table()`](snowflake.snowpark.Table.drop_table.html#snowflake.snowpark.Table.drop_table "snowflake.snowpark.Table.drop_table") or the `with` statement to clean up the cached result when it’s not needed. Refer to the example code below.

Note

An error will be thrown if a cached result is cleaned up and it’s used again, or any other DataFrames derived from the cached result are used again.

Examples::
    
[code]
    >>> create_result = session.sql("create temp table RESULT (NUM int)").collect()
    >>> insert_result = session.sql("insert into RESULT values(1),(2)").collect()
    
[/code]
[code] 
    >>> df = session.table("RESULT")
    >>> df.collect()
    [Row(NUM=1), Row(NUM=2)]
    
[/code]
[code] 
    >>> # Run cache_result and then insert into the original table to see
    >>> # that the cached result is not affected
    >>> df1 = df.cache_result()
    >>> insert_again_result = session.sql("insert into RESULT values (3)").collect()
    >>> df1.collect()
    [Row(NUM=1), Row(NUM=2)]
    >>> df.collect()
    [Row(NUM=1), Row(NUM=2), Row(NUM=3)]
    
[/code]
[code] 
    >>> # You can run cache_result on a result that has already been cached
    >>> df2 = df1.cache_result()
    >>> df2.collect()
    [Row(NUM=1), Row(NUM=2)]
    
[/code]
[code] 
    >>> df3 = df.cache_result()
    >>> # Drop RESULT and see that the cached results still exist
    >>> drop_table_result = session.sql(f"drop table RESULT").collect()
    >>> df1.collect()
    [Row(NUM=1), Row(NUM=2)]
    >>> df2.collect()
    [Row(NUM=1), Row(NUM=2)]
    >>> df3.collect()
    [Row(NUM=1), Row(NUM=2), Row(NUM=3)]
    >>> # Clean up the cached result
    >>> df3.drop_table()
    >>> # use context manager to clean up the cached result after it's use.
    >>> with df2.cache_result() as df4:
    ...     df4.collect()
    [Row(NUM=1), Row(NUM=2)]
    
[/code]

Parameters:
    

**statement_params** – Dictionary of statement level parameters to be set while executing this action.

Returns:
    

A [`Table`](snowflake.snowpark.Table.html#snowflake.snowpark.Table "snowflake.snowpark.Table") object that holds the cached result in a temporary table. All operations on this new DataFrame have no effect on the original.

Note

A temporary table is created to store the cached result and a [`Table`](snowflake.snowpark.Table.html#snowflake.snowpark.Table "snowflake.snowpark.Table") object is returned. You can retrieve the table name by accessing [`Table.table_name`](snowflake.snowpark.Table.table_name.html#snowflake.snowpark.Table.table_name "snowflake.snowpark.Table.table_name"). Note that this temporary Snowflake table

>   * may be automatically removed when the Table object is no longer referenced if `Session.auto_clean_up_temp_table_enabled` is set to `True`.
> 
>   * will be dropped after the session is closed.
> 
> 


To retain a persistent table, consider using [`DataFrameWriter.save_as_table()`](snowflake.snowpark.DataFrameWriter.save_as_table.html#snowflake.snowpark.DataFrameWriter.save_as_table "snowflake.snowpark.DataFrameWriter.save_as_table") to persist the cached result.
