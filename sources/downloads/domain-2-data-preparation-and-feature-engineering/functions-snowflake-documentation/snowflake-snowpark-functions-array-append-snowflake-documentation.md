---
title: "snowflake.snowpark.functions.array_append | Snowflake Documentation"
source: https://docs.snowflake.com/en/developer-guide/snowpark/reference/python/latest/snowpark/api/snowflake.snowpark.functions.array_append.html
cert_domain: domain-2-data-preparation-and-feature-engineering
crawl_depth: 1
crawled: 2026-08-23
---

# snowflake.snowpark.functions.array_append¶

snowflake.snowpark.functions.array_append(_array : Union[[Column](snowflake.snowpark.Column.html#snowflake.snowpark.Column "snowflake.snowpark.column.Column"), str]_, _element : Union[[Column](snowflake.snowpark.Column.html#snowflake.snowpark.Column "snowflake.snowpark.column.Column"), str]_) → [Column](snowflake.snowpark.Column.html#snowflake.snowpark.Column "snowflake.snowpark.column.Column")[[source]](https://github.com/snowflakedb/snowpark-python/blob/v1.54.0/src/snowflake/snowpark/functions.py#L7029-L7060)¶
    

Returns an ARRAY containing all elements from the source ARRAY as well as the new element. The new element is located at end of the ARRAY.

Parameters:
    

  * **array** – The column containing the source ARRAY.

  * **element** – The column containing the element to be appended. The element may be of almost any data type. The data type does not need to match the data type(s) of the existing elements in the ARRAY.




Example::
    
[code]
    >>> from snowflake.snowpark import Row
    >>> df = session.create_dataframe([Row(a=[1, 2, 3])])
    >>> df.select(array_append("a", lit(4)).alias("result")).show()
    ------------
    |"RESULT"  |
    ------------
    |[         |
    |  1,      |
    |  2,      |
    |  3,      |
    |  4       |
    |]         |
    ------------
    
[/code]
