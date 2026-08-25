---
title: "snowflake.snowpark.functions.array_construct_compact | Snowflake Documentation"
source: https://docs.snowflake.com/en/developer-guide/snowpark/reference/python/latest/snowpark/api/snowflake.snowpark.functions.array_construct_compact.html
cert_domain: domain-2-data-preparation-and-feature-engineering
crawl_depth: 1
crawled: 2026-08-23
---

# snowflake.snowpark.functions.array_construct_compact¶

snowflake.snowpark.functions.array_construct_compact(_* cols: Union[[Column](snowflake.snowpark.Column.html#snowflake.snowpark.Column "snowflake.snowpark.column.Column"), str]_) → [Column](snowflake.snowpark.Column.html#snowflake.snowpark.Column "snowflake.snowpark.column.Column")[[source]](https://github.com/snowflakedb/snowpark-python/blob/v1.54.0/src/snowflake/snowpark/functions.py#L7223-L7250)¶
    

Returns an ARRAY constructed from zero, one, or more inputs. The constructed ARRAY omits any NULL input values.

Parameters:
    

**cols** – Columns containing the values (or expressions that evaluate to values). The values do not all need to be of the same data type.

Example::
    
[code]
    >>> df = session.create_dataframe([[1, None, 2], [3, None, 4]], schema=["a", "b", "c"])
    >>> df.select(array_construct_compact("a", "b", "c").alias("result")).show()
    ------------
    |"RESULT"  |
    ------------
    |[         |
    |  1,      |
    |  2       |
    |]         |
    |[         |
    |  3,      |
    |  4       |
    |]         |
    ------------
    
[/code]
