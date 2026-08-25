---
title: "snowflake.snowpark.DataFrame.count | Snowflake Documentation"
source: https://docs.snowflake.com/en/developer-guide/snowpark/reference/python/latest/snowpark/api/snowflake.snowpark.DataFrame.count.html
cert_domain: domain-2-data-preparation-and-feature-engineering
crawl_depth: 1
crawled: 2026-08-23
---

# snowflake.snowpark.DataFrame.count¶

DataFrame.count(_*_ , _statement_params : Optional[Dict[str, str]] = None_, _block : bool = True_, __emit_ast : bool = True_) → int[[source]](https://github.com/snowflakedb/snowpark-python/blob/v1.54.0/src/snowflake/snowpark/dataframe.py#L4664-L4706)¶
DataFrame.count(_*_ , _statement_params : Optional[Dict[str, str]] = None_, _block : bool = False_, __emit_ast : bool = True_) → [AsyncJob](snowflake.snowpark.AsyncJob.html#snowflake.snowpark.AsyncJob "snowflake.snowpark.AsyncJob")
    

Executes the query representing this DataFrame and returns the number of rows in the result (similar to the COUNT function in SQL).

Parameters:
    

  * **statement_params** – Dictionary of statement level parameters to be set while executing this action.

  * **block** – A bool value indicating whether this function will wait until the result is available. When it is `False`, this function executes the underlying queries of the dataframe asynchronously and returns an [`AsyncJob`](snowflake.snowpark.AsyncJob.html#snowflake.snowpark.AsyncJob "snowflake.snowpark.AsyncJob").
