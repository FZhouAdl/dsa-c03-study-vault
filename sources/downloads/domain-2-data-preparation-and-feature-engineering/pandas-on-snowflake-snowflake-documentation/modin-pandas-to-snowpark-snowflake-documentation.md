---
title: "modin.pandas.to_snowpark | Snowflake Documentation"
source: https://docs.snowflake.com/developer-guide/snowpark/reference/python/latest/modin/pandas_api/modin.pandas.to_snowpark
cert_domain: domain-2-data-preparation-and-feature-engineering
crawl_depth: 1
crawled: 2026-08-23
---

# modin.pandas.to_snowpark¶

modin.pandas.to_snowpark(_obj : Union[[DataFrame](modin.pandas.DataFrame.html#modin.pandas.DataFrame "modin.pandas.dataframe.DataFrame"), [Series](modin.pandas.Series.html#modin.pandas.Series "modin.pandas.series.Series")]_, _index : bool = True_, _index_label : Optional[Union[Hashable, Sequence[Hashable]]] = None_) → [DataFrame](../../snowpark/api/snowflake.snowpark.DataFrame.html#snowflake.snowpark.DataFrame "snowflake.snowpark.dataframe.DataFrame")[[source]](https://github.com/snowflakedb/snowpark-python/blob/v1.54.0/src/snowflake/snowpark/modin/plugin/extensions/pd_extensions.py#L531-L677)¶
    

Convert the Snowpark pandas DataFrame or Series to a Snowpark DataFrame. Note that once converted to a Snowpark DataFrame, no ordering information will be preserved. You can call reset_index to generate a default index column that is the same as the row position before the call to_snowpark.

Parameters:
    

  * **obj** – The object to be converted to Snowpark DataFrame. It must be either a Snowpark pandas DataFrame or Series

  * **index** – bool, default True. Whether to keep the index columns in the result Snowpark DataFrame. If True, the index columns will be the first set of columns. Otherwise, no index column will be included in the final Snowpark DataFrame.

  * **index_label** – IndexLabel, default None. Column label(s) to use for the index column(s). If None is given (default) and index is True, then the original index column labels are used. A sequence should be given if the DataFrame uses MultiIndex, and the length of the given sequence should be the same as the number of index columns.



Returns:
    

[`DataFrame`](../../snowpark/api/snowflake.snowpark.DataFrame.html#snowflake.snowpark.DataFrame "snowflake.snowpark.dataframe.DataFrame")
    

A Snowpark DataFrame contains the index columns if index=True and all data columns of the Snowpark pandas DataFrame. The identifier for the Snowpark DataFrame will be the normalized quoted identifier with the same name as the pandas label.

Raises:
    

  * **ValueError if duplicated labels occur among the index and data columns.** – 

  * **ValueError if the label used for a index****or****data column is None.** – 




See also

  * [`Snowpark.DataFrame.to_snowpark_pandas`](../../snowpark/api/snowflake.snowpark.DataFrame.to_snowpark_pandas.html#snowflake.snowpark.DataFrame.to_snowpark_pandas "snowflake.snowpark.DataFrame.to_snowpark_pandas")

  * [`DataFrame.to_snowpark`](modin.pandas.DataFrame.to_snowpark.html#modin.pandas.DataFrame.to_snowpark "modin.pandas.DataFrame.to_snowpark")

  * [`Series.to_snowpark`](modin.pandas.Series.to_snowpark.html#modin.pandas.Series.to_snowpark "modin.pandas.Series.to_snowpark")




Note

The labels of the Snowpark pandas DataFrame or index_label provided will be used as Normalized Snowflake Identifiers of the Snowpark DataFrame. For details about Normalized Snowflake Identifiers, please refer to the Note in [`read_snowflake()`](modin.pandas.read_snowflake.html#modin.pandas.read_snowflake "modin.pandas.read_snowflake")

Examples:
[code] 
    >>> df = pd.DataFrame({'Animal': ['Falcon', 'Falcon',
    ...                               'Parrot', 'Parrot'],
    ...                    'Max Speed': [380., 370., 24., 26.]})
    >>> df
       Animal  Max Speed
    0  Falcon      380.0
    1  Falcon      370.0
    2  Parrot       24.0
    3  Parrot       26.0
    >>> snowpark_df = pd.to_snowpark(df, index_label='Order')
    >>> snowpark_df.order_by('"Max Speed"').show()
    ------------------------------------
    |"Order"  |"Animal"  |"Max Speed"  |
    ------------------------------------
    |2        |Parrot    |24.0         |
    |3        |Parrot    |26.0         |
    |1        |Falcon    |370.0        |
    |0        |Falcon    |380.0        |
    ------------------------------------
    
    >>> snowpark_df = pd.to_snowpark(df, index=False)
    >>> snowpark_df.order_by('"Max Speed"').show()
    --------------------------
    |"Animal"  |"Max Speed"  |
    --------------------------
    |Parrot    |24.0         |
    |Parrot    |26.0         |
    |Falcon    |370.0        |
    |Falcon    |380.0        |
    --------------------------
    
    >>> df = pd.DataFrame({'Animal': ['Falcon', 'Falcon',
    ...                               'Parrot', 'Parrot'],
    ...                    'Max Speed': [380., 370., 24., 26.]}, index=pd.Index([3, 5, 6, 7], name="id"))
    >>> df      
        Animal  Max Speed
    id
    3  Falcon      380.0
    5  Falcon      370.0
    6  Parrot       24.0
    7  Parrot       26.0
    >>> snowpark_df = pd.to_snowpark(df)
    >>> snowpark_df.order_by('"id"').show()
    ---------------------------------
    |"id"  |"Animal"  |"Max Speed"  |
    ---------------------------------
    |3     |Falcon    |380.0        |
    |5     |Falcon    |370.0        |
    |6     |Parrot    |24.0         |
    |7     |Parrot    |26.0         |
    ---------------------------------
    
    
    MultiIndex usage
    
    >>> df = pd.DataFrame({'Animal': ['Falcon', 'Falcon',
    ...                               'Parrot', 'Parrot'],
    ...                    'Max Speed': [380., 370., 24., 26.]},
    ...                    index=pd.MultiIndex.from_tuples([('bar', 'one'), ('foo', 'one'), ('bar', 'two'), ('foo', 'three')], names=['first', 'second']))
    >>> df      
                    Animal  Max Speed
    first second
    bar   one     Falcon      380.0
    foo   one     Falcon      370.0
    bar   two     Parrot       24.0
    foo   three   Parrot       26.0
    >>> snowpark_df = pd.to_snowpark(df, index=True, index_label=['A', 'B'])
    >>> snowpark_df.order_by('"A"', '"B"').show()
    ----------------------------------------
    |"A"  |"B"    |"Animal"  |"Max Speed"  |
    ----------------------------------------
    |bar  |one    |Falcon    |380.0        |
    |bar  |two    |Parrot    |24.0         |
    |foo  |one    |Falcon    |370.0        |
    |foo  |three  |Parrot    |26.0         |
    ----------------------------------------
    
    >>> snowpark_df = pd.to_snowpark(df, index=False)
    >>> snowpark_df.order_by('"Max Speed"').show()
    --------------------------
    |"Animal"  |"Max Speed"  |
    --------------------------
    |Parrot    |24.0         |
    |Parrot    |26.0         |
    |Falcon    |370.0        |
    |Falcon    |380.0        |
    --------------------------
    
    >>> snowpark_df = pd.to_snowpark(df["Animal"], index=False)
    >>> snowpark_df.order_by('"Animal"').show()
    ------------
    |"Animal"  |
    ------------
    |Falcon    |
    |Falcon    |
    |Parrot    |
    |Parrot    |
    ------------
    
[/code]
