---
title: "snowflake.snowpark.functions.approx_count_distinct | Snowflake Documentation"
source: https://docs.snowflake.com/en/developer-guide/snowpark/reference/python/latest/snowpark/api/snowflake.snowpark.functions.approx_count_distinct.html
cert_domain: domain-2-data-preparation-and-feature-engineering
crawl_depth: 1
crawled: 2026-08-23
---

# snowflake.snowpark.functions.approx_count_distinct¶

snowflake.snowpark.functions.approx_count_distinct(_e : Union[[Column](snowflake.snowpark.Column.html#snowflake.snowpark.Column "snowflake.snowpark.column.Column"), str]_) → [Column](snowflake.snowpark.Column.html#snowflake.snowpark.Column "snowflake.snowpark.column.Column")[[source]](https://github.com/snowflakedb/snowpark-python/blob/v1.54.0/src/snowflake/snowpark/functions.py#L863-L880)¶
    

Uses HyperLogLog to return an approximation of the distinct cardinality of the input (i.e. HLL(col1, col2, … ) returns an approximation of COUNT(DISTINCT col1, col2, … )).

Example::
    
[code]
    >>> df = session.create_dataframe([[1, 2], [3, 4], [5, 6]], schema=["a", "b"])
    >>> df.select(approx_count_distinct("a").alias("result")).show()
    ------------
    |"RESULT"  |
    ------------
    |3         |
    ------------
    
[/code]
