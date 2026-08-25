---
title: "snowflake.snowpark.functions.array_agg | Snowflake Documentation"
source: https://docs.snowflake.com/en/developer-guide/snowpark/reference/python/latest/snowpark/api/snowflake.snowpark.functions.array_agg.html
cert_domain: domain-2-data-preparation-and-feature-engineering
crawl_depth: 1
crawled: 2026-08-23
---

# snowflake.snowpark.functions.array_agg¶

snowflake.snowpark.functions.array_agg(_col : Union[[Column](snowflake.snowpark.Column.html#snowflake.snowpark.Column "snowflake.snowpark.column.Column"), str]_, _is_distinct : bool = False_) → [Column](snowflake.snowpark.Column.html#snowflake.snowpark.Column "snowflake.snowpark.column.Column")[[source]](https://github.com/snowflakedb/snowpark-python/blob/v1.54.0/src/snowflake/snowpark/functions.py#L7001-L7026)¶
    

Returns the input values, pivoted into an ARRAY. If the input is empty, an empty ARRAY is returned.

Example::
    
[code]
    >>> df = session.create_dataframe([[1], [2], [3], [1]], schema=["a"])
    >>> df.select(array_agg("a", True).within_group("a").alias("result")).show()
    ------------
    |"RESULT"  |
    ------------
    |[         |
    |  1,      |
    |  2,      |
    |  3       |
    |]         |
    ------------
    
[/code]
