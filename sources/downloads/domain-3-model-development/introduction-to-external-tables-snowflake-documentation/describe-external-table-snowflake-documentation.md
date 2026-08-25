---
title: "DESCRIBE EXTERNAL TABLE | Snowflake Documentation"
source: https://docs.snowflake.com/sql-reference/sql/desc-external-table
cert_domain: domain-3-model-development
crawl_depth: 1
crawled: 2026-08-23
---

# DESCRIBE EXTERNAL TABLE¶

Describes the VALUE column and virtual columns in an external table.

DESCRIBE can be abbreviated to DESC.

See also:
    

[DROP EXTERNAL TABLE](/sql-reference/sql/drop-external-table) , [ALTER EXTERNAL TABLE](/sql-reference/sql/alter-external-table) , [CREATE EXTERNAL TABLE](/sql-reference/sql/create-external-table) , [SHOW EXTERNAL TABLES](/sql-reference/sql/show-external-tables)

[DESCRIBE VIEW](/sql-reference/sql/desc-view)

## Syntax¶
[code] 
    DESC[RIBE] [ EXTERNAL ] TABLE <name> [ TYPE =  { COLUMNS | STAGE } ]
    
[/code]

## Parameters¶

`_name_`
    

Specifies the identifier for the external table to describe. If the identifier contains spaces or special characters, the entire string must be enclosed in double quotes. Identifiers enclosed in double quotes are also case sensitive.

## Usage notes¶

  * To post-process the output of this command, you can use the [pipe operator](/sql-reference/operators-flow) (`->>`) or the [RESULT_SCAN](/sql-reference/functions/result_scan) function. Both constructs treat the output as a result set that you can query.

For example, you can use the pipe operator or RESULT_SCAN function to select specific columns from the SHOW command output or filter the rows.

When you refer to the output columns, use [double-quoted identifiers](/sql-reference/identifiers-syntax#label-delimited-identifier) for the column names. For example, to select the output column `type`, specify `SELECT "type"`.

You must use double-quoted identifiers because the output column names for SHOW commands are in lowercase. The double quotes ensure that the column names in the SELECT list or WHERE clause match the column names in the SHOW command output that was scanned.




## Examples¶

Create an example external table:

> 
[code]
>     CREATE EXTERNAL TABLE emp ( ... );
>     
[/code]

Describe the columns in the table:

> 
[code]
>     DESC EXTERNAL TABLE emp;
>     
[/code]
