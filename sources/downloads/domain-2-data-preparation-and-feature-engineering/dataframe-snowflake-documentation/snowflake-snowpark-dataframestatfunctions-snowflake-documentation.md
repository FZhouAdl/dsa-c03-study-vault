---
title: "snowflake.snowpark.DataFrameStatFunctions | Snowflake Documentation"
source: https://docs.snowflake.com/en/developer-guide/snowpark/reference/python/latest/snowpark/api/snowflake.snowpark.DataFrameStatFunctions.html
cert_domain: domain-2-data-preparation-and-feature-engineering
crawl_depth: 1
crawled: 2026-08-23
---

# snowflake.snowpark.DataFrameStatFunctions¶

_class _snowflake.snowpark.DataFrameStatFunctions(_dataframe : [DataFrame](snowflake.snowpark.DataFrame.html#snowflake.snowpark.DataFrame "snowflake.snowpark.dataframe.DataFrame")_)[[source]](https://github.com/snowflakedb/snowpark-python/blob/v1.54.0/src/snowflake/snowpark/dataframe_stat_functions.py#L44-L516)¶
    

Bases: `object`

Provides computed statistical functions for DataFrames. To access an object of this class, use [`DataFrame.stat`](snowflake.snowpark.DataFrame.stat.html#snowflake.snowpark.DataFrame.stat "snowflake.snowpark.DataFrame.stat").

Methods

[`approxQuantile`](snowflake.snowpark.DataFrameStatFunctions.approxQuantile.html#snowflake.snowpark.DataFrameStatFunctions.approxQuantile "snowflake.snowpark.DataFrameStatFunctions.approxQuantile")(col, percentile, *[, ...]) | For a specified numeric column and a list of desired quantiles, returns an approximate value for the column at each of the desired quantiles.  
---|---  
[`approx_quantile`](snowflake.snowpark.DataFrameStatFunctions.approx_quantile.html#snowflake.snowpark.DataFrameStatFunctions.approx_quantile "snowflake.snowpark.DataFrameStatFunctions.approx_quantile")(col, percentile, *[, ...]) | For a specified numeric column and a list of desired quantiles, returns an approximate value for the column at each of the desired quantiles.  
[`corr`](snowflake.snowpark.DataFrameStatFunctions.corr.html#snowflake.snowpark.DataFrameStatFunctions.corr "snowflake.snowpark.DataFrameStatFunctions.corr")(col1, col2, *[, statement_params]) | Calculates the correlation coefficient for non-null pairs in two numeric columns.  
[`cov`](snowflake.snowpark.DataFrameStatFunctions.cov.html#snowflake.snowpark.DataFrameStatFunctions.cov "snowflake.snowpark.DataFrameStatFunctions.cov")(col1, col2, *[, statement_params]) | Calculates the sample covariance for non-null pairs in two numeric columns.  
[`crosstab`](snowflake.snowpark.DataFrameStatFunctions.crosstab.html#snowflake.snowpark.DataFrameStatFunctions.crosstab "snowflake.snowpark.DataFrameStatFunctions.crosstab")(col1, col2, *[, statement_params]) | Computes a pair-wise frequency table (a `contingency table`) for the specified columns.  
[`sampleBy`](snowflake.snowpark.DataFrameStatFunctions.sampleBy.html#snowflake.snowpark.DataFrameStatFunctions.sampleBy "snowflake.snowpark.DataFrameStatFunctions.sampleBy")(col, fractions[, seed]) | Returns a DataFrame containing a stratified sample without replacement, based on a `dict` that specifies the fraction for each stratum.  
[`sample_by`](snowflake.snowpark.DataFrameStatFunctions.sample_by.html#snowflake.snowpark.DataFrameStatFunctions.sample_by "snowflake.snowpark.DataFrameStatFunctions.sample_by")(col, fractions[, seed]) | Returns a DataFrame containing a stratified sample without replacement, based on a `dict` that specifies the fraction for each stratum.
