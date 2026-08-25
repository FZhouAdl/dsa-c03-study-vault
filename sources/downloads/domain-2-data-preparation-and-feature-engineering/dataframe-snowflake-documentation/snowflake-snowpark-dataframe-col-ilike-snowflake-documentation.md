---
title: "snowflake.snowpark.DataFrame.col_ilike | Snowflake Documentation"
source: https://docs.snowflake.com/en/developer-guide/snowpark/reference/python/latest/snowpark/api/snowflake.snowpark.DataFrame.col_ilike.html
cert_domain: domain-2-data-preparation-and-feature-engineering
crawl_depth: 1
crawled: 2026-08-23
---

# snowflake.snowpark.DataFrame.col_ilike¶

DataFrame.col_ilike(_pattern : str_) → [DataFrame](snowflake.snowpark.DataFrame.html#snowflake.snowpark.DataFrame "snowflake.snowpark.dataframe.DataFrame")[[source]](https://github.com/snowflakedb/snowpark-python/blob/v1.54.0/src/snowflake/snowpark/dataframe.py#L2031-L2088)¶
    

Returns a new DataFrame with only the columns whose names match the specified pattern using case-insensitive ILIKE matching (similar to SELECT * ILIKE ‘pattern’ in SQL).

Parameters:
    

**pattern** – The ILIKE pattern to match column names against. You can use the following wildcards: \- Use an underscore (_) to match any single character. \- Use a percent sign (%) to match any sequence of zero or more characters. \- To match a sequence anywhere within the column name, begin and end the pattern with %.

Returns:
    

A new DataFrame containing only columns matching the pattern.

Return type:
    

[DataFrame](snowflake.snowpark.DataFrame.html#snowflake.snowpark.DataFrame "snowflake.snowpark.DataFrame")

Raises:
    

  * **ValueError** – If SQL simplifier is not enabled.

  * [**SnowparkSQLException**](snowflake.snowpark.exceptions.SnowparkSQLException.html#snowflake.snowpark.exceptions.SnowparkSQLException "snowflake.snowpark.exceptions.SnowparkSQLException") – If no columns match the specified pattern.




Examples:
[code] 
    >>> # Select all columns containing 'id' (case-insensitive)
    >>> df = session.create_dataframe([[1, "John", 101], [2, "Jane", 102]],
    ...                                 schema=["USER_ID", "Name", "dept_id"])
    >>> df.col_ilike("%id%").show()
    -------------------------
    |"USER_ID"  |"DEPT_ID"  |
    -------------------------
    |1          |101        |
    |2          |102        |
    -------------------------
    
[/code]
