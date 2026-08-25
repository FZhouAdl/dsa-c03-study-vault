---
title: "modin.pandas.to_pandas | Snowflake Documentation"
source: https://docs.snowflake.com/en/developer-guide/snowpark/reference/python/1.21.0/modin/pandas_api/snowflake.snowpark.modin.pandas.to_pandas
cert_domain: domain-2-data-preparation-and-feature-engineering
crawl_depth: 1
crawled: 2026-08-23
---

You are viewing documentation about an older version (1.21.0).  [View latest version](/en/developer-guide/snowpark/reference/python/1.54.0/index)

# modin.pandas.to_pandas¶

snowflake.snowpark.modin.pandas.to_pandas(_obj : Union[[DataFrame](modin.pandas.DataFrame.html#modin.pandas.DataFrame "snowflake.snowpark.modin.pandas.dataframe.DataFrame"), [Series](modin.pandas.Series.html#modin.pandas.Series "snowflake.snowpark.modin.pandas.series.Series")]_, _*_ , _statement_params : Optional[dict[str, str]] = None_, _** kwargs: Any_) → Union[[DataFrame](modin.pandas.DataFrame.html#modin.pandas.DataFrame "snowflake.snowpark.modin.pandas.dataframe.DataFrame"), [Series](modin.pandas.Series.html#modin.pandas.Series "snowflake.snowpark.modin.pandas.series.Series")][[source]](https://github.com/snowflakedb/snowpark-python/blob/v1.21.0/src/snowflake/snowpark/modin/plugin/extensions/pd_extensions.py#L560-L602)¶
    

Convert Snowpark pandas DataFrame or Series to pandas DataFrame or Series

Parameters:
    

  * **obj** – The object to be converted to native pandas. It must be either a Snowpark pandas DataFrame or Series

  * **statement_params** – Dictionary of statement level parameters to be set while executing this action.



Returns:
    

pandas DataFrame or Series

See also

  * `DataFrame.to_pandas`

  * `Series.to_pandas`




Examples
[code] 
    >>> df = pd.DataFrame({'Animal': ['Falcon', 'Falcon',
    ...                               'Parrot', 'Parrot'],
    ...                    'Max Speed': [380., 370., 24., 26.]})
    >>> pd.to_pandas(df)
       Animal  Max Speed
    0  Falcon      380.0
    1  Falcon      370.0
    2  Parrot       24.0
    3  Parrot       26.0
    
[/code]
[code] 
    >>> pd.to_pandas(df['Animal'])
    0    Falcon
    1    Falcon
    2    Parrot
    3    Parrot
    Name: Animal, dtype: object
    
[/code]
