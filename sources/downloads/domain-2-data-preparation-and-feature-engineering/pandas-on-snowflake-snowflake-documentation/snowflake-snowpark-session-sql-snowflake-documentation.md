---
title: "snowflake.snowpark.Session.sql | Snowflake Documentation"
source: https://docs.snowflake.com/en/developer-guide/snowpark/reference/python/latest/snowpark/api/snowflake.snowpark.Session.sql
cert_domain: domain-2-data-preparation-and-feature-engineering
crawl_depth: 1
crawled: 2026-08-23
---

# snowflake.snowpark.Session.sql¶

Session.sql(_query : str_, _params : Optional[Sequence[Any]] = None_) → [DataFrame](snowflake.snowpark.DataFrame.html#snowflake.snowpark.DataFrame "snowflake.snowpark.dataframe.DataFrame")[[source]](https://github.com/snowflakedb/snowpark-python/blob/v1.54.0/src/snowflake/snowpark/session.py#L3087-L3169)¶
    

Returns a new DataFrame representing the results of a SQL query.

Note

You can use this method to execute a SQL query lazily, which means the SQL is not executed until methods like [`DataFrame.collect()`](snowflake.snowpark.DataFrame.collect.html#snowflake.snowpark.DataFrame.collect "snowflake.snowpark.DataFrame.collect") or [`DataFrame.to_pandas()`](snowflake.snowpark.DataFrame.to_pandas.html#snowflake.snowpark.DataFrame.to_pandas "snowflake.snowpark.DataFrame.to_pandas") evaluate the DataFrame. For **immediate execution** , chain the call with the collect method: session.sql(query).collect().

Parameters:
    

  * **query** – The SQL statement to execute.

  * **params** – binding parameters. We only support qmark bind variables. For more information, check <https://docs.snowflake.com/en/developer-guide/python-connector/python-connector-example#qmark-or-numeric-binding>

  * **_ast_stmt** – when invoked internally, supplies the AST to use for the resulting dataframe.




Example:
[code] 
    >>> # create a dataframe from a SQL query
    >>> df = session.sql("select 1/2")
    >>> # execute the query
    >>> df.collect()
    [Row(1/2=Decimal('0.500000'))]
    
    >>> # Use params to bind variables
    >>> session.sql("select * from values (?, ?), (?, ?)", params=[1, "a", 2, "b"]).sort("column1").collect()
    [Row(COLUMN1=1, COLUMN2='a'), Row(COLUMN1=2, COLUMN2='b')]
    
[/code]
