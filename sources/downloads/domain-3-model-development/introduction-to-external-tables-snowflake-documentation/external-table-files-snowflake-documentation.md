---
title: "EXTERNAL_TABLE_FILES | Snowflake Documentation"
source: https://docs.snowflake.com/sql-reference/functions/external_table_files
cert_domain: domain-3-model-development
crawl_depth: 1
crawled: 2026-08-23
---

Categories:
    

[Information Schema](/sql-reference/info-schema#label-info-schema-functions) , [Table functions](/sql-reference/functions-table)

# EXTERNAL_TABLE_FILES¶

This table function can be used to query information about the staged data files included in the metadata for a specified [external table](/user-guide/tables-external-intro).

## Syntax¶
[code] 
    EXTERNAL_TABLE_FILES(
          TABLE_NAME => '<string>' )
    
[/code]

## Arguments¶

**Required:**

`TABLE_NAME => '_string_ '`
    

A string specifying an external table name.

## Usage notes¶

  * Returns results for the external table owner (i.e. the role with the OWNERSHIP privilege on the external table), or a higher role, or a role that has the USAGE privilege on the database and schema that contain an external table and any privilege on the external table.
  * The table function cannot retrieve metadata about staged data files until the external table is refreshed (i.e. synched) to include the data files in its metadata.
  * When calling an Information Schema table function, the session must have an INFORMATION_SCHEMA schema in use or the function name must be fully-qualified. For more details, see [Snowflake Information Schema](/sql-reference/info-schema).



## Output¶

The function returns the following columns:

Column Name| Data Type| Description  
---|---|---  
FILE_NAME| TEXT| Name of source file and relative path to the staged file  
REGISTERED_ON| TIMESTAMP_LTZ| Timestamp when the file metadata was added to an external table (i.e. when the external table metadata was refreshed with the file details)  
FILE_SIZE| NUMBER| Size of the file (in bytes)  
LAST_MODIFIED| TIMESTAMP_LTZ| Timestamp when the file was last updated in the stage  
ETAG| HEX| ETag header for the file  
MD5| HEX| MD5 checksum for the file  
  
## Examples¶

Retrieve the metadata stored for all data files referenced by the `mytable` external table:

> 
[code]
>     select *
>     from table(information_schema.external_table_files(TABLE_NAME=>'MYTABLE'));
>     
[/code]
