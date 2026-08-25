---
title: "Logical operators | Snowflake Documentation"
source: https://docs.snowflake.com/sql-reference/operators-logical
cert_domain: domain-3-model-development
crawl_depth: 1
crawled: 2026-08-23
---

# Logical operators¶

Logical operators return the result of a particular Boolean operation on one or two input expressions. Logical operators are also referred to as Boolean operators.

Logical operators can only be used as a predicate (for example, in the [WHERE](/sql-reference/constructs/where) clause). Input expressions must be predicates.

See also:
    

[BOOLAND](/sql-reference/functions/booland) , [BOOLNOT](/sql-reference/functions/boolnot) , [BOOLOR](/sql-reference/functions/boolor) , [BOOLXOR](/sql-reference/functions/boolxor)

## List of logical operators¶

Operator| Syntax example| Description  
---|---|---  
`AND`| `a AND b`| Matches both expressions (`a` and `b`).  
`NOT`| `NOT a`| Doesn’t match the expression.  
`OR`| `a OR b`| Matches either expression.  
  
The order of precedence of these operators is shown below (from highest to lowest):

  * NOT
  * AND
  * OR



## Examples¶

The following examples use logical operators:

  * Use logical operators in queries on table data
  * Use logical operators in queries on Boolean values
  * Show “truth tables” for the logical operators



### Use logical operators in queries on table data¶

Create a table and insert data:
[code] 
    CREATE OR REPLACE TABLE logical_test1 (id INT, a INT, b VARCHAR);
    
    INSERT INTO logical_test1 (id, a, b) VALUES (1, 8, 'Up');
    INSERT INTO logical_test1 (id, a, b) VALUES (2, 25, 'Down');
    INSERT INTO logical_test1 (id, a, b) VALUES (3, 15, 'Down');
    INSERT INTO logical_test1 (id, a, b) VALUES (4, 47, 'Up');
    
    SELECT * FROM logical_test1;
    
[/code]
[code] 
    +----+----+------+
    | ID |  A | B    |
    |----+----+------|
    |  1 |  8 | Up   |
    |  2 | 25 | Down |
    |  3 | 15 | Down |
    |  4 | 47 | Up   |
    +----+----+------+
    
[/code]

#### Execute queries that use a single logical operator¶

Use a single logical operator in the WHERE clause of various queries:
[code] 
    SELECT *
      FROM logical_test1
      WHERE a > 20 AND
            b = 'Down';
    
[/code]
[code] 
    +----+----+------+
    | ID |  A | B    |
    |----+----+------|
    |  2 | 25 | Down |
    +----+----+------+
    
[/code]
[code] 
    SELECT *
      FROM logical_test1
      WHERE a > 20 OR
            b = 'Down';
    
[/code]
[code] 
    +----+----+------+
    | ID |  A | B    |
    |----+----+------|
    |  2 | 25 | Down |
    |  3 | 15 | Down |
    |  4 | 47 | Up   |
    +----+----+------+
    
[/code]
[code] 
    SELECT *
      FROM logical_test1
      WHERE a > 20 OR
            b = 'Up';
    
[/code]
[code] 
    +----+----+------+
    | ID |  A | B    |
    |----+----+------|
    |  1 |  8 | Up   |
    |  2 | 25 | Down |
    |  4 | 47 | Up   |
    +----+----+------+
    
[/code]
[code] 
    SELECT *
      FROM logical_test1
      WHERE NOT a > 20;
    
[/code]
[code] 
    +----+----+------+
    | ID |  A | B    |
    |----+----+------|
    |  1 |  8 | Up   |
    |  3 | 15 | Down |
    +----+----+------+
    
[/code]

#### Show the precedence of logical operators¶

The following examples show the precedence of the logical operators.

The first example shows that the precedence of AND is higher than the precedence of OR. The query returns the rows that match these conditions:

  * `b` equals `Down`.



OR

  * `a` equals `8` AND `b` equals `Up`.


[code] 
    SELECT *
      FROM logical_test1
      WHERE b = 'Down' OR
            a = 8 AND b = 'Up';
    
[/code]
[code] 
    +----+----+------+
    | ID |  A | B    |
    |----+----+------|
    |  1 |  8 | Up   |
    |  2 | 25 | Down |
    |  3 | 15 | Down |
    +----+----+------+
    
[/code]

You can use parentheses in the WHERE clause to change the precedence. For example, the following query returns the rows that match these conditions:

  * `b` equals `Down` OR `a` equals `8`.



AND

  * `b` equals `Up`.


[code] 
    SELECT *
      FROM logical_test1
      WHERE (b = 'Down' OR a = 8) AND b = 'Up';
    
[/code]
[code] 
    +----+---+----+
    | ID | A | B  |
    |----+---+----|
    |  1 | 8 | Up |
    +----+---+----+
    
[/code]

The next example shows that the precedence of NOT is higher than the precedence of AND. For example, the following query returns the rows that match these conditions:

  * `a` does NOT equal _15_.



AND

  * `b` equals `Down`.


[code] 
    SELECT *
      FROM logical_test1
      WHERE NOT a = 15 AND b = 'Down';
    
[/code]
[code] 
    +----+----+------+
    | ID |  A | B    |
    |----+----+------|
    |  2 | 25 | Down |
    +----+----+------+
    
[/code]

You can use parentheses in the WHERE clause to change the precedence. For example, the following query returns the rows that do NOT match both of these conditions:

  * `a` equals _15_.



AND

  * `b` equals `Down`.


[code] 
    SELECT *
      FROM logical_test1
      WHERE NOT (a = 15 AND b = 'Down');
    
[/code]
[code] 
    +----+----+------+
    | ID |  A | B    |
    |----+----+------|
    |  1 |  8 | Up   |
    |  2 | 25 | Down |
    |  4 | 47 | Up   |
    +----+----+------+
    
[/code]

### Use logical operators in queries on Boolean values¶

Create a table and insert data:
[code] 
    CREATE OR REPLACE TABLE logical_test2 (a BOOLEAN, b BOOLEAN);
    
    INSERT INTO logical_test2 VALUES (0, 1);
    
    SELECT * FROM logical_test2;
    
[/code]
[code] 
    +-------+------+
    | A     | B    |
    |-------+------|
    | False | True |
    +-------+------+
    
[/code]

The following query uses the OR operator to return rows where either `a` or `b` is TRUE:
[code] 
    SELECT a, b FROM logical_test2 WHERE a OR b;
    
[/code]
[code] 
    +-------+------+
    | A     | B    |
    |-------+------|
    | False | True |
    +-------+------+
    
[/code]

The following query uses the AND operator to return rows where both `a` and `b` are both TRUE:
[code] 
    SELECT a, b FROM logical_test2 WHERE a AND b;
    
[/code]
[code] 
    +---+---+
    | A | B |
    |---+---|
    +---+---+
    
[/code]

The following query uses the AND operator and the NOT operator to return rows where `b` is TRUE and `a` is FALSE:
[code] 
    SELECT a, b FROM logical_test2 WHERE b AND NOT a;
    
[/code]
[code] 
    +-------+------+
    | A     | B    |
    |-------+------|
    | False | True |
    +-------+------+
    
[/code]

The following query uses the AND operator and the NOT operator to return rows where `a` is TRUE and `b` is FALSE:
[code] 
    SELECT a, b FROM logical_test2 WHERE a AND NOT b;
    
[/code]
[code] 
    +---+---+
    | A | B |
    |---+---|
    +---+---+
    
[/code]

### Show “truth tables” for the logical operators¶

The next few examples show “truth tables” for the logical operators on a Boolean column. For more information about the behavior of Boolean values in Snowflake, see [Ternary logic](/sql-reference/ternary-logic).

Create a new table and data:
[code] 
    CREATE OR REPLACE TABLE logical_test3 (x BOOLEAN);
    
    INSERT INTO logical_test3 (x) VALUES
      (False),
      (True),
      (NULL);
    
[/code]

This shows the truth table for the OR operator:
[code] 
    SELECT x AS "OR",
           x OR False AS "FALSE",
           x OR True AS "TRUE",
           x OR NULL AS "NULL"
      FROM logical_test3;
    
[/code]
[code] 
    +-------+-------+------+------+
    | OR    | FALSE | TRUE | NULL |
    |-------+-------+------+------|
    | False | False | True | NULL |
    | True  | True  | True | True |
    | NULL  | NULL  | True | NULL |
    +-------+-------+------+------+
    
[/code]

This shows the truth table for the AND operator:
[code] 
    SELECT x AS "AND",
           x AND False AS "FALSE",
           x AND True AS "TRUE",
           x AND NULL AS "NULL"
      FROM logical_test3;
    
[/code]
[code] 
    +-------+-------+-------+-------+
    | AND   | FALSE | TRUE  | NULL  |
    |-------+-------+-------+-------|
    | False | False | False | False |
    | True  | False | True  | NULL  |
    | NULL  | False | NULL  | NULL  |
    +-------+-------+-------+-------+
    
[/code]
