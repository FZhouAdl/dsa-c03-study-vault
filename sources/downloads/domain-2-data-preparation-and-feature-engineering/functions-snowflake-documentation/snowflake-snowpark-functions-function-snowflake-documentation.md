---
title: "snowflake.snowpark.functions.function | Snowflake Documentation"
source: https://docs.snowflake.com/en/developer-guide/snowpark/reference/python/latest/snowpark/api/snowflake.snowpark.functions.function.html
cert_domain: domain-2-data-preparation-and-feature-engineering
crawl_depth: 1
crawled: 2026-08-23
---

# snowflake.snowpark.functions.function¶

snowflake.snowpark.functions.function(_function_name : str_) → Callable[[source]](https://github.com/snowflakedb/snowpark-python/blob/v1.54.0/src/snowflake/snowpark/functions.py#L10663-L10693)¶
    

Function object to invoke a Snowflake [system-defined function](https://docs.snowflake.com/en/sql-reference-functions.html) (built-in function). Use this to invoke any built-in functions not explicitly listed in this object.

Parameters:
    

**function_name** – The name of built-in function in Snowflake.

Returns:
    

A `Callable` object for calling a Snowflake system-defined function.

Example::
    
[code]
    >>> df = session.create_dataframe([1, 2, 3, 4], schema=["a"])  # a single column with 4 rows
    >>> df.select(call_function("avg", col("a"))).show()
    ----------------
    |"AVG(""A"")"  |
    ----------------
    |2.500000      |
    ----------------
    
    >>> my_avg = function('avg')
    >>> df.select(my_avg(col("a"))).show()
    ----------------
    |"AVG(""A"")"  |
    ----------------
    |2.500000      |
    ----------------
    
[/code]
