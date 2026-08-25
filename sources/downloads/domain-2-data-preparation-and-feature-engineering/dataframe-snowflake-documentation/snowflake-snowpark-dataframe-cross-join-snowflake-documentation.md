---
title: "snowflake.snowpark.DataFrame.cross_join | Snowflake Documentation"
source: https://docs.snowflake.com/en/developer-guide/snowpark/reference/python/latest/snowpark/api/snowflake.snowpark.DataFrame.cross_join.html
cert_domain: domain-2-data-preparation-and-feature-engineering
crawl_depth: 1
crawled: 2026-08-23
---

# snowflake.snowpark.DataFrame.cross_join¶

DataFrame.cross_join(_right : [DataFrame](snowflake.snowpark.DataFrame.html#snowflake.snowpark.DataFrame "snowflake.snowpark.dataframe.DataFrame")_, _*_ , _lsuffix : str = ''_, _rsuffix : str = ''_, _directed : bool = False_) → [DataFrame](snowflake.snowpark.DataFrame.html#snowflake.snowpark.DataFrame "snowflake.snowpark.dataframe.DataFrame")[[source]](https://github.com/snowflakedb/snowpark-python/blob/v1.54.0/src/snowflake/snowpark/dataframe.py#L4233-L4310)¶
    

Performs a cross join, which returns the Cartesian product of the current [`DataFrame`](snowflake.snowpark.DataFrame.html#snowflake.snowpark.DataFrame "snowflake.snowpark.DataFrame") and another [`DataFrame`](snowflake.snowpark.DataFrame.html#snowflake.snowpark.DataFrame "snowflake.snowpark.DataFrame") (`right`).

If the current and `right` DataFrames have columns with the same name, and you need to refer to one of these columns in the returned DataFrame, use the [`col()`](snowflake.snowpark.DataFrame.col.html#snowflake.snowpark.DataFrame.col "snowflake.snowpark.DataFrame.col") function on the current or `right` DataFrame to disambiguate references to these columns.

Example:
[code] 
    >>> df1 = session.create_dataframe([[1, 2], [3, 4]], schema=["a", "b"])
    >>> df2 = session.create_dataframe([[5, 6], [7, 8]], schema=["c", "d"])
    >>> df1.cross_join(df2).sort("a", "b", "c", "d").show()
    -------------------------
    |"A"  |"B"  |"C"  |"D"  |
    -------------------------
    |1    |2    |5    |6    |
    |1    |2    |7    |8    |
    |3    |4    |5    |6    |
    |3    |4    |7    |8    |
    -------------------------
    
    >>> df3 = session.create_dataframe([[1, 2], [3, 4]], schema=["a", "b"])
    >>> df4 = session.create_dataframe([[5, 6], [7, 8]], schema=["a", "b"])
    >>> df3.cross_join(df4, lsuffix="_l", rsuffix="_r").sort("a_l", "b_l", "a_r", "b_r").show()
    ---------------------------------
    |"A_L"  |"B_L"  |"A_R"  |"B_R"  |
    ---------------------------------
    |1      |2      |5      |6      |
    |1      |2      |7      |8      |
    |3      |4      |5      |6      |
    |3      |4      |7      |8      |
    ---------------------------------
    
[/code]

Parameters:
    

  * **right** – the right [`DataFrame`](snowflake.snowpark.DataFrame.html#snowflake.snowpark.DataFrame "snowflake.snowpark.DataFrame") to join.

  * **lsuffix** – Suffix to add to the overlapping columns of the left DataFrame.

  * **rsuffix** – Suffix to add to the overlapping columns of the right DataFrame.

  * **directed** – Whether the join is a directed join, which forces the left argument to be scanned before the right.




Note

If both `lsuffix` and `rsuffix` are empty, the overlapping columns will have random column names in the result DataFrame. If either one is not empty, the overlapping columns won’t have random names.
