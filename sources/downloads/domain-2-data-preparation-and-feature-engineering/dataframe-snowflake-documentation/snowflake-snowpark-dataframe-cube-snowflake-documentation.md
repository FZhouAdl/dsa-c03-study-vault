---
title: "snowflake.snowpark.DataFrame.cube | Snowflake Documentation"
source: https://docs.snowflake.com/en/developer-guide/snowpark/reference/python/latest/snowpark/api/snowflake.snowpark.DataFrame.cube.html
cert_domain: domain-2-data-preparation-and-feature-engineering
crawl_depth: 1
crawled: 2026-08-23
note: little server-rendered content (JS-rendered or form-gated page)
---

# snowflake.snowpark.DataFrame.cube¶

DataFrame.cube(_* cols: Union[[Column](snowflake.snowpark.Column.html#snowflake.snowpark.Column "snowflake.snowpark.column.Column"), str, Iterable[Union[[Column](snowflake.snowpark.Column.html#snowflake.snowpark.Column "snowflake.snowpark.column.Column"), str]]]_) → [RelationalGroupedDataFrame](snowflake.snowpark.RelationalGroupedDataFrame.html#snowflake.snowpark.RelationalGroupedDataFrame "snowflake.snowpark.relational_grouped_dataframe.RelationalGroupedDataFrame")[[source]](https://github.com/snowflakedb/snowpark-python/blob/v1.54.0/src/snowflake/snowpark/dataframe.py#L2689-L2720)¶
    

Performs a SQL [GROUP BY CUBE](https://docs.snowflake.com/en/sql-reference/constructs/group-by-cube.html). on the DataFrame.

Parameters:
    

**cols** – The columns to group by cube.
