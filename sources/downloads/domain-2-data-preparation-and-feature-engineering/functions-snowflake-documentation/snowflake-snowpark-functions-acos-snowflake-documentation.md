---
title: "snowflake.snowpark.functions.acos | Snowflake Documentation"
source: https://docs.snowflake.com/en/developer-guide/snowpark/reference/python/latest/snowpark/api/snowflake.snowpark.functions.acos.html
cert_domain: domain-2-data-preparation-and-feature-engineering
crawl_depth: 1
crawled: 2026-08-23
---

# snowflake.snowpark.functions.acos¶

snowflake.snowpark.functions.acos(_e : Union[[Column](snowflake.snowpark.Column.html#snowflake.snowpark.Column "snowflake.snowpark.column.Column"), str]_) → [Column](snowflake.snowpark.Column.html#snowflake.snowpark.Column "snowflake.snowpark.column.Column")[[source]](https://github.com/snowflakedb/snowpark-python/blob/v1.54.0/src/snowflake/snowpark/functions.py#L2263-L2280)¶
    

Computes the inverse cosine (arc cosine) of its input; the result is a number in the interval [-pi, pi].

Example::
    
[code]
    >>> from snowflake.snowpark.types import DecimalType
    >>> df = session.create_dataframe([[0.5]], schema=["deg"])
    >>> df.select(acos(col("deg")).cast(DecimalType(scale=3)).alias("result")).show()
    ------------
    |"RESULT"  |
    ------------
    |1.047     |
    ------------
    
[/code]
