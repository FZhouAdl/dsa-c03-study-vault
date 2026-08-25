---
title: "snowflake.snowpark.functions.to_decimal | Snowflake Documentation"
source: https://docs.snowflake.com/en/developer-guide/snowpark/reference/python/latest/snowpark/api/snowflake.snowpark.functions.to_decimal.html
cert_domain: domain-2-data-preparation-and-feature-engineering
crawl_depth: 1
crawled: 2026-08-23
---

# snowflake.snowpark.functions.to_decimal¶

snowflake.snowpark.functions.to_decimal(_e : Union[[Column](snowflake.snowpark.Column.html#snowflake.snowpark.Column "snowflake.snowpark.column.Column"), str]_, _precision : int_, _scale : int_) → [Column](snowflake.snowpark.Column.html#snowflake.snowpark.Column "snowflake.snowpark.column.Column")[[source]](https://github.com/snowflakedb/snowpark-python/blob/v1.54.0/src/snowflake/snowpark/functions.py#L2066-L2092)¶
    

Converts an input expression to a decimal.

Example::
    
[code]
    >>> df = session.create_dataframe(['12', '11.3', '-90.12345'], schema=['a'])
    >>> df.select(to_decimal(col('a'), 38, 0).as_('ans')).collect()
    [Row(ANS=12), Row(ANS=11), Row(ANS=-90)]
    
[/code]
[code] 
    >>> df.select(to_decimal(col('a'), 38, 2).as_('ans')).collect()
    [Row(ANS=Decimal('12.00')), Row(ANS=Decimal('11.30')), Row(ANS=Decimal('-90.12'))]
    
[/code]
