---
title: "snowflake.snowpark.DataFrame.create_or_replace_view | Snowflake Documentation"
source: https://docs.snowflake.com/en/developer-guide/snowpark/reference/python/latest/snowpark/api/snowflake.snowpark.DataFrame.create_or_replace_view.html
cert_domain: domain-2-data-preparation-and-feature-engineering
crawl_depth: 1
crawled: 2026-08-23
---

# snowflake.snowpark.DataFrame.create_or_replace_view¶

DataFrame.create_or_replace_view(_name : Union[str, Iterable[str]]_, _*_ , _comment : Optional[str] = None_, _statement_params : Optional[Dict[str, str]] = None_, _copy_grants : bool = False_) → List[[Row](snowflake.snowpark.Row.html#snowflake.snowpark.Row "snowflake.snowpark.row.Row")][[source]](https://github.com/snowflakedb/snowpark-python/blob/v1.54.0/src/snowflake/snowpark/dataframe.py#L5512-L5570)¶
    

Creates a view that captures the computation expressed by this DataFrame.

For `name`, you can include the database and schema name (i.e. specify a fully-qualified name). If no database name or schema name are specified, the view will be created in the current database or schema.

`name` must be a valid [Snowflake identifier](https://docs.snowflake.com/en/sql-reference/identifiers-syntax.html).

Parameters:
    

  * **name** – The name of the view to create or replace. Can be a list of strings that specifies the database name, schema name, and view name.

  * **comment** – Adds a comment for the created view. See [COMMENT](https://docs.snowflake.com/en/sql-reference/sql/comment).

  * **copy_grants** – A boolean value that specifies whether to retain the access permissions from the original view when a new view is created. Defaults to False.

  * **statement_params** – Dictionary of statement level parameters to be set while executing this action.
