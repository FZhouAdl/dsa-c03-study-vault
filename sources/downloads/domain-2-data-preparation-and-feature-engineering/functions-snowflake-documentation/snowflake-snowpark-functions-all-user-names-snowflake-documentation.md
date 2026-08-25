---
title: "snowflake.snowpark.functions.all_user_names | Snowflake Documentation"
source: https://docs.snowflake.com/en/developer-guide/snowpark/reference/python/latest/snowpark/api/snowflake.snowpark.functions.all_user_names.html
cert_domain: domain-2-data-preparation-and-feature-engineering
crawl_depth: 1
crawled: 2026-08-23
---

# snowflake.snowpark.functions.all_user_names¶

snowflake.snowpark.functions.all_user_names() → [Column](snowflake.snowpark.Column.html#snowflake.snowpark.Column "snowflake.snowpark.column.Column")[[source]](https://github.com/snowflakedb/snowpark-python/blob/v1.54.0/src/snowflake/snowpark/_functions/scalar_functions.py#L20-L32)¶
    

Returns a list of all user names in the current account.

Example:
[code] 
    >>> # Return result is tied to session, so we only test if the result exists
    >>> result = session.create_dataframe([1]).select(all_user_names()).collect()
    >>> assert result[0]['ALL_USER_NAMES()'] is not None
    >>> assert isinstance(result[0]['ALL_USER_NAMES()'], str)
    
[/code]
