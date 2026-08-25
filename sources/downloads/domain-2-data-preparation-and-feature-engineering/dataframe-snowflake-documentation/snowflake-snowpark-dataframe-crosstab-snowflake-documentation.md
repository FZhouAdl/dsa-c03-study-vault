---
title: "snowflake.snowpark.DataFrame.crosstab | Snowflake Documentation"
source: https://docs.snowflake.com/en/developer-guide/snowpark/reference/python/latest/snowpark/api/snowflake.snowpark.DataFrame.crosstab.html
cert_domain: domain-2-data-preparation-and-feature-engineering
crawl_depth: 1
crawled: 2026-08-23
---

# snowflake.snowpark.DataFrame.crosstab¶

DataFrame.crosstab(_col1 : Union[[Column](snowflake.snowpark.Column.html#snowflake.snowpark.Column "snowflake.snowpark.column.Column"), str]_, _col2 : Union[[Column](snowflake.snowpark.Column.html#snowflake.snowpark.Column "snowflake.snowpark.column.Column"), str]_, _*_ , _statement_params : Optional[Dict[str, str]] = None_) → [DataFrame](snowflake.snowpark.DataFrame.html#snowflake.snowpark.DataFrame "snowflake.snowpark.dataframe.DataFrame")[[source]](https://github.com/snowflakedb/snowpark-python/blob/v1.54.0/src/snowflake/snowpark/dataframe_stat_functions.py#L286-L370)¶
    

Computes a pair-wise frequency table (a `contingency table`) for the specified columns. The method returns a DataFrame containing this table.

In the returned contingency table:
    

  * The first column of each row contains the distinct values of `col1`.

  * The name of the first column is the name of `col1`.

  * The rest of the column names are the distinct values of `col2`.

  * For pairs that have no occurrences, the contingency table contains 0 as the count.




Note

The number of distinct values in `col2` should not exceed 1000.

Example:
[code] 
    >>> df = session.create_dataframe([(1, 1), (1, 2), (2, 1), (2, 1), (2, 3), (3, 2), (3, 3)], schema=["key", "value"])
    >>> ct = df.stat.crosstab("key", "value").sort(df["key"])
    >>> ct.show()  
    ---------------------------------------------------------------------------------------------
    |"KEY"  |"CAST(1 AS NUMBER(38,0))"  |"CAST(2 AS NUMBER(38,0))"  |"CAST(3 AS NUMBER(38,0))"  |
    ---------------------------------------------------------------------------------------------
    |1      |1                          |1                          |0                          |
    |2      |2                          |0                          |1                          |
    |3      |0                          |1                          |1                          |
    ---------------------------------------------------------------------------------------------
    
[/code]

Parameters:
    

  * **col1** – The name of the first column to use.

  * **col2** – The name of the second column to use.

  * **statement_params** – Dictionary of statement level parameters to be set while executing this action.
