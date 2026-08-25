---
title: "USE WAREHOUSE | Snowflake Documentation"
source: https://docs.snowflake.com/sql-reference/sql/use-warehouse
cert_domain: domain-3-model-development
crawl_depth: 1
crawled: 2026-08-23
---

# USE WAREHOUSE¶

Specifies the active/current [virtual warehouse](/user-guide/warehouses-overview) for the session. You must specify a warehouse for a session, and the warehouse must be running before you can execute queries and DML statements in the session.

To view the current warehouse for a session, call the [CURRENT_WAREHOUSE](/sql-reference/functions/current_warehouse) context function.

See also:
    

[ALTER WAREHOUSE](/sql-reference/sql/alter-warehouse) , [CREATE WAREHOUSE](/sql-reference/sql/create-warehouse) , [SHOW WAREHOUSES](/sql-reference/sql/show-warehouses)

## Syntax¶
[code] 
    USE WAREHOUSE <name>
    
[/code]

## Parameters¶

`_name_`
    

Specifies the identifier for the warehouse to use for the session. If the identifier contains spaces or special characters, the entire string must be enclosed in double quotes. Identifiers enclosed in double quotes are also case-sensitive.

## Examples¶

The following example specifies the warehouse where the current session performs its work:
[code] 
    USE WAREHOUSE mywarehouse;
    
[/code]

The following example changes from one warehouse to another, then back to the original warehouse. The name of the original warehouse is stored in a variable. Run the following commands:
[code] 
    SELECT CURRENT_WAREHOUSE();
    SET original_warehouse = (SELECT CURRENT_WAREHOUSE());
    USE WAREHOUSE warehouse_two;
    SELECT CURRENT_WAREHOUSE();
    USE WAREHOUSE IDENTIFIER($original_warehouse);
    SELECT CURRENT_WAREHOUSE();
    
[/code]

The output for these commands shows how the current warehouse value changes:
[code] 
    >SELECT CURRENT_WAREHOUSE();
    +---------------------+
    | WAREHOUSE_ONE       |
    +---------------------+
    
    >SET original_warehouse = (SELECT CURRENT_WAREHOUSE());
    
    >USE WAREHOUSE warehouse_two;
    >SELECT CURRENT_WAREHOUSE();
    +---------------------+
    | WAREHOUSE_TWO       |
    +---------------------+
    
    >USE WAREHOUSE IDENTIFIER($original_warehouse);
    >SELECT CURRENT_WAREHOUSE();
    +---------------------+
    | WAREHOUSE_ONE       |
    +---------------------+
    
[/code]
