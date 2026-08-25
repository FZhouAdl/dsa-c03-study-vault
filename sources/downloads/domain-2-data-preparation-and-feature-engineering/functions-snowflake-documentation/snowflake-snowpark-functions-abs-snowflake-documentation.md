---
title: "snowflake.snowpark.functions.abs | Snowflake Documentation"
source: https://docs.snowflake.com/en/developer-guide/snowpark/reference/python/latest/snowpark/api/snowflake.snowpark.functions.abs.html
cert_domain: domain-2-data-preparation-and-feature-engineering
crawl_depth: 1
crawled: 2026-08-23
---

# snowflake.snowpark.functions.abs¶

snowflake.snowpark.functions.abs(_e : Union[[Column](snowflake.snowpark.Column.html#snowflake.snowpark.Column "snowflake.snowpark.column.Column"), str]_) → [Column](snowflake.snowpark.Column.html#snowflake.snowpark.Column "snowflake.snowpark.column.Column")[[source]](https://github.com/snowflakedb/snowpark-python/blob/v1.54.0/src/snowflake/snowpark/functions.py#L2245-L2260)¶
    

Returns the absolute value of a numeric expression.

Example::
    
[code]
    >>> df = session.create_dataframe([[-1]], schema=["a"])
    >>> df.select(abs(col("a")).alias("result")).show()
    ------------
    |"RESULT"  |
    ------------
    |1         |
    ------------
    
[/code]
