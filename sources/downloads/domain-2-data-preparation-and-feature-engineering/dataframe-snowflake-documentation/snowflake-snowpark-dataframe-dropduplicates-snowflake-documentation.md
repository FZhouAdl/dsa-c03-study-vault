---
title: "snowflake.snowpark.DataFrame.dropDuplicates | Snowflake Documentation"
source: https://docs.snowflake.com/en/developer-guide/snowpark/reference/python/latest/snowpark/api/snowflake.snowpark.DataFrame.dropDuplicates.html
cert_domain: domain-2-data-preparation-and-feature-engineering
crawl_depth: 1
crawled: 2026-08-23
---

# snowflake.snowpark.DataFrame.dropDuplicates¶

DataFrame.dropDuplicates(_* subset: Union[str, Iterable[str]]_) → [DataFrame](snowflake.snowpark.DataFrame.html#snowflake.snowpark.DataFrame "snowflake.snowpark.dataframe.DataFrame")[[source]](https://github.com/snowflakedb/snowpark-python/blob/v1.54.0/src/snowflake/snowpark/dataframe.py#L2777-L2848)¶
    

Creates a new DataFrame by removing duplicated rows on given subset of columns.

If no subset of columns is specified, this function is the same as the [`distinct()`](snowflake.snowpark.DataFrame.distinct.html#snowflake.snowpark.DataFrame.distinct "snowflake.snowpark.DataFrame.distinct") function. The result is non-deterministic when removing duplicated rows from the subset of columns but not all columns.

For example, if we have a DataFrame `df`, which has columns (“a”, “b”, “c”) and contains three rows `(1, 1, 1), (1, 1, 2), (1, 2, 3)`, the result of `df.dropDuplicates("a", "b")` can be either `(1, 1, 1), (1, 2, 3)` or `(1, 1, 2), (1, 2, 3)`

Parameters:
    

**subset** – The column names on which duplicates are dropped.

`dropDuplicates()` is an alias of [`drop_duplicates()`](snowflake.snowpark.DataFrame.drop_duplicates.html#snowflake.snowpark.DataFrame.drop_duplicates "snowflake.snowpark.DataFrame.drop_duplicates").
