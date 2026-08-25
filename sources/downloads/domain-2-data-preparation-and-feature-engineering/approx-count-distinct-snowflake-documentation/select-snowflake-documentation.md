---
title: "SELECT | Snowflake Documentation"
source: https://docs.snowflake.com/sql-reference/sql/select
cert_domain: domain-2-data-preparation-and-feature-engineering
crawl_depth: 1
crawled: 2026-08-23
---

# SELECT¶

SELECT can be used as either a statement or as a clause within other statements:

  * As a statement, the SELECT statement is the most commonly executed SQL statement; it queries the database and retrieves a set of rows.
  * As a clause, SELECT defines the set of columns returned by a query.



See also:
    

[Query syntax](/sql-reference/constructs)

## Syntax¶

The following sections describe the syntax for this command:

  * Selecting all columns
  * Selecting specific columns



### Selecting all columns¶
[code] 
    [ ... ]
    SELECT [ { ALL | DISTINCT } ]
           [ TOP <n> ]
           [{<object_name>|<alias>}.]*
    
           [ ILIKE '<pattern>' ]
    
           [ EXCLUDE
             {
               <col_name> | ( <col_name>, <col_name>, ... )
             }
           ]
    
           [ REPLACE
             {
               ( <expr> AS <col_name> [ , <expr> AS <col_name>, ... ] )
             }
           ]
    
           [ RENAME
             {
               <col_name> AS <col_alias>
               | ( <col_name> AS <col_alias>, <col_name> AS <col_alias>, ... )
             }
           ]
    
[/code]

You can specify the following combinations of keywords after SELECT *. The keywords must be in the order shown below:
[code] 
    SELECT * ILIKE ... REPLACE ...
    
[/code]
[code] 
    SELECT * ILIKE ... RENAME ...
    
[/code]
[code] 
    SELECT * ILIKE ... REPLACE ... RENAME ...
    
[/code]
[code] 
    SELECT * EXCLUDE ... REPLACE ...
    
[/code]
[code] 
    SELECT * EXCLUDE ... RENAME ...
    
[/code]
[code] 
    SELECT * EXCLUDE ... REPLACE ... RENAME ...
    
[/code]
[code] 
    SELECT * REPLACE ... RENAME ...
    
[/code]

### Selecting specific columns¶
[code] 
    [ ... ]
    SELECT [ { ALL | DISTINCT } ]
           [ TOP <n> ]
           {
             [{<object_name>|<alias>}.]<col_name>
             | [{<object_name>|<alias>}.]$<col_position>
             | <expr>
           }
           [ [ AS ] <col_alias> ]
           [ , ... ]
    [ ... ]
    
[/code]

A trailing comma is supported in a column list. For example, the following SELECT statement is supported:
[code] 
    SELECT emp_id,
           name,
           dept,
      FROM employees;
    
[/code]

For more information about SELECT as a statement and the other clauses within the statement, see [Query syntax](/sql-reference/constructs).

## Parameters¶

`ALL | DISTINCT`
    

Specifies whether to perform duplicate elimination on the result set:

  * `ALL` includes all values in the result set.
  * `DISTINCT` eliminates duplicate values from the result set.



Default: `ALL`

`TOP _n_`
    

Specifies the maximum number of results to return. See [TOP <n>](/sql-reference/constructs/top_n).

`_object_name_` or   
`_alias_`
    

Specifies the object identifier or object alias as defined in the [FROM](/sql-reference/constructs/from) clause.

`*`
    

The asterisk is shorthand to indicate that the output should include all columns of the specified object, or all columns of all objects if `*` is not qualified with an object name or alias. The columns are returned in the order shown by executing the [DESCRIBE](/sql-reference/sql/desc) command on the object.

When you specify `*`, you can also specify `ILIKE`, `EXCLUDE`, `REPLACE`, and `RENAME`:

`ILIKE '_pattern_ '`
    

Specifies that only the columns that match `_pattern_` should be included in the results.

In `_pattern_`, you can use the following SQL wildcards:

  * Use an underscore (`_`) to match any single character.
  * Use a percent sign (`%`) to match any sequence of zero or more characters.



To match a sequence anywhere within the column name, begin and end the pattern with `%`.

Matching is case-insensitive.

If no columns match the specified pattern, a compilation error occurs (`001080 (42601): ... SELECT with no columns`).

`EXCLUDE _col_name_`   
`EXCLUDE (_col_name_ , _col_name_ , ...)`
    

Specifies the columns that should be excluded from the results.

If you are selecting from multiple tables, use `SELECT _table_name_.*` to specify that you want to select all columns from a specific table, and specify the unqualified column name in `EXCLUDE`. For example:
[code]
    SELECT table_a.* EXCLUDE column_in_table_a ,
      table_b.* EXCLUDE column_in_table_b
      ...
    
[/code]

`REPLACE (_expr_ AS _col_name_ [ , _expr_ AS _col_name_ , ...] )`
    

Replaces the value of `_col_name_` with the value of the evaluated expression `_expr_`.

For example, to prepend the string `'DEPT-'` to the values in the `department_id` column, use:
[code]
    SELECT * REPLACE ('DEPT-' || department_id AS department_id) ...
    
[/code]

For `_col_name_`:

  * The column must exist and cannot be filtered out by `ILIKE` or `EXCEPT`.
  * You cannot specify the same column more than once in the list of replacements.
  * If the column is in multiple tables (for example, in both tables in a join), the statement fails with an “ambiguous column” error.



`_expr_` must evaluate to a single value.

`RENAME _col_name_ AS _col_alias_`   
`RENAME (_col_name_ AS _col_alias_ , _col_name_ AS _col_alias_ , ...)`
    

Specifies the column aliases that should be used in the results.

If you are selecting from multiple tables, use `SELECT _table_name_.*` to specify that you want to select all columns from a specific table, and specify the unqualified column name in `RENAME`. For example:
[code]
    SELECT table_a.* RENAME column_in_table_a AS col_alias_a,
      table_b.* RENAME column_in_table_b AS col_alias_b
      ...
    
[/code]

Note

When specifying a combination of keywords after `SELECT *`:

  * You cannot specify both `ILIKE` and `EXCLUDE`.

  * If you specify `EXCLUDE` with `RENAME` or `REPLACE`:

    * You must specify `EXCLUDE` before `RENAME` or `REPLACE`:
[code] SELECT * EXCLUDE col_a RENAME col_b AS alias_b ...
          
[/code]
[code] SELECT * EXCLUDE employee_id REPLACE ('DEPT-' || department_id AS department_id) ...
          
[/code]

    * You cannot specify the same column in `EXCLUDE` and `RENAME`.

  * If you specify `ILIKE` with `RENAME` or `REPLACE`, you must specify `ILIKE` first:
[code] SELECT * ILIKE '%id%' RENAME department_id AS department ...
        
[/code]
[code] SELECT * ILIKE '%id%' REPLACE ('DEPT-' || department_id AS department_id) ...
        
[/code]

  * If you specify `REPLACE` and `RENAME`:

    * You must specify `REPLACE` first:
[code] SELECT * REPLACE ('DEPT-' || department_id AS department_id) RENAME employee_id as employee ...
          
[/code]

    * You can specify the same column name in `REPLACE` and `RENAME`:
[code] SELECT * REPLACE ('DEPT-' || department_id AS department_id) RENAME department_id as department ...
          
[/code]




`_col_name_`
    

Specifies the column identifier as defined in the [FROM](/sql-reference/constructs/from) clause.

`$_col_position_`
    

Specifies the position of the column (1-based) as defined in the [FROM](/sql-reference/constructs/from) clause. If a column is referenced from a table, this number can’t exceed the maximum number of columns in the table.

`_expr_`
    

Specifies an expression, such as a mathematical expression, that evaluates to a specific value for any given row.

`[ AS ] _col_alias_`
    

Specifies the column alias assigned to the resulting expression. This is used as the display name in a top-level SELECT list, and the column name in an inline view.

Do not assign a column alias that is the same as the name of another column referenced in the query. For example, if you are selecting columns named `prod_id` and `product_id`, do not alias `prod_id` as `product_id`. See Error case: Specifying an alias that matches another column name.

## Usage notes¶

  * Aliases and identifiers are case-insensitive by default. To preserve case, enclose them within double quotes (`"`). For more information, see [Object identifiers](/sql-reference/identifiers).

  * Without an ORDER BY clause, the results returned by SELECT are an unordered set. Running the same query repeatedly against the same tables might result in a different output order every time. If order matters, use the `ORDER BY` clause.

  * SELECT can be used not only as an independent statement, but also as a clause in other statements, for example `INSERT INTO ... SELECT ...;`. SELECT can also be used in a [subquery](/user-guide/querying-subqueries) within a statement.

  * In many cases, when you use a column alias for an expression (i.e. `_expr_ AS _col_alias_`) in other parts of the same query (in JOIN, FROM, WHERE, GROUP BY, other column expressions, etc.), the expression is evaluated only once.

However, note that in some cases, the expression can be evaluated multiple times, which can result in different values for the alias used in different parts of the same query.




## Examples¶

A few simple examples are provided below.

  * Setting up the data for the examples
  * Examples of selecting all columns (SELECT *)
  * Examples of selecting specific columns (SELECT colname)



Many additional examples are included in other parts of the documentation, including the detailed descriptions of [Query syntax](/sql-reference/constructs).

For examples related to querying an event table (whose schema is predefined by Snowflake), refer to [Viewing log messages](/developer-guide/logging-tracing/logging-accessing-messages) and [Viewing trace data](/developer-guide/logging-tracing/tracing-accessing-events).

### Setting up the data for the examples¶

Some of the queries below use the following tables and data:

> 
[code]
>     CREATE TABLE employee_table (
>         employee_ID INTEGER,
>         last_name VARCHAR,
>         first_name VARCHAR,
>         department_ID INTEGER
>         );
>     
>     CREATE TABLE department_table (
>         department_ID INTEGER,
>         department_name VARCHAR
>         );
>     
[/code]
[code]
>     INSERT INTO employee_table (employee_ID, last_name, first_name, department_ID) VALUES
>         (101, 'Montgomery', 'Pat', 1),
>         (102, 'Levine', 'Terry', 2),
>         (103, 'Comstock', 'Dana', 2);
>     
>     INSERT INTO department_table (department_ID, department_name) VALUES
>         (1, 'Engineering'),
>         (2, 'Customer Support'),
>         (3, 'Finance');
>     
[/code]

### Examples of selecting all columns (SELECT *)¶

  * Selecting all columns in the table
  * Selecting all columns with names that match a pattern
  * Selecting all columns except one column
  * Selecting all columns except two or more columns
  * Selecting all columns and renaming one column
  * Selecting all columns and renaming multiple columns
  * Selecting all columns with names that match a pattern and renaming a column
  * Selecting all columns, excluding a column, and renaming multiple columns
  * Selecting all columns and replacing the value of a column
  * Selecting all columns, replacing the value of a column, and renaming the column
  * Selecting all columns with names that match a pattern and replacing the value in a column
  * Selecting all columns from multiple tables, excluding a column, and renaming a column



#### Selecting all columns in the table¶

This example shows how to select all columns in `employee_table`:
[code] 
    SELECT * FROM employee_table;
    
[/code]
[code] 
    +-------------+------------+------------+---------------+
    | EMPLOYEE_ID | LAST_NAME  | FIRST_NAME | DEPARTMENT_ID |
    |-------------+------------+------------+---------------|
    |         101 | Montgomery | Pat        |             1 |
    |         102 | Levine     | Terry      |             2 |
    |         103 | Comstock   | Dana       |             2 |
    +-------------+------------+------------+---------------+
    
[/code]

#### Selecting all columns with names that match a pattern¶

This example shows how to select all columns in `employee_table` with names that contain `id`:
[code] 
    SELECT * ILIKE '%id%' FROM employee_table;
    
[/code]
[code] 
    +-------------+---------------+
    | EMPLOYEE_ID | DEPARTMENT_ID |
    |-------------+---------------|
    |         101 |             1 |
    |         102 |             2 |
    |         103 |             2 |
    +-------------+---------------+
    
[/code]

#### Selecting all columns except one column¶

This example shows how to select all columns in `employee_table` except for the `department_id` column:
[code] 
    SELECT * EXCLUDE department_id FROM employee_table;
    
[/code]
[code] 
    +-------------+------------+------------+
    | EMPLOYEE_ID | LAST_NAME  | FIRST_NAME |
    |-------------+------------+------------|
    |         101 | Montgomery | Pat        |
    |         102 | Levine     | Terry      |
    |         103 | Comstock   | Dana       |
    +-------------+------------+------------+
    
[/code]

#### Selecting all columns except two or more columns¶

This example shows how to select all columns in `employee_table` except for the `department_id` and `employee_id` columns:
[code] 
    SELECT * EXCLUDE (department_id, employee_id) FROM employee_table;
    
[/code]
[code] 
    +------------+------------+
    | LAST_NAME  | FIRST_NAME |
    |------------+------------|
    | Montgomery | Pat        |
    | Levine     | Terry      |
    | Comstock   | Dana       |
    +------------+------------+
    
[/code]

#### Selecting all columns and renaming one column¶

This example shows how to select all columns in `employee_table` and rename the `department_id` column:
[code] 
    SELECT * RENAME department_id AS department FROM employee_table;
    
[/code]
[code] 
    +-------------+------------+------------+------------+
    | EMPLOYEE_ID | LAST_NAME  | FIRST_NAME | DEPARTMENT |
    |-------------+------------+------------+------------|
    |         101 | Montgomery | Pat        |          1 |
    |         102 | Levine     | Terry      |          2 |
    |         103 | Comstock   | Dana       |          2 |
    +-------------+------------+------------+------------+
    
[/code]

#### Selecting all columns and renaming multiple columns¶

This example shows how to select all columns in `employee_table` and rename the `department_id` and `employee_id` columns:
[code] 
    SELECT * RENAME (department_id AS department, employee_id AS id) FROM employee_table;
    
[/code]
[code] 
    +-----+------------+------------+------------+
    |  ID | LAST_NAME  | FIRST_NAME | DEPARTMENT |
    |-----+------------+------------+------------|
    | 101 | Montgomery | Pat        |          1 |
    | 102 | Levine     | Terry      |          2 |
    | 103 | Comstock   | Dana       |          2 |
    +-----+------------+------------+------------+
    
[/code]

#### Selecting all columns, excluding a column, and renaming multiple columns¶

This example shows how to select all columns in `employee_table`, exclude the `first_name` column, and rename the `department_id` and `employee_id` columns:
[code] 
    SELECT * EXCLUDE first_name RENAME (department_id AS department, employee_id AS id) FROM employee_table;
    
[/code]
[code] 
    +-----+------------+------------+
    |  ID | LAST_NAME  | DEPARTMENT |
    |-----+------------+------------|
    | 101 | Montgomery |          1 |
    | 102 | Levine     |          2 |
    | 103 | Comstock   |          2 |
    +-----+------------+------------+
    
[/code]

#### Selecting all columns with names that match a pattern and renaming a column¶

This example shows how to select all columns in `employee_table` with names that contain `id` and rename the `department_id` column:
[code] 
    SELECT * ILIKE '%id%' RENAME department_id AS department FROM employee_table;
    
[/code]
[code] 
    +-------------+------------+
    | EMPLOYEE_ID | DEPARTMENT |
    |-------------+------------|
    |         101 |          1 |
    |         102 |          2 |
    |         103 |          2 |
    +-------------+------------+
    
[/code]

#### Selecting all columns and replacing the value of a column¶

This example shows how to select all columns in `employee_table` and replace the value in the `department_id` column with the ID prepended with `DEPT-`:
[code] 
    SELECT * REPLACE ('DEPT-' || department_id AS department_id) FROM employee_table;
    
[/code]
[code] 
    +-------------+------------+------------+---------------+
    | EMPLOYEE_ID | LAST_NAME  | FIRST_NAME | DEPARTMENT_ID |
    |-------------+------------+------------+---------------|
    |         101 | Montgomery | Pat        | DEPT-1        |
    |         102 | Levine     | Terry      | DEPT-2        |
    |         103 | Comstock   | Dana       | DEPT-2        |
    +-------------+------------+------------+---------------+
    
[/code]

#### Selecting all columns, replacing the value of a column, and renaming the column¶

This example shows how to select all columns in `employee_table`, replace the value in the `department_id` column with the ID prepended with `DEPT-`, and rename the column:
[code] 
    SELECT * REPLACE ('DEPT-' || department_id AS department_id) RENAME department_id AS department FROM employee_table;
    
[/code]
[code] 
    +-------------+------------+------------+------------+
    | EMPLOYEE_ID | LAST_NAME  | FIRST_NAME | DEPARTMENT |
    |-------------+------------+------------+------------|
    |         101 | Montgomery | Pat        | DEPT-1     |
    |         102 | Levine     | Terry      | DEPT-2     |
    |         103 | Comstock   | Dana       | DEPT-2     |
    +-------------+------------+------------+------------+
    
[/code]

#### Selecting all columns with names that match a pattern and replacing the value in a column¶

This example shows how to select all columns in `employee_table` with names that contain `id` and prepending `DEPT-` to the values in the `department_id` column:
[code] 
    SELECT * ILIKE '%id%' REPLACE('DEPT-' || department_id AS department_id) FROM employee_table;
    
[/code]
[code] 
    +-------------+---------------+
    | EMPLOYEE_ID | DEPARTMENT_ID |
    |-------------+---------------|
    |         101 | DEPT-1        |
    |         102 | DEPT-2        |
    |         103 | DEPT-2        |
    +-------------+---------------+
    
[/code]

#### Selecting all columns from multiple tables, excluding a column, and renaming a column¶

This example joins two tables and selects all columns from both tables except one column from `employee_table`. The example also renames one of the columns selected from `department_table`.
[code] 
    SELECT
      employee_table.* EXCLUDE department_id,
      department_table.* RENAME department_name AS department
    FROM employee_table INNER JOIN department_table
      ON employee_table.department_id = department_table.department_id
    ORDER BY department, last_name, first_name;
    
[/code]
[code] 
    +-------------+------------+------------+---------------+------------------+
    | EMPLOYEE_ID | LAST_NAME  | FIRST_NAME | DEPARTMENT_ID | DEPARTMENT       |
    |-------------+------------+------------+---------------+------------------|
    |         103 | Comstock   | Dana       |             2 | Customer Support |
    |         102 | Levine     | Terry      |             2 | Customer Support |
    |         101 | Montgomery | Pat        |             1 | Engineering      |
    +-------------+------------+------------+---------------+------------------+
    
[/code]

### Examples of selecting specific columns (SELECT colname)¶

  * Selecting a single column by name
  * Selecting multiple columns by name from joined tables
  * Selecting a column by position
  * Specifying an alias for a column in the output
  * Error case: Specifying an alias that matches another column name



#### Selecting a single column by name¶

This example shows how to look up an employee’s last name if you know their ID.
[code] 
    SELECT last_name FROM employee_table WHERE employee_ID = 101;
    +------------+
    | LAST_NAME  |
    |------------|
    | Montgomery |
    +------------+
    
[/code]

#### Selecting multiple columns by name from joined tables¶

This example lists each employee and the name of the department that each employee works in. The output is in order by department name, and within each department the employees are in order by name. This query uses a join to relate the information in one table to the information in another table.
[code] 
    SELECT department_name, last_name, first_name
        FROM employee_table INNER JOIN department_table
            ON employee_table.department_ID = department_table.department_ID
        ORDER BY department_name, last_name, first_name;
    +------------------+------------+------------+
    | DEPARTMENT_NAME  | LAST_NAME  | FIRST_NAME |
    |------------------+------------+------------|
    | Customer Support | Comstock   | Dana       |
    | Customer Support | Levine     | Terry      |
    | Engineering      | Montgomery | Pat        |
    +------------------+------------+------------+
    
[/code]

#### Selecting a column by position¶

This example shows how to use `$` to specify a column by column number, rather than by column name:
[code] 
    SELECT $2 FROM employee_table ORDER BY $2;
    +------------+
    | $2         |
    |------------|
    | Comstock   |
    | Levine     |
    | Montgomery |
    +------------+
    
[/code]

#### Specifying an alias for a column in the output¶

This example shows that the output columns do not need to be taken directly from the tables in the `FROM` clause; the output columns can be general expressions. This example calculates the area of a circle that has a radius of 2.0. This example also shows how to use a column alias so that the output has a meaningful column name:
[code] 
    SELECT pi() * 2.0 * 2.0 AS area_of_circle;
    +----------------+
    | AREA_OF_CIRCLE |
    |----------------|
    |   12.566370614 |
    +----------------+
    
[/code]

#### Error case: Specifying an alias that matches another column name¶

This example demonstrates why it is not recommended to use a column alias that matches the name of another column that is used in the query. This GROUP BY query results in a SQL compiler error, not an ambiguous column error. The alias `prod_id` that is assigned to `product_id` in `table1` matches the name of the `prod_id` column in `table2`. The simplest solution to this error is to give the column a different alias.
[code] 
    CREATE OR REPLACE TABLE table1 (product_id NUMBER);
    
    CREATE OR REPLACE TABLE table2 (prod_id NUMBER);
    
    SELECT t1.product_id AS prod_id, t2.prod_id
      FROM table1 AS t1 JOIN table2 AS t2
        ON t1.product_id=t2.prod_id
      GROUP BY prod_id, t2.prod_id;
    
[/code]
[code] 
    001104 (42601): SQL compilation error: error line 1 at position 7
    'T1.PRODUCT_ID' in select clause is neither an aggregate nor in the group by clause.
    
[/code]
