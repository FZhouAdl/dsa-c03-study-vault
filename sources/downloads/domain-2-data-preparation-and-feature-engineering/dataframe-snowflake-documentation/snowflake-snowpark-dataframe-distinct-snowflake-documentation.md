---
title: "snowflake.snowpark.DataFrame.distinct | Snowflake Documentation"
source: https://docs.snowflake.com/en/developer-guide/snowpark/reference/python/latest/snowpark/api/snowflake.snowpark.DataFrame.distinct.html
cert_domain: domain-2-data-preparation-and-feature-engineering
crawl_depth: 1
crawled: 2026-08-23
note: little server-rendered content (JS-rendered or form-gated page)
---

# snowflake.snowpark.DataFrame.distinct¶

DataFrame.distinct() → [DataFrame](snowflake.snowpark.DataFrame.html#snowflake.snowpark.DataFrame "snowflake.snowpark.dataframe.DataFrame")[[source]](https://github.com/snowflakedb/snowpark-python/blob/v1.54.0/src/snowflake/snowpark/dataframe.py#L2722-L2775)¶
    

Returns a new DataFrame that contains only the rows with distinct values from the current DataFrame.

This is equivalent to performing a SELECT DISTINCT in SQL.
