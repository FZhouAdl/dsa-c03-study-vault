---
title: "snowflake.snowpark.DataFrameAnalyticsFunctions | Snowflake Documentation"
source: https://docs.snowflake.com/en/developer-guide/snowpark/reference/python/latest/snowpark/api/snowflake.snowpark.DataFrameAnalyticsFunctions.html
cert_domain: domain-2-data-preparation-and-feature-engineering
crawl_depth: 1
crawled: 2026-08-23
---

# snowflake.snowpark.DataFrameAnalyticsFunctions¶

_class _snowflake.snowpark.DataFrameAnalyticsFunctions(_dataframe : [DataFrame](snowflake.snowpark.DataFrame.html#snowflake.snowpark.DataFrame "snowflake.snowpark.dataframe.DataFrame")_)[[source]](https://github.com/snowflakedb/snowpark-python/blob/v1.54.0/src/snowflake/snowpark/dataframe_analytics_functions.py#L30-L802)¶
    

Bases: `object`

Provides data analytics functions for DataFrames. To access an object of this class, use `DataFrame.analytics`.

Methods

[`compute_lag`](snowflake.snowpark.DataFrameAnalyticsFunctions.compute_lag.html#snowflake.snowpark.DataFrameAnalyticsFunctions.compute_lag "snowflake.snowpark.DataFrameAnalyticsFunctions.compute_lag")(cols, lags, order_by, group_by) | Creates lag columns to the specified columns of the DataFrame by grouping and ordering criteria.  
---|---  
[`compute_lead`](snowflake.snowpark.DataFrameAnalyticsFunctions.compute_lead.html#snowflake.snowpark.DataFrameAnalyticsFunctions.compute_lead "snowflake.snowpark.DataFrameAnalyticsFunctions.compute_lead")(cols, leads, order_by, group_by) | Creates lead columns to the specified columns of the DataFrame by grouping and ordering criteria.  
[`cumulative_agg`](snowflake.snowpark.DataFrameAnalyticsFunctions.cumulative_agg.html#snowflake.snowpark.DataFrameAnalyticsFunctions.cumulative_agg "snowflake.snowpark.DataFrameAnalyticsFunctions.cumulative_agg")(aggs, group_by, order_by, ...) | Applies cummulative aggregations to the specified columns of the DataFrame using defined window direction, and grouping and ordering criteria.  
[`moving_agg`](snowflake.snowpark.DataFrameAnalyticsFunctions.moving_agg.html#snowflake.snowpark.DataFrameAnalyticsFunctions.moving_agg "snowflake.snowpark.DataFrameAnalyticsFunctions.moving_agg")(aggs, window_sizes, order_by, ...) | Applies moving aggregations to the specified columns of the DataFrame using defined window sizes, and grouping and ordering criteria.  
[`time_series_agg`](snowflake.snowpark.DataFrameAnalyticsFunctions.time_series_agg.html#snowflake.snowpark.DataFrameAnalyticsFunctions.time_series_agg "snowflake.snowpark.DataFrameAnalyticsFunctions.time_series_agg")(time_col, aggs, windows, ...) | Applies aggregations to the specified columns of the DataFrame over specified time windows, and grouping criteria.
