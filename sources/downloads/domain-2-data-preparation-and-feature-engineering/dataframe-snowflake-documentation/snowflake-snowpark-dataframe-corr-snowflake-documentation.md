---
title: "snowflake.snowpark.DataFrame.corr | Snowflake Documentation"
source: https://docs.snowflake.com/en/developer-guide/snowpark/reference/python/latest/snowpark/api/snowflake.snowpark.DataFrame.corr.html
cert_domain: domain-2-data-preparation-and-feature-engineering
crawl_depth: 1
crawled: 2026-08-23
---

# snowflake.snowpark.DataFrame.corr¶

DataFrame.corr(_col1 : Union[[Column](snowflake.snowpark.Column.html#snowflake.snowpark.Column "snowflake.snowpark.column.Column"), str]_, _col2 : Union[[Column](snowflake.snowpark.Column.html#snowflake.snowpark.Column "snowflake.snowpark.column.Column"), str]_, _*_ , _statement_params : Optional[Dict[str, str]] = None_) → Optional[float][[source]](https://github.com/snowflakedb/snowpark-python/blob/v1.54.0/src/snowflake/snowpark/dataframe_stat_functions.py#L171-L227)¶
    

Calculates the correlation coefficient for non-null pairs in two numeric columns.

Example:
[code] 
    >>> df = session.create_dataframe([[0.1, 0.5], [0.2, 0.6], [0.3, 0.7]], schema=["a", "b"])
    >>> df.stat.corr("a", "b")
    0.9999999999999991
    
[/code]

Parameters:
    

  * **col1** – The name of the first numeric column to use.

  * **col2** – The name of the second numeric column to use.

  * **statement_params** – Dictionary of statement level parameters to be set while executing this action.



Returns:
    

The correlation of the two numeric columns. If there is not enough data to generate the correlation, the method returns `None`. statement_params: Dictionary of statement level parameters to be set while executing this action.
