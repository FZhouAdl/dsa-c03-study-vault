---
title: "snowflake.snowpark.functions.add_months | Snowflake Documentation"
source: https://docs.snowflake.com/en/developer-guide/snowpark/reference/python/latest/snowpark/api/snowflake.snowpark.functions.add_months.html
cert_domain: domain-2-data-preparation-and-feature-engineering
crawl_depth: 1
crawled: 2026-08-23
---

# snowflake.snowpark.functions.add_months¶

snowflake.snowpark.functions.add_months(_date_or_timestamp : Union[[Column](snowflake.snowpark.Column.html#snowflake.snowpark.Column "snowflake.snowpark.column.Column"), str]_, _number_of_months : Union[[Column](snowflake.snowpark.Column.html#snowflake.snowpark.Column "snowflake.snowpark.column.Column"), int]_) → [Column](snowflake.snowpark.Column.html#snowflake.snowpark.Column "snowflake.snowpark.column.Column")[[source]](https://github.com/snowflakedb/snowpark-python/blob/v1.54.0/src/snowflake/snowpark/functions.py#L621-L636)¶
    

Adds or subtracts a specified number of months to a date or timestamp, preserving the end-of-month information.

Example
[code] 
    >>> import datetime
    >>> df = session.create_dataframe([datetime.date(2022, 4, 6)], schema=["d"])
    >>> df.select(add_months("d", 4)).collect()[0][0]
    datetime.date(2022, 8, 6)
    
[/code]
