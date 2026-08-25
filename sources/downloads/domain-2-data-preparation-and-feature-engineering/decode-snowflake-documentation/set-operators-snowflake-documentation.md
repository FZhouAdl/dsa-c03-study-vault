---
title: "Set operators | Snowflake Documentation"
source: https://docs.snowflake.com/sql-reference/operators-query
cert_domain: domain-2-data-preparation-and-feature-engineering
crawl_depth: 1
crawled: 2026-08-23
---

# Set operators¶

Set operators combine the intermediate results of multiple query blocks into a single result set.

## General syntax¶
[code] 
    [ ( ] <query> [ ) ]
    {
      INTERSECT |
      { MINUS | EXCEPT } |
      UNION [ { DISTINCT | ALL } ] [ BY NAME ]
    }
    [ ( ] <query> [ ) ]
    [ ORDER BY ... ]
    [ LIMIT ... ]
    
[/code]

## General usage notes¶

  * Each query can itself contain query operators, so that you can combine multiple query expressions with set operators.

  * You can apply the [ORDER BY](/sql-reference/constructs/order-by) and [LIMIT / FETCH](/sql-reference/constructs/limit) clauses to the result of the set operator.

  * When using these operators:

    * Make sure that each query selects the same number of columns, with the exception of queries that include UNION BY NAME or UNION ALL BY NAME.

    * Make sure that the data type of each column is consistent across the rows from different sources. One of the examples in the Use the UNION operator and cast mismatched data types section illustrates the potential problem and solution when data types don’t match.

    * In general, make sure the “meanings,” as well as the data types, of the columns match. The following query with the UNION ALL operator won’t produce the desired results:
[code] SELECT LastName, FirstName FROM employees
          UNION ALL
          SELECT FirstName, LastName FROM contractors;
          
[/code]

The risk of error increases when you use an asterisk to specify all columns of a table, for example:
[code] SELECT * FROM table1
          UNION ALL
          SELECT * FROM table2;
          
[/code]

If the tables have the same number of columns, but the columns aren’t in the same order, the query results will probably be incorrect when you use these operators.

The UNION BY NAME and UNION ALL BY NAME operators are exceptions for this scenario. For example, the following query returns the correct results:
[code] SELECT LastName, FirstName FROM employees
          UNION ALL BY NAME
          SELECT FirstName, LastName FROM contractors;
          
[/code]

    * The names of the output columns are based on the names of the columns of the first query. For example, consider the following query:
[code] SELECT LastName, FirstName FROM employees
          UNION ALL
          SELECT FirstName, LastName FROM contractors;
          
[/code]

This query behaves as though the query were the following:
[code] SELECT LastName, FirstName FROM employees
          UNION ALL
          SELECT FirstName AS LastName, LastName AS FirstName FROM contractors;
          
[/code]

  * The precedence of the set operators matches the ANSI and ISO SQL standards:

    * The UNION [ALL] and MINUS (EXCEPT) operators have equal precedence.
    * The INTERSECT operator has higher precedence than UNION [ALL] and MINUS (EXCEPT).

Snowflake processes operators of equal precedence from left to right.

You can use parentheses to force the expressions to be evaluated in a different order.

Not all database vendors follow the ANSI/ISO standard for precedence of set operators. Snowflake recommends using parentheses to specify the order of evaluation, especially if you are porting code from another vendor to Snowflake, or writing code that you might execute on other databases as well as on Snowflake.




## Sample tables for examples¶

Some of the examples in this topic use the following sample tables. Both tables have a postal code column. One table records the postal code of each sales office, and the other records the postal code of each customer.
[code] 
    CREATE OR REPLACE TABLE sales_office_postal_example(
      office_name VARCHAR,
      postal_code VARCHAR);
    
    INSERT INTO sales_office_postal_example VALUES ('sales1', '94061');
    INSERT INTO sales_office_postal_example VALUES ('sales2', '94070');
    INSERT INTO sales_office_postal_example VALUES ('sales3', '98116');
    INSERT INTO sales_office_postal_example VALUES ('sales4', '98005');
    
    CREATE OR REPLACE TABLE customer_postal_example(
      customer VARCHAR,
      postal_code VARCHAR);
    
    INSERT INTO customer_postal_example VALUES ('customer1', '94066');
    INSERT INTO customer_postal_example VALUES ('customer2', '94061');
    INSERT INTO customer_postal_example VALUES ('customer3', '98444');
    INSERT INTO customer_postal_example VALUES ('customer4', '98005');
    
[/code]

## INTERSECT¶

Returns rows from one query’s result set which also appear in another query’s result set, with duplicate elimination.

### Syntax¶
[code] 
    [ ( ] <query> [ ) ]
    INTERSECT
    [ ( ] <query> [ ) ]
    
[/code]

### INTERSECT operator examples¶

To find the postal codes that are in both the `sales_office_postal_example` table and the `customer_postal_example` table, query the sample tables:
[code] 
    SELECT postal_code FROM sales_office_postal_example
    INTERSECT
    SELECT postal_code FROM customer_postal_example
    ORDER BY postal_code;
    
[/code]
[code] 
    +-------------+
    | POSTAL_CODE |
    |-------------|
    | 94061       |
    | 98005       |
    +-------------+
    
[/code]

## MINUS , EXCEPT¶

Returns the rows returned by the first query that aren’t also returned by the second query.

The MINUS and EXCEPT keywords have the same meaning and can be used interchangeably.

### Syntax¶
[code] 
    [ ( ] <query> [ ) ]
    MINUS
    [ ( ] <query> [ ) ]
    
    [ ( ] <query> [ ) ]
    EXCEPT
    [ ( ] <query> [ ) ]
    
[/code]

### MINUS operator examples¶

Query the sample tables to find the postal codes in the `sales_office_postal_example` table that aren’t also in the `customer_postal_example` table:
[code] 
    SELECT postal_code FROM sales_office_postal_example
    MINUS
    SELECT postal_code FROM customer_postal_example
    ORDER BY postal_code;
    
[/code]
[code] 
    +-------------+
    | POSTAL_CODE |
    |-------------|
    | 94070       |
    | 98116       |
    +-------------+
    
[/code]

Query the sample tables to find the postal codes in the `customer_postal_example` table that aren’t also in the `sales_office_postal_example` table:
[code] 
    SELECT postal_code FROM customer_postal_example
    MINUS
    SELECT postal_code FROM sales_office_postal_example
    ORDER BY postal_code;
    
[/code]
[code] 
    +-------------+
    | POSTAL_CODE |
    |-------------|
    | 94066       |
    | 98444       |
    +-------------+
    
[/code]

## UNION [ { DISTINCT | ALL } ] [ BY NAME ]¶

Combines the result sets from two queries:

  * UNION [ DISTINCT ] combines rows by column position with duplicate elimination.
  * UNION ALL combines rows by column position without duplicate elimination.
  * UNION [ DISTINCT ] BY NAME combines rows by column name with duplicate elimination.
  * UNION ALL BY NAME combines rows by column name without duplicate elimination.



The default is UNION DISTINCT (that is, combine rows by column position with duplicate elimination). The DISTINCT keyword is optional. The DISTINCT keyword and the ALL keyword are mutually exclusive.

Use UNION or UNION ALL when the column positions match in the tables that you are combining. Use UNION BY NAME or UNION ALL BY NAME for the following use cases:

  * The tables that you are combining have varying column orders.
  * The tables that you are combining have evolving schemas, where columns are added or reordered.
  * You want to combine subsets of columns that have different positions in the tables.



### Syntax¶
[code] 
    [ ( ] <query> [ ) ]
    UNION [ { DISTINCT | ALL } ] [ BY NAME ]
    [ ( ] <query> [ ) ]
    
[/code]

### Usage notes for the BY NAME clause¶

In addition to the general usage notes, the following usage notes apply to UNION BY NAME and UNION ALL BY NAME:

  * Columns with the same identifiers are matched and combined. Matching of unquoted identifiers is case-insensitive, and matching of quoted identifiers is case-sensitive.
  * The inputs aren’t required to have the same number of columns. If a column exists in one input but not the other, it is filled with NULL values in the combined result set for each row where it’s missing.
  * The order of columns in the combined result set is determined by the order of unique columns from left to right, as they are first encountered.



### UNION operator examples¶

The following examples use the UNION operator:

  * Combine the results from two queries by column position
  * Combine the results from two queries by column name
  * Use an alias to combine the results from two queries with different column names
  * Use the UNION operator and cast mismatched data types



#### Combine the results from two queries by column position¶

To combine the result sets by column position from two queries on the sample tables, use the UNION operator:
[code] 
    SELECT office_name office_or_customer, postal_code FROM sales_office_postal_example
    UNION
    SELECT customer, postal_code FROM customer_postal_example
    ORDER BY postal_code;
    
[/code]
[code] 
    +--------------------+-------------+
    | OFFICE_OR_CUSTOMER | POSTAL_CODE |
    |--------------------+-------------|
    | sales1             | 94061       |
    | customer2          | 94061       |
    | customer1          | 94066       |
    | sales2             | 94070       |
    | sales4             | 98005       |
    | customer4          | 98005       |
    | sales3             | 98116       |
    | customer3          | 98444       |
    +--------------------+-------------+
    
[/code]

#### Combine the results from two queries by column name¶

Create two tables with differing column order and insert data:
[code] 
    CREATE OR REPLACE TABLE union_demo_column_order1 (
      a INTEGER,
      b VARCHAR);
    
    INSERT INTO union_demo_column_order1 VALUES
      (1, 'one'),
      (2, 'two'),
      (3, 'three');
    
    CREATE OR REPLACE TABLE union_demo_column_order2 (
      B VARCHAR,
      A INTEGER);
    
    INSERT INTO union_demo_column_order2 VALUES
      ('three', 3),
      ('four', 4);
    
[/code]

To combine the result sets by column name from two queries, use the UNION BY NAME operator:
[code] 
    SELECT * FROM union_demo_column_order1
    UNION BY NAME
    SELECT * FROM union_demo_column_order2
    ORDER BY a;
    
[/code]
[code] 
    +---+-------+
    | A | B     |
    |---+-------|
    | 1 | one   |
    | 2 | two   |
    | 3 | three |
    | 4 | four  |
    +---+-------+
    
[/code]

The output shows that the query eliminated the duplicate row (with `3` in column `A` and `three` in column `B`).

To combine the tables without duplicate elimination, use the UNION ALL BY NAME operator:
[code] 
    SELECT * FROM union_demo_column_order1
    UNION ALL BY NAME
    SELECT * FROM union_demo_column_order2
    ORDER BY a;
    
[/code]
[code] 
    +---+-------+
    | A | B     |
    |---+-------|
    | 1 | one   |
    | 2 | two   |
    | 3 | three |
    | 3 | three |
    | 4 | four  |
    +---+-------+
    
[/code]

Notice that the cases of the column names don’t match in the two tables. The column names are lowercase in the `union_demo_column_order1` table and uppercase in the `union_demo_column_order2` table. If you run a query with quotation marks around the column names, an error is returned because the matching of quoted identifiers is case-sensitive. For example, the following query places quotation marks around the column names:
[code] 
    SELECT 'a', 'b' FROM union_demo_column_order1
    UNION ALL BY NAME
    SELECT 'B', 'A' FROM union_demo_column_order2
    ORDER BY a;
    
[/code]
[code] 
    000904 (42000): SQL compilation error: error line 4 at position 9
    invalid identifier 'A'
    
[/code]

#### Use an alias to combine the results from two queries with different column names¶

When you use the UNION BY NAME operator to combine the result sets by column name from two queries on the sample tables, the rows in the result set have NULL values because the column names don’t match:
[code] 
    SELECT office_name, postal_code FROM sales_office_postal_example
    UNION BY NAME
    SELECT customer, postal_code FROM customer_postal_example
    ORDER BY postal_code;
    
[/code]
[code] 
    +-------------+-------------+-----------+
    | OFFICE_NAME | POSTAL_CODE | CUSTOMER  |
    |-------------+-------------+-----------|
    | sales1      | 94061       | NULL      |
    | NULL        | 94061       | customer2 |
    | NULL        | 94066       | customer1 |
    | sales2      | 94070       | NULL      |
    | sales4      | 98005       | NULL      |
    | NULL        | 98005       | customer4 |
    | sales3      | 98116       | NULL      |
    | NULL        | 98444       | customer3 |
    +-------------+-------------+-----------+
    
[/code]

The output shows that columns with different identifiers aren’t combined and that rows have NULL values for columns that are in one table but not the other. The `postal_code` column is in both tables, so there are no NULL values in the output for the `postal_code` column.

The following query uses the alias `office_or_customer` so that columns with different names have the same name for the duration of the query:
[code] 
    SELECT office_name AS office_or_customer, postal_code FROM sales_office_postal_example
    UNION BY NAME
    SELECT customer AS office_or_customer, postal_code FROM customer_postal_example
    ORDER BY postal_code;
    
[/code]
[code] 
    +--------------------+-------------+
    | OFFICE_OR_CUSTOMER | POSTAL_CODE |
    |--------------------+-------------|
    | sales1             | 94061       |
    | customer2          | 94061       |
    | customer1          | 94066       |
    | sales2             | 94070       |
    | sales4             | 98005       |
    | customer4          | 98005       |
    | sales3             | 98116       |
    | customer3          | 98444       |
    +--------------------+-------------+
    
[/code]

#### Use the UNION operator and cast mismatched data types¶

This example demonstrates a potential issue with using the UNION operator when data types don’t match, then provides the solution.

Start by creating the tables and inserting some data:
[code] 
    CREATE OR REPLACE TABLE union_test1 (v VARCHAR);
    CREATE OR REPLACE TABLE union_test2 (i INTEGER);
    
    INSERT INTO union_test1 (v) VALUES ('Smith, Jane');
    INSERT INTO union_test2 (i) VALUES (42);
    
[/code]

Execute a UNION by column position operation with different data types (a VARCHAR value in `union_test1` and an INTEGER value in `union_test2`):
[code] 
    SELECT v FROM union_test1
    UNION
    SELECT i FROM union_test2;
    
[/code]

This query returns an error:
[code] 
    100038 (22018): Numeric value 'Smith, Jane' is not recognized
    
[/code]

Now use explicit casting to convert the inputs to a compatible type:
[code] 
    SELECT v::VARCHAR FROM union_test1
    UNION
    SELECT i::VARCHAR FROM union_test2;
    
[/code]
[code] 
    +-------------+
    | V::VARCHAR  |
    |-------------|
    | Smith, Jane |
    | 42          |
    +-------------+
    
[/code]
