---
title: "snowflake.snowpark.DataFrame.explain | Snowflake Documentation"
source: https://docs.snowflake.com/en/developer-guide/snowpark/reference/python/latest/snowpark/api/snowflake.snowpark.DataFrame.explain.html
cert_domain: domain-2-data-preparation-and-feature-engineering
crawl_depth: 1
crawled: 2026-08-23
---

# snowflake.snowpark.DataFrame.explain¶

DataFrame.explain() → None[[source]](https://github.com/snowflakedb/snowpark-python/blob/v1.54.0/src/snowflake/snowpark/dataframe.py#L6767-L6775)¶
    

Prints the list of queries that will be executed to evaluate this DataFrame. Prints the query execution plan if only one SELECT/DML/DDL statement will be executed.

For more information about the query execution plan, see the [EXPLAIN](https://docs.snowflake.com/en/sql-reference/sql/explain.html) command.
