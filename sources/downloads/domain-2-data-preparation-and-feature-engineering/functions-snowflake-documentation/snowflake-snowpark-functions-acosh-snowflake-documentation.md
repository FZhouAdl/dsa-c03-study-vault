---
title: "snowflake.snowpark.functions.acosh | Snowflake Documentation"
source: https://docs.snowflake.com/en/developer-guide/snowpark/reference/python/latest/snowpark/api/snowflake.snowpark.functions.acosh.html
cert_domain: domain-2-data-preparation-and-feature-engineering
crawl_depth: 1
crawled: 2026-08-23
---

# snowflake.snowpark.functions.acosh¶

snowflake.snowpark.functions.acosh(_e : Union[[Column](snowflake.snowpark.Column.html#snowflake.snowpark.Column "snowflake.snowpark.column.Column"), str]_) → [Column](snowflake.snowpark.Column.html#snowflake.snowpark.Column "snowflake.snowpark.column.Column")[[source]](https://github.com/snowflakedb/snowpark-python/blob/v1.54.0/src/snowflake/snowpark/functions.py#L11553-L11565)¶
    

Returns the inverse(arc) hyperbolic cosine of the input value.

Example:
[code] 
    >>> df = session.create_dataframe([2.352409615], schema=["a"])
    >>> df.select(acosh("a").as_("acosh")).collect()
    [Row(ACOSH=1.4999999998857607)]
    
[/code]
