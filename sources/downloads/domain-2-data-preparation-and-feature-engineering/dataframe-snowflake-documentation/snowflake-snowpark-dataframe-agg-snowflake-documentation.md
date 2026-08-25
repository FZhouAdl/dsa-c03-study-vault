---
title: "snowflake.snowpark.DataFrame.agg | Snowflake Documentation"
source: https://docs.snowflake.com/en/developer-guide/snowpark/reference/python/latest/snowpark/api/snowflake.snowpark.DataFrame.agg.html
cert_domain: domain-2-data-preparation-and-feature-engineering
crawl_depth: 1
crawled: 2026-08-23
---

# snowflake.snowpark.DataFrame.agg¶

DataFrame.agg(_* exprs: Union[[Column](snowflake.snowpark.Column.html#snowflake.snowpark.Column "snowflake.snowpark.column.Column"), Tuple[Union[[Column](snowflake.snowpark.Column.html#snowflake.snowpark.Column "snowflake.snowpark.column.Column"), str], str], Dict[str, str]]_) → [DataFrame](snowflake.snowpark.DataFrame.html#snowflake.snowpark.DataFrame "snowflake.snowpark.dataframe.DataFrame")[[source]](https://github.com/snowflakedb/snowpark-python/blob/v1.54.0/src/snowflake/snowpark/dataframe.py#L2438-L2521)¶
    

Aggregate the data in the DataFrame. Use this method if you don’t need to group the data ([`group_by()`](snowflake.snowpark.DataFrame.group_by.html#snowflake.snowpark.DataFrame.group_by "snowflake.snowpark.DataFrame.group_by")).

Parameters:
    

**exprs** – 

A variable length arguments list where every element is

  * A Column object

  * A tuple where the first element is a column object or a column name and the second element is the name of the aggregate function

  * A list of the above




or a `dict` maps column names to aggregate function names.

Examples:
[code] 
    >>> from snowflake.snowpark.functions import col, stddev, stddev_pop
    
    >>> df = session.create_dataframe([[1, 2], [3, 4], [1, 4]], schema=["A", "B"])
    >>> df.agg(stddev(col("a"))).show()
    ----------------------
    |"STDDEV(A)"         |
    ----------------------
    |1.1547003940416753  |
    ----------------------
    
    
    >>> df.agg(stddev(col("a")), stddev_pop(col("a"))).show()
    -------------------------------------------
    |"STDDEV(A)"         |"STDDEV_POP(A)"     |
    -------------------------------------------
    |1.1547003940416753  |0.9428091005076267  |
    -------------------------------------------
    
    
    >>> df.agg(("a", "min"), ("b", "max")).show()
    -----------------------
    |"MIN(A)"  |"MAX(B)"  |
    -----------------------
    |1         |4         |
    -----------------------
    
    
    >>> df.agg({"a": "count", "b": "sum"}).show()
    -------------------------
    |"COUNT(A)"  |"SUM(B)"  |
    -------------------------
    |3           |10        |
    -------------------------
    
[/code]

Note

The name of the aggregate function to compute must be a valid Snowflake [aggregate function](https://docs.snowflake.com/en/sql-reference/functions-aggregation.html).

See also

  * [`RelationalGroupedDataFrame.agg()`](snowflake.snowpark.RelationalGroupedDataFrame.agg.html#snowflake.snowpark.RelationalGroupedDataFrame.agg "snowflake.snowpark.RelationalGroupedDataFrame.agg")

  * [`DataFrame.group_by()`](snowflake.snowpark.DataFrame.group_by.html#snowflake.snowpark.DataFrame.group_by "snowflake.snowpark.DataFrame.group_by")
