---
title: "snowflake.snowpark.functions.lead | Snowflake Documentation"
source: https://docs.snowflake.com/en/developer-guide/snowpark/reference/python/latest/snowpark/api/snowflake.snowpark.functions.lead.html
cert_domain: domain-2-data-preparation-and-feature-engineering
crawl_depth: 1
crawled: 2026-08-23
---

# snowflake.snowpark.functions.lead¶

snowflake.snowpark.functions.lead(_e : Union[[Column](snowflake.snowpark.Column.html#snowflake.snowpark.Column "snowflake.snowpark.column.Column"), str]_, _offset : int = 1_, _default_value : Union[[Column](snowflake.snowpark.Column.html#snowflake.snowpark.Column "snowflake.snowpark.column.Column"), None, bool, int, float, str, bytearray, Decimal, date, datetime, time, bytes, NaTType, float64, list, tuple, dict] = None_, _ignore_nulls : bool = False_) → [Column](snowflake.snowpark.Column.html#snowflake.snowpark.Column "snowflake.snowpark.column.Column")[[source]](https://github.com/snowflakedb/snowpark-python/blob/v1.54.0/src/snowflake/snowpark/functions.py#L9117-L9159)¶
    

Accesses data in a subsequent row in the same result set without having to join the table to itself.

Example:
[code] 
    >>> from snowflake.snowpark.window import Window
    >>> df = session.create_dataframe(
    ...     [
    ...         [1, 2, 1],
    ...         [1, 2, 3],
    ...         [2, 1, 10],
    ...         [2, 2, 1],
    ...         [2, 2, 3],
    ...     ],
    ...     schema=["x", "y", "z"]
    ... )
    >>> df.select(lead("Z").over(Window.partition_by(col("X")).order_by(col("Y"))).alias("result")).sort("result").collect()
    [Row(RESULT=None), Row(RESULT=None), Row(RESULT=1), Row(RESULT=3), Row(RESULT=3)]
    
[/code]
