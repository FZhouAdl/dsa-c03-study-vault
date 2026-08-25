---
title: "snowflake.snowpark.functions.call_table_function | Snowflake Documentation"
source: https://docs.snowflake.com/en/developer-guide/snowpark/reference/python/latest/snowpark/api/snowflake.snowpark.functions.call_table_function.html
cert_domain: domain-2-data-preparation-and-feature-engineering
crawl_depth: 1
crawled: 2026-08-23
---

# snowflake.snowpark.functions.call_table_function¶

snowflake.snowpark.functions.call_table_function(_function_name : Union[str, Iterable[str]]_, _* args: Union[[Column](snowflake.snowpark.Column.html#snowflake.snowpark.Column "snowflake.snowpark.column.Column"), None, bool, int, float, str, bytearray, Decimal, date, datetime, time, bytes, NaTType, float64, list, tuple, dict]_, _** kwargs: Union[[Column](snowflake.snowpark.Column.html#snowflake.snowpark.Column "snowflake.snowpark.column.Column"), None, bool, int, float, str, bytearray, Decimal, date, datetime, time, bytes, NaTType, float64, list, tuple, dict]_) → [TableFunctionCall](snowflake.snowpark.table_function.TableFunctionCall.html#snowflake.snowpark.table_function.TableFunctionCall "snowflake.snowpark.table_function.TableFunctionCall")[[source]](https://github.com/snowflakedb/snowpark-python/blob/v1.54.0/src/snowflake/snowpark/functions.py#L10575-L10606)¶
    

Invokes a Snowflake table function, including system-defined table functions and user-defined table functions.

It returns a [`TableFunctionCall()`](snowflake.snowpark.table_function.TableFunctionCall.html#snowflake.snowpark.table_function.TableFunctionCall "snowflake.snowpark.table_function.TableFunctionCall") so you can specify the partition clause.

Parameters:
    

  * **function_name** – The name of the table function.

  * **args** – The positional arguments of the table function.

  * ****kwargs** – The named arguments of the table function. Some table functions (e.g., `flatten`) have named arguments instead of positional ones.




Example::
    
[code]
    >>> from snowflake.snowpark.functions import lit
    >>> session.table_function(call_table_function("split_to_table", lit("split words to table"), lit(" ")).over()).collect()
    [Row(SEQ=1, INDEX=1, VALUE='split'), Row(SEQ=1, INDEX=2, VALUE='words'), Row(SEQ=1, INDEX=3, VALUE='to'), Row(SEQ=1, INDEX=4, VALUE='table')]
    
[/code]
