---
title: "snowflake.snowpark.DataFrame.describe | Snowflake Documentation"
source: https://docs.snowflake.com/en/developer-guide/snowpark/reference/python/latest/snowpark/api/snowflake.snowpark.DataFrame.describe.html
cert_domain: domain-2-data-preparation-and-feature-engineering
crawl_depth: 1
crawled: 2026-08-23
---

# snowflake.snowpark.DataFrame.describe¶

DataFrame.describe(_* cols: Union[str, List[str]]_, _strings_include_math_stats =False_) → [DataFrame](snowflake.snowpark.DataFrame.html#snowflake.snowpark.DataFrame "snowflake.snowpark.dataframe.DataFrame")[[source]](https://github.com/snowflakedb/snowpark-python/blob/v1.54.0/src/snowflake/snowpark/dataframe.py#L6108-L6231)¶
    

Computes basic statistics for numeric columns, which includes `count`, `mean`, `stddev`, `min`, and `max`. If no columns are provided, this function computes statistics for all numerical or string columns. Non-numeric and non-string columns will be ignored when calling this method.

Example::
    
[code]
    >>> df = session.create_dataframe([[1, 2], [3, 4]], schema=["a", "b"])
    >>> desc_result = df.describe().sort("SUMMARY").show()
    -------------------------------------------------------
    |"SUMMARY"  |"A"                 |"B"                 |
    -------------------------------------------------------
    |count      |2.0                 |2.0                 |
    |max        |3.0                 |4.0                 |
    |mean       |2.0                 |3.0                 |
    |min        |1.0                 |2.0                 |
    |stddev     |1.4142135623730951  |1.4142135623730951  |
    -------------------------------------------------------
    
[/code]

Parameters:
    

  * **cols** – The names of columns whose basic statistics are computed.

  * **strings_include_math_stats** – Whether StringType columns should have mean and stddev stats included.
