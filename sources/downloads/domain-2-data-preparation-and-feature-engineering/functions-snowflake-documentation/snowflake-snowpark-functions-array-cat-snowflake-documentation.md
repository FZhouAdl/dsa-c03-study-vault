---
title: "snowflake.snowpark.functions.array_cat | Snowflake Documentation"
source: https://docs.snowflake.com/en/developer-guide/snowpark/reference/python/latest/snowpark/api/snowflake.snowpark.functions.array_cat.html
cert_domain: domain-2-data-preparation-and-feature-engineering
crawl_depth: 1
crawled: 2026-08-23
---

# snowflake.snowpark.functions.array_cat¶

snowflake.snowpark.functions.array_cat(_array1 : Union[[Column](snowflake.snowpark.Column.html#snowflake.snowpark.Column "snowflake.snowpark.column.Column"), str]_, _array2 : Union[[Column](snowflake.snowpark.Column.html#snowflake.snowpark.Column "snowflake.snowpark.column.Column"), str]_) → [Column](snowflake.snowpark.Column.html#snowflake.snowpark.Column "snowflake.snowpark.column.Column")[[source]](https://github.com/snowflakedb/snowpark-python/blob/v1.54.0/src/snowflake/snowpark/functions.py#L7135-L7164)¶
    

Returns the concatenation of two ARRAYs.

Parameters:
    

  * **array1** – Column containing the source ARRAY.

  * **array2** – Column containing the ARRAY to be appended to array1.




Example::
    
[code]
    >>> from snowflake.snowpark import Row
    >>> df = session.create_dataframe([Row(a=[1, 2, 3], b=[4, 5])])
    >>> df.select(array_cat("a", "b").alias("result")).show()
    ------------
    |"RESULT"  |
    ------------
    |[         |
    |  1,      |
    |  2,      |
    |  3,      |
    |  4,      |
    |  5       |
    |]         |
    ------------
    
[/code]
