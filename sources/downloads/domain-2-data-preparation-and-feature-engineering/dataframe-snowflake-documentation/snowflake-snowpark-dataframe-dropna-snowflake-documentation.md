---
title: "snowflake.snowpark.DataFrame.dropna | Snowflake Documentation"
source: https://docs.snowflake.com/en/developer-guide/snowpark/reference/python/latest/snowpark/api/snowflake.snowpark.DataFrame.dropna.html
cert_domain: domain-2-data-preparation-and-feature-engineering
crawl_depth: 1
crawled: 2026-08-23
---

# snowflake.snowpark.DataFrame.dropna¶

DataFrame.dropna(_how : str = 'any'_, _thresh : Optional[int] = None_, _subset : Optional[Union[str, Iterable[str]]] = None_) → [DataFrame](snowflake.snowpark.DataFrame.html#snowflake.snowpark.DataFrame "snowflake.snowpark.dataframe.DataFrame")[[source]](https://github.com/snowflakedb/snowpark-python/blob/v1.54.0/src/snowflake/snowpark/dataframe_na_functions.py#L88-L270)¶
    

Returns a new DataFrame that excludes all rows containing fewer than a specified number of non-null and non-NaN values in the specified columns.

Parameters:
    

  * **how** – An `str` with value either ‘any’ or ‘all’. If ‘any’, drop a row if it contains any nulls. If ‘all’, drop a row only if all its values are null. The default value is ‘any’. If `thresh` is provided, `how` will be ignored.

  * **thresh** – 

The minimum number of non-null and non-NaN values that should be in the specified columns in order for the row to be included. It overwrites `how`. In each case:

>     * If `thresh` is not provided or `None`, the length of `subset` will be used when `how` is ‘any’ and 1 will be used when `how` is ‘all’.
> 
>     * If `thresh` is greater than the number of the specified columns, the method returns an empty DataFrame.
> 
>     * If `thresh` is less than 1, the method returns the original DataFrame.

  * **subset** – 

A list of the names of columns to check for null and NaN values. In each case:

>     * If `subset` is not provided or `None`, all columns will be included.
> 
>     * If `subset` is empty, the method returns the original DataFrame.




Examples:
[code] 
    >>> df = session.create_dataframe([[1.0, 1], [float('nan'), 2], [None, 3], [4.0, None], [float('nan'), None]]).to_df("a", "b")
    >>> # drop a row if it contains any nulls, with checking all columns
    >>> df.na.drop().show()
    -------------
    |"A"  |"B"  |
    -------------
    |1.0  |1    |
    -------------
    
    >>> # drop a row only if all its values are null, with checking all columns
    >>> df.na.drop(how='all').show()
    ---------------
    |"A"   |"B"   |
    ---------------
    |1.0   |1     |
    |nan   |2     |
    |NULL  |3     |
    |4.0   |NULL  |
    ---------------
    
    >>> # drop a row if it contains at least one non-null and non-NaN values, with checking all columns
    >>> df.na.drop(thresh=1).show()
    ---------------
    |"A"   |"B"   |
    ---------------
    |1.0   |1     |
    |nan   |2     |
    |NULL  |3     |
    |4.0   |NULL  |
    ---------------
    
    >>> # drop a row if it contains any nulls, with checking column "a"
    >>> df.na.drop(subset=["a"]).show()
    --------------
    |"A"  |"B"   |
    --------------
    |1.0  |1     |
    |4.0  |NULL  |
    --------------
    
    >>> df.na.drop(subset="a").show()
    --------------
    |"A"  |"B"   |
    --------------
    |1.0  |1     |
    |4.0  |NULL  |
    --------------
    
[/code]

See also

`DataFrame.dropna()`
