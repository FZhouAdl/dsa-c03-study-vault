---
title: "snowflake.snowpark.DataFrame.to_snowpark_pandas | Snowflake Documentation"
source: https://docs.snowflake.com/developer-guide/snowpark/reference/python/latest/snowpark/api/snowflake.snowpark.DataFrame.to_snowpark_pandas
cert_domain: domain-2-data-preparation-and-feature-engineering
crawl_depth: 1
crawled: 2026-08-23
---

# snowflake.snowpark.DataFrame.to_snowpark_pandas¶

DataFrame.to_snowpark_pandas(_index_col : Optional[Union[str, List[str]]] = None_, _columns : Optional[List[str]] = None_, _enforce_ordering : bool = False_) → [modin.pandas.DataFrame](../../modin/pandas_api/modin.pandas.DataFrame.html#modin.pandas.DataFrame "modin.pandas.DataFrame")[[source]](https://github.com/snowflakedb/snowpark-python/blob/v1.54.0/src/snowflake/snowpark/dataframe.py#L1427-L1572)¶
    

Convert the Snowpark DataFrame to Snowpark pandas DataFrame.

Parameters:
    

  * **index_col** – A column name or a list of column names to use as index.

  * **columns** – A list of column names for the columns to select from the Snowpark DataFrame. If not specified, select all columns except ones configured in index_col.

  * **enforce_ordering** – If False, Snowpark pandas will provide relaxed consistency and ordering guarantees for the returned DataFrame object. Otherwise, strict consistency and ordering guarantees are provided. Please refer to the documentation of [`read_snowflake()`](../../modin/pandas_api/modin.pandas.read_snowflake.html#modin.pandas.read_snowflake "modin.pandas.read_snowflake") for more details. If DDL or DML queries have been used in this query this parameter is ignored and ordering is enforced.



Returns:
    

[`DataFrame`](../../modin/pandas_api/modin.pandas.DataFrame.html#modin.pandas.DataFrame "modin.pandas.DataFrame")
    

A Snowpark pandas DataFrame contains index and data columns based on the snapshot of the current Snowpark DataFrame, which triggers an eager evaluation.

If index_col is provided, the specified index_col is selected as the index column(s) for the result dataframe, otherwise, a default range index from 0 to n - 1 is created as the index column, where n is the number of rows. Please note that is also used as the start row ordering for the dataframe, but there is no guarantee that the default row ordering is the same for two Snowpark pandas dataframe created from the same Snowpark Dataframe.

If columns are provided, the specified columns are selected as the data column(s) for the result dataframe, otherwise, all Snowpark DataFrame columns (exclude index_col) are selected as data columns.

Note

Transformations performed on the returned Snowpark pandas Dataframe do not affect the Snowpark DataFrame from which it was created. Call \- [`modin.pandas.to_snowpark`](../../modin/pandas_api/modin.pandas.to_snowpark.html#modin.pandas.to_snowpark "modin.pandas.to_snowpark") to transform a Snowpark pandas DataFrame back to a Snowpark DataFrame.

The column names used for columns or index_cols must be Normalized Snowflake Identifiers, and the Normalized Snowflake Identifiers of a Snowpark DataFrame can be displayed by calling df.show(). For details about Normalized Snowflake Identifiers, please refer to the Note in [`read_snowflake()`](../../modin/pandas_api/modin.pandas.read_snowflake.html#modin.pandas.read_snowflake "modin.pandas.read_snowflake")

to_snowpark_pandas works only when the environment is set up correctly for Snowpark pandas. This environment may require version of Python and pandas different from what Snowpark Python uses If the environment is setup incorrectly, an error will be raised when to_snowpark_pandas is called.

For Python version support information, please refer to: \- the prerequisites section <https://docs.snowflake.com/en/developer-guide/snowpark/python/snowpark-pandas#prerequisites> \- the installation section <https://docs.snowflake.com/en/developer-guide/snowpark/python/snowpark-pandas#installing-the-snowpark-pandas-api>

See also

  * [`modin.pandas.to_snowpark`](../../modin/pandas_api/modin.pandas.to_snowpark.html#modin.pandas.to_snowpark "modin.pandas.to_snowpark")

  * [`modin.pandas.DataFrame.to_snowpark`](../../modin/pandas_api/modin.pandas.DataFrame.to_snowpark.html#modin.pandas.DataFrame.to_snowpark "modin.pandas.DataFrame.to_snowpark")

  * [`modin.pandas.Series.to_snowpark`](../../modin/pandas_api/modin.pandas.Series.to_snowpark.html#modin.pandas.Series.to_snowpark "modin.pandas.Series.to_snowpark")




Example::
    
[code]
    >>> df = session.create_dataframe([[1, 2, 3]], schema=["a", "b", "c"])
    >>> snowpark_pandas_df = df.to_snowpark_pandas()  
    >>> snowpark_pandas_df      
       A  B  C
    0  1  2  3
    
[/code]
[code] 
    >>> snowpark_pandas_df = df.to_snowpark_pandas(index_col='A')  
    >>> snowpark_pandas_df      
       B  C
    A
    1  2  3
    >>> snowpark_pandas_df = df.to_snowpark_pandas(index_col='A', columns=['B'])  
    >>> snowpark_pandas_df      
       B
    A
    1  2
    >>> snowpark_pandas_df = df.to_snowpark_pandas(index_col=['B', 'A'], columns=['A', 'C', 'A'])  
    >>> snowpark_pandas_df      
         A  C  A
    B A
    2 1  1  3  1
    
[/code]
