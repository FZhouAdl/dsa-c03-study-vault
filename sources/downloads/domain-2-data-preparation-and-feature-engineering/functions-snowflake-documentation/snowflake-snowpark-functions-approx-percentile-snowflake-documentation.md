---
title: "snowflake.snowpark.functions.approx_percentile | Snowflake Documentation"
source: https://docs.snowflake.com/en/developer-guide/snowpark/reference/python/latest/snowpark/api/snowflake.snowpark.functions.approx_percentile.html
cert_domain: domain-2-data-preparation-and-feature-engineering
crawl_depth: 1
crawled: 2026-08-23
---

# snowflake.snowpark.functions.approx_percentile¶

snowflake.snowpark.functions.approx_percentile(_col : Union[[Column](snowflake.snowpark.Column.html#snowflake.snowpark.Column "snowflake.snowpark.column.Column"), str]_, _percentile : float_) → [Column](snowflake.snowpark.Column.html#snowflake.snowpark.Column "snowflake.snowpark.column.Column")[[source]](https://github.com/snowflakedb/snowpark-python/blob/v1.54.0/src/snowflake/snowpark/functions.py#L1383-L1410)¶
    

Returns an approximated value for the desired percentile. This function uses the t-Digest algorithm.

Example::
    
[code]
    >>> df = session.create_dataframe([0,1,2,3,4,5,6,7,8,9], schema=["a"])
    >>> df.select(approx_percentile("a", 0.5).alias("result")).show()
    ------------
    |"RESULT"  |
    ------------
    |4.5       |
    ------------
    
[/code]
