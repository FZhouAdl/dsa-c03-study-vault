---
title: "snowflake.snowpark.DataFrame.drop | Snowflake Documentation"
source: https://docs.snowflake.com/en/developer-guide/snowpark/reference/python/latest/snowpark/api/snowflake.snowpark.DataFrame.drop.html
cert_domain: domain-2-data-preparation-and-feature-engineering
crawl_depth: 1
crawled: 2026-08-23
---

# snowflake.snowpark.DataFrame.drop¶

DataFrame.drop(_* cols: Union[[Column](snowflake.snowpark.Column.html#snowflake.snowpark.Column "snowflake.snowpark.column.Column"), str, Iterable[Union[[Column](snowflake.snowpark.Column.html#snowflake.snowpark.Column "snowflake.snowpark.column.Column"), str]]]_) → [DataFrame](snowflake.snowpark.DataFrame.html#snowflake.snowpark.DataFrame "snowflake.snowpark.dataframe.DataFrame")[[source]](https://github.com/snowflakedb/snowpark-python/blob/v1.54.0/src/snowflake/snowpark/dataframe.py#L1908-L2029)¶
    

Returns a new DataFrame that excludes the columns with the specified names from the output.

This is functionally equivalent to calling [`select()`](snowflake.snowpark.DataFrame.select.html#snowflake.snowpark.DataFrame.select "snowflake.snowpark.DataFrame.select") and passing in all columns except the ones to exclude. This is a no-op if schema does not contain the given column name(s).

Example:
[code] 
    >>> df = session.create_dataframe([[1, 2, 3]], schema=["a", "b", "c"])
    >>> df.drop("a", "b").show()
    -------
    |"C"  |
    -------
    |3    |
    -------
    
[/code]

Parameters:
    

***cols** – the columns to exclude, as `str`, [`Column`](snowflake.snowpark.Column.html#snowflake.snowpark.Column "snowflake.snowpark.Column") or a list of those.

Raises:
    

[**SnowparkClientException**](snowflake.snowpark.exceptions.SnowparkClientException.html#snowflake.snowpark.exceptions.SnowparkClientException "snowflake.snowpark.exceptions.SnowparkClientException") – if the resulting [`DataFrame`](snowflake.snowpark.DataFrame.html#snowflake.snowpark.DataFrame "snowflake.snowpark.DataFrame") contains no output columns.
