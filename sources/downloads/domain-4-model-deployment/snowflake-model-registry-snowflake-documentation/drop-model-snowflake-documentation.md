---
title: "DROP MODEL | Snowflake Documentation"
source: https://docs.snowflake.com/sql-reference/sql/drop-model
cert_domain: domain-4-model-deployment
crawl_depth: 1
crawled: 2026-08-23
---

# DROP MODEL¶

Removes a machine learning model from the current/specified schema.

See also:
    

[CREATE MODEL](/sql-reference/sql/create-model) , [ALTER MODEL](/sql-reference/sql/alter-model) , [SHOW MODELS](/sql-reference/sql/show-models)

## Syntax¶
[code] 
    DROP MODEL <name>
    
[/code]

## Parameters¶

`_name_`
    

Specifies the identifier for the model to drop.

If the identifier contains spaces or special characters, the entire string must be enclosed in double quotes. Identifiers enclosed in double quotes are also case-sensitive.

For more information, see [Identifier requirements](/sql-reference/identifiers-syntax).

If the model identifier is not fully-qualified (in the form of `_db_name_._schema_name_._model_name_` or `_schema_name_._model_name_`), the command looks for the model in the current schema for the session.

## Usage notes¶

  * All versions in the model are dropped along with the model.
  * There is no UNDROP MODEL command. To restore a dropped model, train and log it again.
