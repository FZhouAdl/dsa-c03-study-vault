---
title: "snowflake.snowpark.functions.sql_expr | Snowflake Documentation"
source: https://docs.snowflake.com/en/developer-guide/snowpark/reference/python/latest/snowpark/api/snowflake.snowpark.functions.sql_expr.html
cert_domain: domain-2-data-preparation-and-feature-engineering
crawl_depth: 1
crawled: 2026-08-23
---

# snowflake.snowpark.functions.sql_expr¶

snowflake.snowpark.functions.sql_expr(_sql : str_, _*_ , _is_constant : bool = False_) → [Column](snowflake.snowpark.Column.html#snowflake.snowpark.Column "snowflake.snowpark.column.Column")[[source]](https://github.com/snowflakedb/snowpark-python/blob/v1.54.0/src/snowflake/snowpark/functions.py#L399-L429)¶
    

Creates a [`Column`](snowflake.snowpark.Column.html#snowflake.snowpark.Column "snowflake.snowpark.Column") expression from raw SQL text. Note that the function does not interpret or check the SQL text.

Parameters:
    

  * **sql** – The raw SQL text to embed as a column expression.

  * **is_constant** – When `True`, indicates that `sql` is a constant value (for example an object/array/scalar literal) that does not reference any input columns. This allows the SQL simplifier to treat the expression as having no column dependencies.




Example::
    
[code]
    >>> df = session.create_dataframe([[1, 2], [3, 4]], schema=["A", "B"])
    >>> df.select(sql_expr("a + 1").as_("c"), sql_expr("a = 1").as_("d")).collect()  # use SQL expression
    [Row(C=2, D=True), Row(C=4, D=False)]
    
[/code]
