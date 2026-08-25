---
title: "snowflake.snowpark.DataFrame.except_ | Snowflake Documentation"
source: https://docs.snowflake.com/en/developer-guide/snowpark/reference/python/latest/snowpark/api/snowflake.snowpark.DataFrame.except_.html
cert_domain: domain-2-data-preparation-and-feature-engineering
crawl_depth: 1
crawled: 2026-08-23
---

# snowflake.snowpark.DataFrame.except_¶

DataFrame.except_(_other : [DataFrame](snowflake.snowpark.DataFrame.html#snowflake.snowpark.DataFrame "snowflake.snowpark.dataframe.DataFrame")_) → [DataFrame](snowflake.snowpark.DataFrame.html#snowflake.snowpark.DataFrame "snowflake.snowpark.dataframe.DataFrame")[[source]](https://github.com/snowflakedb/snowpark-python/blob/v1.54.0/src/snowflake/snowpark/dataframe.py#L3451-L3498)¶
    

Returns a new DataFrame that contains all the rows from the current DataFrame except for the rows that also appear in the `other` DataFrame. Duplicate rows are eliminated.

Example:
[code] 
    >>> df1 = session.create_dataframe([[1, 2], [3, 4]], schema=["a", "b"])
    >>> df2 = session.create_dataframe([[1, 2], [5, 6]], schema=["c", "d"])
    >>> df1.subtract(df2).show()
    -------------
    |"A"  |"B"  |
    -------------
    |3    |4    |
    -------------
    
[/code]

[`minus()`](snowflake.snowpark.DataFrame.minus.html#snowflake.snowpark.DataFrame.minus "snowflake.snowpark.DataFrame.minus") and [`subtract()`](snowflake.snowpark.DataFrame.subtract.html#snowflake.snowpark.DataFrame.subtract "snowflake.snowpark.DataFrame.subtract") are aliases of `except_()`.

Parameters:
    

**other** – The [`DataFrame`](snowflake.snowpark.DataFrame.html#snowflake.snowpark.DataFrame "snowflake.snowpark.DataFrame") that contains the rows to exclude.
