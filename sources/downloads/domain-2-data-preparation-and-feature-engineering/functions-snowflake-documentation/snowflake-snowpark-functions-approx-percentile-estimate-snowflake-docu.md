---
title: "snowflake.snowpark.functions.approx_percentile_estimate | Snowflake Documentation"
source: https://docs.snowflake.com/en/developer-guide/snowpark/reference/python/latest/snowpark/api/snowflake.snowpark.functions.approx_percentile_estimate.html
cert_domain: domain-2-data-preparation-and-feature-engineering
crawl_depth: 1
crawled: 2026-08-23
---

# snowflake.snowpark.functions.approx_percentile_estimate¶

snowflake.snowpark.functions.approx_percentile_estimate(_state : Union[[Column](snowflake.snowpark.Column.html#snowflake.snowpark.Column "snowflake.snowpark.column.Column"), str]_, _percentile : float_) → [Column](snowflake.snowpark.Column.html#snowflake.snowpark.Column "snowflake.snowpark.column.Column")[[source]](https://github.com/snowflakedb/snowpark-python/blob/v1.54.0/src/snowflake/snowpark/functions.py#L1450-L1482)¶
    

Returns the desired approximated percentile value for the specified t-Digest state. APPROX_PERCENTILE_ESTIMATE(APPROX_PERCENTILE_ACCUMULATE(.)) is equivalent to APPROX_PERCENTILE(.).

Example::
    
[code]
    >>> df = session.create_dataframe([1,2,3,4,5], schema=["a"])
    >>> df_accu = df.select(approx_percentile_accumulate("a").alias("app_percentile_accu"))
    >>> df_accu.select(approx_percentile_estimate("app_percentile_accu", 0.5).alias("result")).show()
    ------------
    |"RESULT"  |
    ------------
    |3.0       |
    ------------
    
[/code]
