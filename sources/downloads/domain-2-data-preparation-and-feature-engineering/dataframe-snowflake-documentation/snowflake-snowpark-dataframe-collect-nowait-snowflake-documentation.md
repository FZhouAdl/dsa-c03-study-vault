---
title: "snowflake.snowpark.DataFrame.collect_nowait | Snowflake Documentation"
source: https://docs.snowflake.com/en/developer-guide/snowpark/reference/python/latest/snowpark/api/snowflake.snowpark.DataFrame.collect_nowait.html
cert_domain: domain-2-data-preparation-and-feature-engineering
crawl_depth: 1
crawled: 2026-08-23
---

# snowflake.snowpark.DataFrame.collect_nowait¶

DataFrame.collect_nowait(_*_ , _statement_params : Optional[Dict[str, str]] = None_, _log_on_exception : bool = False_, _case_sensitive : bool = True_) → [AsyncJob](snowflake.snowpark.AsyncJob.html#snowflake.snowpark.AsyncJob "snowflake.snowpark.async_job.AsyncJob")[[source]](https://github.com/snowflakedb/snowpark-python/blob/v1.54.0/src/snowflake/snowpark/dataframe.py#L843-L890)¶
    

Executes the query representing this DataFrame asynchronously and returns: class:AsyncJob. It is equivalent to `collect(block=False)`.

Parameters:
    

  * **statement_params** – Dictionary of statement level parameters to be set while executing this action.

  * **case_sensitive** – A bool value which is controls the case sensitivity of the fields in the [`Row`](snowflake.snowpark.Row.html#snowflake.snowpark.Row "snowflake.snowpark.Row") objects after collecting the result using [`AsyncJob.result()`](snowflake.snowpark.AsyncJob.result.html#snowflake.snowpark.AsyncJob.result "snowflake.snowpark.AsyncJob.result"). Defaults to `True`.




See also

[`collect()`](snowflake.snowpark.DataFrame.collect.html#snowflake.snowpark.DataFrame.collect "snowflake.snowpark.DataFrame.collect")
