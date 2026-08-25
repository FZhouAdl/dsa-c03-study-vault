---
title: "ROLLBACK | Snowflake Documentation"
source: https://docs.snowflake.com/sql-reference/sql/rollback
cert_domain: domain-3-model-development
crawl_depth: 1
crawled: 2026-08-23
---

# ROLLBACK¶

Rolls back an open transaction in the current session.

See also:
    

[BEGIN](/sql-reference/sql/begin) , [COMMIT](/sql-reference/sql/commit) , [SHOW TRANSACTIONS](/sql-reference/sql/show-transactions) , [DESCRIBE TRANSACTION](/sql-reference/sql/desc-transaction)

## Syntax¶
[code] 
    ROLLBACK [ WORK ]
    
[/code]

## Parameters¶

`WORK`
    

Optional keyword that provides compatibility with other database systems.

## Examples¶

Begin a transaction, insert some values into a table, and then complete the transaction by rolling back the changes made in the transaction:
[code] 
    SELECT COUNT(*) FROM A1;
    
    +----------+
    | COUNT(*) |
    |----------+
    |        0 |
    +----------+
    
    BEGIN NAME T4;
    
    SELECT CURRENT_TRANSACTION();
    
    +-----------------------+
    | CURRENT_TRANSACTION() |
    |-----------------------+
    | 1432071523422         |
    +-----------------------+
    
    INSERT INTO A1 VALUES (1), (2);
    
    +-------------------------+
    | number of rows inserted |
    |-------------------------+
    | 2                       |
    +-------------------------+
    
    ROLLBACK;
    
    SELECT COUNT(*) FROM A1;
    
    +----------+
    | COUNT(*) |
    |----------+
    |        0 |
    +----------+
    
    SELECT CURRENT_TRANSACTION();
    
    +-----------------------+
    | CURRENT_TRANSACTION() |
    |-----------------------+
    | [NULL]                |
    +-----------------------+
    
    SELECT LAST_TRANSACTION();
    
    +--------------------+
    | LAST_TRANSACTION() |
    |--------------------+
    | 1432071523422      |
    +--------------------+
    
[/code]
