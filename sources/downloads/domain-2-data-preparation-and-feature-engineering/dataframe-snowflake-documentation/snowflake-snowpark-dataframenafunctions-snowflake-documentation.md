---
title: "snowflake.snowpark.DataFrameNaFunctions | Snowflake Documentation"
source: https://docs.snowflake.com/en/developer-guide/snowpark/reference/python/latest/snowpark/api/snowflake.snowpark.DataFrameNaFunctions.html
cert_domain: domain-2-data-preparation-and-feature-engineering
crawl_depth: 1
crawled: 2026-08-23
---

# snowflake.snowpark.DataFrameNaFunctions¶

_class _snowflake.snowpark.DataFrameNaFunctions(_dataframe : [DataFrame](snowflake.snowpark.DataFrame.html#snowflake.snowpark.DataFrame "snowflake.snowpark.dataframe.DataFrame")_)[[source]](https://github.com/snowflakedb/snowpark-python/blob/v1.54.0/src/snowflake/snowpark/dataframe_na_functions.py#L82-L729)¶
    

Bases: `object`

Provides functions for handling missing values in a [`DataFrame`](snowflake.snowpark.DataFrame.html#snowflake.snowpark.DataFrame "snowflake.snowpark.DataFrame").

Methods

[`drop`](snowflake.snowpark.DataFrameNaFunctions.drop.html#snowflake.snowpark.DataFrameNaFunctions.drop "snowflake.snowpark.DataFrameNaFunctions.drop")([how, thresh, subset]) | Returns a new DataFrame that excludes all rows containing fewer than a specified number of non-null and non-NaN values in the specified columns.  
---|---  
[`fill`](snowflake.snowpark.DataFrameNaFunctions.fill.html#snowflake.snowpark.DataFrameNaFunctions.fill "snowflake.snowpark.DataFrameNaFunctions.fill")(value[, subset, include_decimal]) | Returns a new DataFrame that replaces all null and NaN values in the specified columns with the values provided.  
[`replace`](snowflake.snowpark.DataFrameNaFunctions.replace.html#snowflake.snowpark.DataFrameNaFunctions.replace "snowflake.snowpark.DataFrameNaFunctions.replace")(to_replace[, value, subset, ...]) | Returns a new DataFrame that replaces values in the specified columns.
