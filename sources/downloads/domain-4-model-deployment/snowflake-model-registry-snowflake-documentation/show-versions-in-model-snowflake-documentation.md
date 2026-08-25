---
title: "SHOW VERSIONS IN MODEL | Snowflake Documentation"
source: https://docs.snowflake.com/sql-reference/sql/show-versions-in-model
cert_domain: domain-4-model-deployment
crawl_depth: 1
crawled: 2026-08-23
---

# SHOW VERSIONS IN MODEL¶

Lists the versions in a machine learning model. Models may have multiple versions, one of which must be designated as the default (see [ALTER MODEL](/sql-reference/sql/alter-model)).

The output returns table metadata and properties, ordered lexicographically by database, schema, and model name (see Output in this topic for descriptions of the output columns). This is important to note if you wish to filter the results using the provided filters.

See also:
    

[CREATE MODEL](/sql-reference/sql/create-model) , [DROP MODEL](/sql-reference/sql/drop-model) , [ALTER MODEL](/sql-reference/sql/alter-model), [SHOW MODELS](/sql-reference/sql/show-models)

## Syntax¶
[code] 
    SHOW VERSIONS [ LIKE '<pattern>' ] IN MODEL <model_name>
    
[/code]

## Parameters¶

`LIKE 'pattern'`
    

Optionally filters the command output by object name. The filter uses case-insensitive pattern matching, with support for SQL wildcard characters (`%` and `_`).

For example, the following patterns return the same results:

`... LIKE '%testing%' ...` `... LIKE '%TESTING%' ...`

  
Default: No value (no filtering is applied to the output).

`IN MODEL _model_name_`
    

Specifies the identifier of the model that contains the versions to be listed. If the identifier contains spaces, special characters, or mixed-case characters, the entire identifier must be enclosed in double quotes. Identifiers enclosed in double quotes are also case-sensitive (e.g. `"My Object"`).

If the model identifier is not fully-qualified (in the form of `_db_name_._schema_name_._model_name_` or `_schema_name_._model_name_`), the command looks for the model in the current schema for the session.

## Output¶

The command output provides table properties and metadata in the following columns:

Column| Description  
---|---  
created_on| Date and time when the model version was created.  
name| Name of the version.  
aliases| Aliases of the model version, if any, including any you have assigned using [ALTER MODEL](/sql-reference/sql/alter-model) and any system aliases (DEFAULT, FIRST, or LAST) that apply. If a model version has no aliases, this column contains an empty ARRAY ([]).  
database_name| Database in which the version is stored.  
schema_name| Schema in which the version is stored.  
model_name| Name of the model that this version belongs to.  
is_default_version| Boolean value indicating whether this version is the model’s default version.  
functions| JSON array of the names of the functions available in this version.  
metadata| JSON object containing metadata as key-value pairs (`{}` if no metadata is specified).  
user_data| JSON object from the `user_data` section of the model definition manifest (`{}` if no user data is specified).  
  
## Usage notes¶

  * The command doesn’t require a running warehouse to execute.
  * The command only returns objects for which the current user’s current role has been granted at least one access privilege.
  * The MANAGE GRANTS access privilege implicitly allows its holder to see every object in the account. By default, only the account administrator (users with the ACCOUNTADMIN role) and security administrator (users with the SECURITYADMIN role) have the MANAGE GRANTS privilege.



  * To post-process the output of this command, you can use the [pipe operator](/sql-reference/operators-flow) (`->>`) or the [RESULT_SCAN](/sql-reference/functions/result_scan) function. Both constructs treat the output as a result set that you can query.

For example, you can use the pipe operator or RESULT_SCAN function to select specific columns from the SHOW command output or filter the rows.

When you refer to the output columns, use [double-quoted identifiers](/sql-reference/identifiers-syntax#label-delimited-identifier) for the column names. For example, to select the output column `type`, specify `SELECT "type"`.

You must use double-quoted identifiers because the output column names for SHOW commands are in lowercase. The double quotes ensure that the column names in the SELECT list or WHERE clause match the column names in the SHOW command output that was scanned.
