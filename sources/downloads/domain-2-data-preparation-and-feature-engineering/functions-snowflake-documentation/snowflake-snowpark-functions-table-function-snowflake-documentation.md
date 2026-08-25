---
title: "snowflake.snowpark.functions.table_function | Snowflake Documentation"
source: https://docs.snowflake.com/en/developer-guide/snowpark/reference/python/latest/snowpark/api/snowflake.snowpark.functions.table_function.html
cert_domain: domain-2-data-preparation-and-feature-engineering
crawl_depth: 1
crawled: 2026-08-23
---

# snowflake.snowpark.functions.table_function¶

snowflake.snowpark.functions.table_function(_function_name : str_) → Callable[[source]](https://github.com/snowflakedb/snowpark-python/blob/v1.54.0/src/snowflake/snowpark/functions.py#L10609-L10630)¶
    

Create a function object to invoke a Snowflake table function.

Parameters:
    

**function_name** – The name of the table function.

Example::
    
[code]
    >>> from snowflake.snowpark.functions import lit
    >>> split_to_table = table_function("split_to_table")
    >>> session.table_function(split_to_table(lit("split words to table"), lit(" ")).over()).collect()
    [Row(SEQ=1, INDEX=1, VALUE='split'), Row(SEQ=1, INDEX=2, VALUE='words'), Row(SEQ=1, INDEX=3, VALUE='to'), Row(SEQ=1, INDEX=4, VALUE='table')]
    
[/code]
