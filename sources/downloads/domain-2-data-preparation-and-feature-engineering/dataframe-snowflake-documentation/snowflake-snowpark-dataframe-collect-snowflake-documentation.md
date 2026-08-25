---
title: "snowflake.snowpark.DataFrame.collect | Snowflake Documentation"
source: https://docs.snowflake.com/en/developer-guide/snowpark/reference/python/latest/snowpark/api/snowflake.snowpark.DataFrame.collect.html
cert_domain: domain-2-data-preparation-and-feature-engineering
crawl_depth: 1
crawled: 2026-08-23
---

# snowflake.snowpark.DataFrame.collect¶

DataFrame.collect(_*_ , _statement_params : Optional[Dict[str, str]] = None_, _block : bool = True_, _log_on_exception : bool = False_, _case_sensitive : bool = True_, __emit_ast : bool = True_) → List[[Row](snowflake.snowpark.Row.html#snowflake.snowpark.Row "snowflake.snowpark.Row")][[source]](https://github.com/snowflakedb/snowpark-python/blob/v1.54.0/src/snowflake/snowpark/dataframe.py#L790-L841)¶
DataFrame.collect(_*_ , _statement_params : Optional[Dict[str, str]] = None_, _block : bool = False_, _log_on_exception : bool = False_, _case_sensitive : bool = True_, __emit_ast : bool = True_) → [AsyncJob](snowflake.snowpark.AsyncJob.html#snowflake.snowpark.AsyncJob "snowflake.snowpark.AsyncJob")
    

Executes the query representing this DataFrame and returns the result as a list of [`Row`](snowflake.snowpark.Row.html#snowflake.snowpark.Row "snowflake.snowpark.Row") objects.

Parameters:
    

  * **statement_params** – Dictionary of statement level parameters to be set while executing this action.

  * **block** – A bool value indicating whether this function will wait until the result is available. When it is `False`, this function executes the underlying queries of the dataframe asynchronously and returns an [`AsyncJob`](snowflake.snowpark.AsyncJob.html#snowflake.snowpark.AsyncJob "snowflake.snowpark.AsyncJob").

  * **case_sensitive** – A bool value which controls the case sensitivity of the fields in the [`Row`](snowflake.snowpark.Row.html#snowflake.snowpark.Row "snowflake.snowpark.Row") objects returned by the `collect`. Defaults to `True`.




See also

[`collect_nowait()`](snowflake.snowpark.DataFrame.collect_nowait.html#snowflake.snowpark.DataFrame.collect_nowait "snowflake.snowpark.DataFrame.collect_nowait")
