---
title: "modin.pandas.DataFrame.apply | Snowflake Documentation"
source: https://docs.snowflake.com/en/developer-guide/snowpark/reference/python/latest/modin/pandas_api/modin.pandas.DataFrame.apply
cert_domain: domain-2-data-preparation-and-feature-engineering
crawl_depth: 1
crawled: 2026-08-23
---

# modin.pandas.DataFrame.apply¶

DataFrame.apply(_func_ , _axis =0_, _raw =False_, _result_type =None_, _args =()_, _by_row ='compat'_, _engine ='python'_, _engine_kwargs =None_, _** kwargs_) → Union[[DataFrame](modin.pandas.DataFrame.html#modin.pandas.DataFrame "modin.pandas.dataframe.DataFrame"), [Series](modin.pandas.Series.html#modin.pandas.Series "modin.pandas.series.Series")][[source]](https://github.com/snowflakedb/snowpark-python/blob/v1.54.0/.tox/docs/lib/python3.10/site-packages/modin/pandas/dataframe.py#L423-L482)¶
    

Apply a function along an axis of the DataFrame.

Objects passed to the function are Series objects whose index is either the DataFrame’s index (`axis=0`) or the DataFrame’s columns (`axis=1`). By default (`result_type=None`), the final return type is inferred from the return type of the applied function. Otherwise, it depends on the result_type argument.

Snowpark pandas currently only supports `apply` with `axis=1` and callable `func`.

Parameters:
    

  * **func** (_function_) – A Python function object to apply to each column or row.

  * **axis** (_{0_ _or_ _'index'__,__1_ _or_ _'columns'}__,__default 0_) – 

Axis along which the function is applied:

    * 0 or ‘index’: apply function to each column.

    * 1 or ‘columns’: apply function to each row.

  * **raw** (_bool_ _,__default False_) – 

Determines if row or column is passed as a Series or ndarray object:

    * `False` : passes each row or column as a Series to the function.

    * `True` : the passed function will receive ndarray objects instead.

  * **result_type** (_{'expand'__,__'reduce'__,__'broadcast'__,__None}__,__default None_) – 

These only act when `axis=1` (columns):

    * ’expand’ : list-like results will be turned into columns.

    * ’reduce’ : returns a Series if possible rather than expanding list-like results. This is the opposite of ‘expand’.

    * ’broadcast’ : results will be broadcast to the original shape of the DataFrame, the original index and columns will be retained.

Snowpark pandas does not yet support the `result_type` parameter.

  * **args** (_tuple_) – Positional arguments to pass to func in addition to the array/series.

  * ****kwargs** – Additional keyword arguments to pass as keywords arguments to func.



Returns:
    

Result of applying `func` along the given axis of the DataFrame.

Return type:
    

[Series](modin.pandas.Series.html#modin.pandas.Series "modin.pandas.Series") or [DataFrame](modin.pandas.DataFrame.html#modin.pandas.DataFrame "modin.pandas.DataFrame")

See also

[`Series.apply`](modin.pandas.Series.apply.html#modin.pandas.Series.apply "modin.pandas.Series.apply") : For applying more complex functions on a Series.

[`DataFrame.applymap`](modin.pandas.DataFrame.applymap.html#modin.pandas.DataFrame.applymap "modin.pandas.DataFrame.applymap") : Apply a function elementwise on a whole DataFrame.

Notes

1\. When `func` has a type annotation for its return value, the result will be cast to the corresponding dtype. When no type annotation is provided, data will be converted to VARIANT type in Snowflake, and the result will have `dtype=object`. In this case, the return value must be JSON-serializable, which can be a valid input to `json.dumps` (e.g., `dict` and `list` objects are JSON-serializable, but `bytes` and `datetime.datetime` objects are not). The return type hint is used only when `func` is a series-to-scalar function.

2\. Under the hood, we use Snowflake Vectorized Python UDFs to implement apply() method with axis=1. You can find type mappings from Snowflake SQL types to pandas dtypes [here](https://docs.snowflake.com/en/developer-guide/udf/python/udf-python-batch#type-support).

3\. Snowflake supports two types of NULL values in variant data: [JSON NULL and SQL NULL](https://docs.snowflake.com/en/user-guide/semistructured-considerations#null-values). When no type annotation is provided and Variant data is returned, Python `None` is translated to JSON NULL, and all other pandas missing values (np.nan, pd.NA, pd.NaT) are translated to SQL NULL.

4\. If `func` is a series-to-series function that can also be used as a scalar-to-scalar function (e.g., `np.sqrt`, `lambda x: x+1`), using `df.applymap()` to apply the function element-wise may give better performance.

5\. When `func` can return a series with different indices, e.g., `lambda x: pd.Series([1, 2], index=["a", "b"] if x.sum() > 2 else ["b", "c"])`, the values with the same label will be merged together.

6\. The index values of returned series from `func` must be JSON-serializable. For example, `lambda x: pd.Series([1], index=[bytes(1)])` will raise a SQL execption because python `bytes` objects are not JSON-serializable.

7\. When `func` uses any first-party modules or third-party packages inside the function, you need to add these dependencies via `session.add_import()` and `session.add_packages()`.

8\. The Snowpark pandas module cannot currently be referenced inside the definition of `func`. If you need to call a general pandas API like `pd.Timestamp` inside `func`, please use the original `pandas` module (with `import pandas`) as a workaround.

9\. To create a permanent function, pass the “snowflake_udf_params” dictionary argument to `apply`. See examples below for details.

Examples
[code] 
    >>> df = pd.DataFrame([[2, 0], [3, 7], [4, 9]], columns=['A', 'B'])
    >>> df
       A  B
    0  2  0
    1  3  7
    2  4  9
    
[/code]

Using a reducing function on `axis=1`:
[code] 
    >>> df.apply(np.sum, axis=1)
    0     2
    1    10
    2    13
    dtype: int64
    
[/code]

Returning a list-like object will result in a Series:
[code] 
    >>> df.apply(lambda x: [1, 2], axis=1)
    0    [1, 2]
    1    [1, 2]
    2    [1, 2]
    dtype: object
    
[/code]

To work with 3rd party packages, add them to the current session:
[code] 
    >>> import scipy.stats
    >>> pd.session.custom_package_usage_config['enabled'] = True
    >>> pd.session.add_packages(['numpy', scipy])
    >>> df.apply(lambda x: np.dot(x * scipy.stats.norm.cdf(0), x * scipy.stats.norm.cdf(0)), axis=1)
    0     1.00
    1    14.50
    2    24.25
    dtype: float64
    
[/code]

To generate a permanent UDTF, pass a dictionary as the snowflake_udf_params argument to apply. The following example generates a permanent UDTF named “permanent_double”:
[code] 
    >>> session.sql("CREATE STAGE sample_upload_stage").collect()  
    >>> def double(x: int) -> int:  
    ...     return x * 2
    ...
    >>> df.apply(double, snowflake_udf_params={"name": "permanent_double", "stage_location": "@sample_upload_stage"})  
       A   B
    0  4   0
    1  6  14
    2  8  18
    
[/code]

You may also pass “replace” and “if_not_exists” in the dictionary to overwrite or re-use existing UDTFs.

With the “replace” flag:
[code] 
    >>> df.apply(double, snowflake_udf_params={  
    ...     "name": "permanent_double",
    ...     "stage_location": "@sample_upload_stage",
    ...     "replace": True,
    ... })
    
[/code]

With the “if_not_exists” flag:
[code] 
    >>> df.apply(double, snowflake_udf_params={  
    ...     "name": "permanent_double",
    ...     "stage_location": "@sample_upload_stage",
    ...     "if_not_exists": True,
    ... })
    
[/code]

Note that Snowpark pandas may still attempt to upload a new UDTF even when “if_not_exists” is passed; the generated SQL will just contain a CREATE FUNCTION IF NOT EXISTS query instead. Subsequent calls to apply within the same session may skip this query.

Passing the immutable keyword creates an immutable UDTF, which assumes that the UDTF will return the same result for the same inputs.
[code] 
    >>> df.apply(double, snowflake_udf_params={  
    ...     "name": "permanent_double",
    ...     "stage_location": "@sample_upload_stage",
    ...     "replace": True,
    ...     "immutable": True,
    ... })
    
[/code]
