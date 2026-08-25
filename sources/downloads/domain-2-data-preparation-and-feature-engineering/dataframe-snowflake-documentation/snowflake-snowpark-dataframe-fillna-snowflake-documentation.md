---
title: "snowflake.snowpark.DataFrame.fillna | Snowflake Documentation"
source: https://docs.snowflake.com/en/developer-guide/snowpark/reference/python/latest/snowpark/api/snowflake.snowpark.DataFrame.fillna.html
cert_domain: domain-2-data-preparation-and-feature-engineering
crawl_depth: 1
crawled: 2026-08-23
---

# snowflake.snowpark.DataFrame.fillna¶

DataFrame.fillna(_value : Union[None, bool, int, float, str, bytearray, Decimal, date, datetime, time, bytes, NaTType, float64, list, tuple, dict, Dict[str, Union[None, bool, int, float, str, bytearray, Decimal, date, datetime, time, bytes, NaTType, float64, list, tuple, dict]]]_, _subset : Optional[Union[str, Iterable[str]]] = None_, _*_ , _include_decimal : bool = False_) → [DataFrame](snowflake.snowpark.DataFrame.html#snowflake.snowpark.DataFrame "snowflake.snowpark.dataframe.DataFrame")[[source]](https://github.com/snowflakedb/snowpark-python/blob/v1.54.0/src/snowflake/snowpark/dataframe_na_functions.py#L272-L489)¶
    

Returns a new DataFrame that replaces all null and NaN values in the specified columns with the values provided.

Parameters:
    

  * **value** – A scalar value or a `dict` that associates the names of columns with the values that should be used to replace null and NaN values in those columns. If `value` is a `dict`, `subset` is ignored. If `value` is an empty `dict`, the method returns the original DataFrame.

  * **subset** – 

A list of the names of columns to check for null and NaN values. In each case:

>     * If `subset` is not provided or `None`, all columns will be included.
> 
>     * If `subset` is empty, the method returns the original DataFrame.

  * **include_decimal** – Whether to allow `Decimal` values to fill in `IntegerType` and `FloatType` columns.




Examples:
[code] 
    >>> df = session.create_dataframe([[1.0, 1], [float('nan'), 2], [None, 3], [4.0, None], [float('nan'), None]]).to_df("a", "b")
    >>> # fill null and NaN values in all columns
    >>> df.na.fill(3.14).show()
    ---------------
    |"A"   |"B"   |
    ---------------
    |1.0   |1     |
    |3.14  |2     |
    |3.14  |3     |
    |4.0   |NULL  |
    |3.14  |NULL  |
    ---------------
    
    >>> # fill null and NaN values in column "a"
    >>> df.na.fill(3.14, subset="a").show()
    ---------------
    |"A"   |"B"   |
    ---------------
    |1.0   |1     |
    |3.14  |2     |
    |3.14  |3     |
    |4.0   |NULL  |
    |3.14  |NULL  |
    ---------------
    
    >>> # fill null and NaN values in column "a"
    >>> df.na.fill({"a": 3.14}).show()
    ---------------
    |"A"   |"B"   |
    ---------------
    |1.0   |1     |
    |3.14  |2     |
    |3.14  |3     |
    |4.0   |NULL  |
    |3.14  |NULL  |
    ---------------
    
    >>> # fill null and NaN values in column "a" and "b"
    >>> df.na.fill({"a": 3.14, "b": 15}).show()
    --------------
    |"A"   |"B"  |
    --------------
    |1.0   |1    |
    |3.14  |2    |
    |3.14  |3    |
    |4.0   |15   |
    |3.14  |15   |
    --------------
    
    >>> df2 = session.create_dataframe([[1.0, True], [2.0, False], [3.0, False], [None, None]]).to_df("a", "b")
    >>> df2.na.fill(True).show()
    ----------------
    |"A"   |"B"    |
    ----------------
    |1.0   |True   |
    |2.0   |False  |
    |3.0   |False  |
    |NULL  |True   |
    ----------------
    
[/code]

Note

If the type of a given value in `value` doesn’t match the column data type (e.g. a `float` for [`StringType`](snowflake.snowpark.types.StringType.html#snowflake.snowpark.types.StringType "snowflake.snowpark.types.StringType") column), this replacement will be skipped in this column. Especially,

>   * `int` can be filled in a column with [`FloatType`](snowflake.snowpark.types.FloatType.html#snowflake.snowpark.types.FloatType "snowflake.snowpark.types.FloatType") or [`DoubleType`](snowflake.snowpark.types.DoubleType.html#snowflake.snowpark.types.DoubleType "snowflake.snowpark.types.DoubleType"), but `float` cannot filled in a column with [`IntegerType`](snowflake.snowpark.types.IntegerType.html#snowflake.snowpark.types.IntegerType "snowflake.snowpark.types.IntegerType") or [`LongType`](snowflake.snowpark.types.LongType.html#snowflake.snowpark.types.LongType "snowflake.snowpark.types.LongType").
> 
> 


See also

`DataFrame.fillna()`
