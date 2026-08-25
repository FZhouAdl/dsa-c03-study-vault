---
title: "snowflake.snowpark.functions.array_compact | Snowflake Documentation"
source: https://docs.snowflake.com/en/developer-guide/snowpark/reference/python/latest/snowpark/api/snowflake.snowpark.functions.array_compact.html
cert_domain: domain-2-data-preparation-and-feature-engineering
crawl_depth: 1
crawled: 2026-08-23
---

# snowflake.snowpark.functions.array_compact¶

snowflake.snowpark.functions.array_compact(_array : Union[[Column](snowflake.snowpark.Column.html#snowflake.snowpark.Column "snowflake.snowpark.column.Column"), str]_) → [Column](snowflake.snowpark.Column.html#snowflake.snowpark.Column "snowflake.snowpark.column.Column")[[source]](https://github.com/snowflakedb/snowpark-python/blob/v1.54.0/src/snowflake/snowpark/functions.py#L7167-L7191)¶
    

Returns a compacted ARRAY with missing and null values removed, effectively converting sparse arrays into dense arrays.

Parameters:
    

**array** – Column containing the source ARRAY to be compacted

Example::
    
[code]
    >>> from snowflake.snowpark import Row
    >>> df = session.create_dataframe([Row(a=[1, None, 3])])
    >>> df.select("a", array_compact("a").alias("compacted")).show()
    -------------------------
    |"A"      |"COMPACTED"  |
    -------------------------
    |[        |[            |
    |  1,     |  1,         |
    |  null,  |  3          |
    |  3      |]            |
    |]        |             |
    -------------------------
    
[/code]
