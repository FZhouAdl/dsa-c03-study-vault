---
title: "snowflake.snowpark.DataFrame.approx_quantile | Snowflake Documentation"
source: https://docs.snowflake.com/en/developer-guide/snowpark/reference/python/latest/snowpark/api/snowflake.snowpark.DataFrame.approx_quantile.html
cert_domain: domain-2-data-preparation-and-feature-engineering
crawl_depth: 1
crawled: 2026-08-23
---

# snowflake.snowpark.DataFrame.approx_quantile¶

DataFrame.approx_quantile(_col : Union[[Column](snowflake.snowpark.Column.html#snowflake.snowpark.Column "snowflake.snowpark.column.Column"), str, Iterable[Union[[Column](snowflake.snowpark.Column.html#snowflake.snowpark.Column "snowflake.snowpark.column.Column"), str]]]_, _percentile : Iterable[float]_, _*_ , _statement_params : Optional[Dict[str, str]] = None_) → Union[List[float], List[List[float]]][[source]](https://github.com/snowflakedb/snowpark-python/blob/v1.54.0/src/snowflake/snowpark/dataframe_stat_functions.py#L55-L169)¶
    

For a specified numeric column and a list of desired quantiles, returns an approximate value for the column at each of the desired quantiles. This function uses the t-Digest algorithm.

Examples:
[code] 
    >>> df = session.create_dataframe([1, 2, 3, 4, 5, 6, 7, 8, 9, 0], schema=["a"])
    >>> df.stat.approx_quantile("a", [0, 0.1, 0.4, 0.6, 1])  
    
    >>> df2 = session.create_dataframe([[0.1, 0.5], [0.2, 0.6], [0.3, 0.7]], schema=["a", "b"])
    >>> df2.stat.approx_quantile(["a", "b"], [0, 0.1, 0.6])  
    
[/code]

Parameters:
    

  * **col** – The name of the numeric column.

  * **percentile** – A list of float values greater than or equal to 0.0 and less than 1.0.

  * **statement_params** – Dictionary of statement level parameters to be set while executing this action.



Returns:
    

A list of approximate percentile values if `col` is a single column name, or a matrix with the dimensions `(len(col) * len(percentile)` containing the approximate percentile values if `col` is a list of column names.
