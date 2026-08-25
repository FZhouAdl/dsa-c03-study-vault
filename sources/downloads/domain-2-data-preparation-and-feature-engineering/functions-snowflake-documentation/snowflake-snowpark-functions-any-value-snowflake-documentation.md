---
title: "snowflake.snowpark.functions.any_value | Snowflake Documentation"
source: https://docs.snowflake.com/en/developer-guide/snowpark/reference/python/latest/snowpark/api/snowflake.snowpark.functions.any_value.html
cert_domain: domain-2-data-preparation-and-feature-engineering
crawl_depth: 1
crawled: 2026-08-23
---

# snowflake.snowpark.functions.any_value¶

snowflake.snowpark.functions.any_value(_e : Union[[Column](snowflake.snowpark.Column.html#snowflake.snowpark.Column "snowflake.snowpark.column.Column"), str]_) → [Column](snowflake.snowpark.Column.html#snowflake.snowpark.Column "snowflake.snowpark.column.Column")[[source]](https://github.com/snowflakedb/snowpark-python/blob/v1.54.0/src/snowflake/snowpark/functions.py#L639-L650)¶
    

Returns a non-deterministic any value for the specified column. This is an aggregate and window function.

Example
[code] 
    >>> df = session.create_dataframe([[1, 2], [3, 4]], schema=["a", "b"])
    >>> result = df.select(any_value("a")).collect()
    >>> assert len(result) == 1  # non-deterministic value in result.
    
[/code]
