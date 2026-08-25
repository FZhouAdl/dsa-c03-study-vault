---
title: "snowflake.snowpark.functions.approx_percentile_combine | Snowflake Documentation"
source: https://docs.snowflake.com/en/developer-guide/snowpark/reference/python/latest/snowpark/api/snowflake.snowpark.functions.approx_percentile_combine.html
cert_domain: domain-2-data-preparation-and-feature-engineering
crawl_depth: 1
crawled: 2026-08-23
---

# snowflake.snowpark.functions.approx_percentile_combine¶

snowflake.snowpark.functions.approx_percentile_combine(_state : Union[[Column](snowflake.snowpark.Column.html#snowflake.snowpark.Column "snowflake.snowpark.column.Column"), str]_) → [Column](snowflake.snowpark.Column.html#snowflake.snowpark.Column "snowflake.snowpark.column.Column")[[source]](https://github.com/snowflakedb/snowpark-python/blob/v1.54.0/src/snowflake/snowpark/functions.py#L1485-L1532)¶
    

Combines (merges) percentile input states into a single output state. This allows scenarios where APPROX_PERCENTILE_ACCUMULATE is run over horizontal partitions of the same table, producing an algorithm state for each table partition. These states can later be combined using APPROX_PERCENTILE_COMBINE, producing the same output state as a single run of APPROX_PERCENTILE_ACCUMULATE over the entire table.

Example::
    
[code]
    >>> df1 = session.create_dataframe([1,2,3,4,5], schema=["a"])
    >>> df2 = session.create_dataframe([6,7,8,9,10], schema=["b"])
    >>> df_accu1 = df1.select(approx_percentile_accumulate("a").alias("app_percentile_accu"))
    >>> df_accu2 = df2.select(approx_percentile_accumulate("b").alias("app_percentile_accu"))
    >>> df_accu1.union(df_accu2).select(approx_percentile_combine("app_percentile_accu").alias("result")).show()
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
    |    1.000000000000000e+00,  |
    |    6.000000000000000e+00,  |
    |    1.000000000000000e+00,  |
    |    7.000000000000000e+00,  |
    |    1.000000000000000e+00,  |
    |    8.000000000000000e+00,  |
    |    1.000000000000000e+00,  |
    |    9.000000000000000e+00,  |
    |    1.000000000000000e+00,  |
    |    1.000000000000000e+01,  |
    |    1.000000000000000e+00   |
    |  ],                        |
    |  "type": "tdigest",        |
    |  "version": 1              |
    |}                           |
    ------------------------------
    
[/code]
