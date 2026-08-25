---
title: "snowflake.snowpark.functions.approx_percentile_accumulate | Snowflake Documentation"
source: https://docs.snowflake.com/en/developer-guide/snowpark/reference/python/latest/snowpark/api/snowflake.snowpark.functions.approx_percentile_accumulate.html
cert_domain: domain-2-data-preparation-and-feature-engineering
crawl_depth: 1
crawled: 2026-08-23
---

# snowflake.snowpark.functions.approx_percentile_accumulate¶

snowflake.snowpark.functions.approx_percentile_accumulate(_col : Union[[Column](snowflake.snowpark.Column.html#snowflake.snowpark.Column "snowflake.snowpark.column.Column"), str]_) → [Column](snowflake.snowpark.Column.html#snowflake.snowpark.Column "snowflake.snowpark.column.Column")[[source]](https://github.com/snowflakedb/snowpark-python/blob/v1.54.0/src/snowflake/snowpark/functions.py#L1416-L1447)¶
    

Returns the internal representation of the t-Digest state (as a JSON object) at the end of aggregation. This function uses the t-Digest algorithm.

Example::
    
[code]
    >>> df = session.create_dataframe([1,2,3,4,5], schema=["a"])
    >>> df.select(approx_percentile_accumulate("a").alias("result")).show()
    ------------------------------
    |"RESULT"                    |
    ------------------------------
    |{                           |
    |  "state": [                |
    |    1.000000000000000e+00,  |
    |    1.000000000000000e+00,  |
    |    2.000000000000000e+00,  |
    |    1.000000000000000e+00,  |
    |    3.000000000000000e+00,  |
    |    1.000000000000000e+00,  |
    |    4.000000000000000e+00,  |
    |    1.000000000000000e+00,  |
    |    5.000000000000000e+00,  |
    |    1.000000000000000e+00   |
    |  ],                        |
    |  "type": "tdigest",        |
    |  "version": 1              |
    |}                           |
    ------------------------------
    
[/code]
