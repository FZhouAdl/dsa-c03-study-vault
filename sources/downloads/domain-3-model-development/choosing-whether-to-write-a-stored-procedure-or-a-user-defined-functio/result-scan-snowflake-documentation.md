---
title: "RESULT_SCAN | Snowflake Documentation"
source: https://docs.snowflake.com/sql-reference/functions/result_scan
cert_domain: domain-3-model-development
crawl_depth: 1
crawled: 2026-08-23
---

Categories:
    

[Table functions](/sql-reference/functions-table)

# RESULT_SCAN¶

Returns the result set of a previous command (within 24 hours of when you ran the query) as if the result was a table. This function is particularly useful if you want to process the output from any of the following operations:

  * [SHOW](/sql-reference/sql/show) or [DESC[RIBE]](/sql-reference/sql/desc) command that you ran.

  * Query that you ran on metadata or account usage information, such as [Snowflake Information Schema](/sql-reference/info-schema) or [Account Usage](/sql-reference/account-usage).

  * The result of a stored procedure that you [called](/sql-reference/sql/call).

As an alternative to using RESULT_SCAN, you can call a stored procedure that returns tabular data in the [FROM clause of a SELECT statement](/developer-guide/stored-procedure/stored-procedures-selecting-from).




The command or query can be from the current session or any of your other sessions, including past sessions, as long as the 24 hour period hasn’t elapsed. This period isn’t adjustable. For more information, see [Using Persisted Query Results](/user-guide/querying-persisted-results).

Tip

You can use the [pipe operator](/sql-reference/operators-flow) (`->>`) instead of this function to process the results of a previous command.

See also:
    

[DESCRIBE RESULT](/sql-reference/sql/desc-result) (Account & Session DDL)

## Syntax¶
[code] 
    RESULT_SCAN ( [ { '<query_id>' | <query_index>  | LAST_QUERY_ID() } ] )
    
[/code]

## Arguments¶

`'_query_id_ '` or `_query_index_` or `LAST_QUERY_ID()`
    

A specification of a query that you ran within the last 24 hours in any session, an integer index of a query in the current session, or the [LAST_QUERY_ID](/sql-reference/functions/last_query_id) function, which returns the ID of a query within your current session.

Snowflake query IDs are unique strings that resemble `01b71944-0001-b181-0000-0129032279f6`.

Query indexes are relative to the first query in the current session (if positive) or to the most recent query (if negative). For example, `RESULT_SCAN(-1)` is equivalent to `RESULT_SCAN(LAST_QUERY_ID())`.

This argument is optional. If it is omitted, the default is `RESULT_SCAN(-1)`, which returns the result set of the most recent command.

## Usage notes¶

  * If the original query was run manually, only the user who ran the original query can use the RESULT_SCAN function to process the output of the query. Even a user with the ACCOUNTADMIN privilege can’t access the results of another user’s query by calling RESULT_SCAN.

  * If the original query was run by using [a task](/user-guide/tasks-intro), the role that owns the task, instead of a specific user, triggered and ran the query. If a user or a task is operating with the same role, they can use RESULT_SCAN to access the query results.

  * Snowflake stores all query results for 24 hours. This function only returns results for queries that were run within this time period.

  * Result sets don’t have any metadata associated with them, so processing large results might be slower than if you were querying an actual table.

  * The query containing the RESULT_SCAN can include clauses, such as filters and ORDER BY clauses, that weren’t in the original query. You can use these clauses to narrow down or modify the result set.

  * A RESULT_SCAN isn’t guaranteed to return rows in the same order as the original query returned the rows. You can include an ORDER BY clause with the RESULT_SCAN query to specify a specific order.

  * To retrieve the ID for a specific query, use any of the following methods:

Snowsight:
    

In either of the following locations, click the provided link to display or copy the ID:

    * In **Worksheets** under **Projects** , after running a query, the **Query Details** include a link for the ID.
    * In **Query History** under **Monitoring** , each query includes the ID as a link.
SQL:
    

Call one of the following functions:

    * [QUERY_HISTORY , QUERY_HISTORY_BY_*](/sql-reference/functions/query_history) table function.

    * [LAST_QUERY_ID](/sql-reference/functions/last_query_id) function (if the query was run in the current session).

For example:
[code] SELECT LAST_QUERY_ID(-2);
          
[/code]

This is equivalent to using [LAST_QUERY_ID](/sql-reference/functions/last_query_id) as the input for RESULT_SCAN.

  * If RESULT_SCAN processes query output that contained duplicate column names (for example, a query that joined two tables that have overlapping column names), then RESULT_SCAN references the duplicate columns with modified names, appending `_1`, `_2`, and so on to the original name. For an example, see the following Examples section.

  * Timestamps in Parquet files that are queried by using the vectorized scanner sometimes display the time in a different time zone. Use the [CONVERT_TIMEZONE](/sql-reference/functions/convert_timezone) function to convert to a standard time zone for all timestamp data.




## Collation details¶

When `RESULT_SCAN` returns the results of the previous statement, `RESULT_SCAN` preserves the collation specification(s) of the values that it returns.

## Examples¶

The following examples use the RESULT_SCAN function.

### Simple examples¶

Retrieve all values greater than `1` from the result of your most recent query in the current session:
[code] 
    SELECT $1 AS value FROM VALUES (1), (2), (3);
    
[/code]
[code] 
    +-------+
    | VALUE |
    |-------|
    |     1 |
    |     2 |
    |     3 |
    +-------+
    
[/code]
[code] 
    SELECT * FROM TABLE(RESULT_SCAN(LAST_QUERY_ID())) WHERE value > 1;
    
[/code]
[code] 
    +-------+
    | VALUE |
    |-------|
    |     2 |
    |     3 |
    +-------+
    
[/code]

Retrieve all values from your second most recent query in the current session:
[code] 
    SELECT * FROM TABLE(RESULT_SCAN(LAST_QUERY_ID(-2)));
    
[/code]

Retrieve all values from your first query in the current session:
[code] 
    SELECT * FROM TABLE(RESULT_SCAN(LAST_QUERY_ID(1)));
    
[/code]

Retrieve the values from the `c2` column in the result of the specified query:
[code] 
    SELECT c2 FROM TABLE(RESULT_SCAN('ce6687a4-331b-4a57-a061-02b2b0f0c17c'));
    
[/code]

### Examples using DESCRIBE and SHOW commands¶

Process the result of a [DESCRIBE USER](/sql-reference/sql/desc-user) command to retrieve particular fields of interest, such as the user’s default role. Because the output column names from the DESC USER command were generated in lowercase, the commands use [double-quoted identifiers](/sql-reference/identifiers-syntax#label-delimited-identifier) for the column names in the query to ensure that the column names in the query match the column names in the output that was scanned.
[code] 
    DESC USER jessicajones;
    SELECT "property", "value" FROM TABLE(RESULT_SCAN(LAST_QUERY_ID()))
      WHERE "property" = 'DEFAULT_ROLE';
    
[/code]

Process the result of a [SHOW TABLES](/sql-reference/sql/show-tables) command to extract empty tables that are older than 21 days. The SHOW command generates lowercase column names, so the command quotes the names to use matching case:
[code] 
    SHOW TABLES;
    SELECT "database_name", "schema_name", "name" as "table_name", "rows", "created_on"
      FROM table(RESULT_SCAN(LAST_QUERY_ID()))
      WHERE "rows" = 0 AND "created_on" < DATEADD(day, -21, CURRENT_TIMESTAMP())
      ORDER BY "created_on";
    
[/code]

Process the result of a [SHOW TABLES](/sql-reference/sql/show-tables) command to extract the tables in descending order of size. The following example also shows how to use a UDF to show table size in a slightly more human-readable format:
[code] 
    -- Show byte counts with suffixes such as "KB", "MB", and "GB".
    CREATE OR REPLACE FUNCTION NiceBytes(NUMBER_OF_BYTES INTEGER)
    RETURNS VARCHAR
    AS
    $$
    CASE
      WHEN NUMBER_OF_BYTES < 1024
        THEN NUMBER_OF_BYTES::VARCHAR
      WHEN NUMBER_OF_BYTES >= 1024 AND NUMBER_OF_BYTES < 1048576
        THEN (NUMBER_OF_BYTES / 1024)::VARCHAR || 'KB'
      WHEN NUMBER_OF_BYTES >= 1048576 AND NUMBER_OF_BYTES < (POW(2, 30))
        THEN (NUMBER_OF_BYTES / 1048576)::VARCHAR || 'MB'
      ELSE
        (NUMBER_OF_BYTES / POW(2, 30))::VARCHAR || 'GB'
    END
    $$
    ;
    SHOW TABLES;
    -- Show all of my tables in descending order of size.
    SELECT "database_name", "schema_name", "name" as "table_name", NiceBytes("bytes") AS "size"
      FROM table(RESULT_SCAN(LAST_QUERY_ID()))
      ORDER BY "bytes" DESC;
    
[/code]

### Examples using a stored procedure¶

Stored procedure calls return a value. However, this value can’t be processed directly because you can’t embed a stored procedure call in another statement. To work around this limitation, you can use RESULT_SCAN to process the value returned by a stored procedure. A simplified example is below:

First, create a procedure that returns a “complicated” value (in this case, a string that contains JSON-compatible data) that can be processed after it has been returned from the CALL.
[code] 
    CREATE OR REPLACE PROCEDURE return_json()
      RETURNS VARCHAR
      LANGUAGE JavaScript
      AS
      $$
        return '{"keyA": "ValueA", "keyB": "ValueB"}';
      $$
      ;
    
[/code]

Call the procedure:
[code] 
    CALL return_json();
    
[/code]
[code] 
    +--------------------------------------+
    | RETURN_JSON                          |
    |--------------------------------------|
    | {"keyA": "ValueA", "keyB": "ValueB"} |
    +--------------------------------------+
    
[/code]

The next three steps extract the data from the result set.

Get the first (and only) column:
[code] 
    SELECT $1 AS output_col FROM table(RESULT_SCAN(LAST_QUERY_ID()));
    
[/code]
[code] 
    +--------------------------------------+
    | OUTPUT_COL                           |
    |--------------------------------------|
    | {"keyA": "ValueA", "keyB": "ValueB"} |
    +--------------------------------------+
    
[/code]

Convert the output from a VARCHAR value to a VARIANT value:
[code] 
    SELECT PARSE_JSON(output_col) AS json_col FROM TABLE(RESULT_SCAN(LAST_QUERY_ID()));
    
[/code]
[code] 
    +---------------------+
    | JSON_COL            |
    |---------------------|
    | {                   |
    |   "keyA": "ValueA", |
    |   "keyB": "ValueB"  |
    | }                   |
    +---------------------+
    
[/code]

Extract the value that corresponds to the key `keyB`:
[code] 
    SELECT json_col:keyB FROM TABLE(RESULT_SCAN(LAST_QUERY_ID()));
    
[/code]
[code] 
    +---------------+
    | JSON_COL:KEYB |
    |---------------|
    | "ValueB"      |
    +---------------+
    
[/code]

The following example shows a more compact way to extract the same data that was extracted in the previous example. This example has fewer statements, but is harder to read:
[code] 
    CALL return_json();
    
[/code]
[code] 
    +--------------------------------------+
    | RETURN_JSON                          |
    |--------------------------------------|
    | {"keyA": "ValueA", "keyB": "ValueB"} |
    +--------------------------------------+
    
[/code]
[code] 
    SELECT JSON_COL:keyB
     FROM (
          SELECT PARSE_JSON($1::VARIANT) AS json_col
            FROM TABLE(RESULT_SCAN(LAST_QUERY_ID()))
          );
    
[/code]
[code] 
    +---------------+
    | JSON_COL:KEYB |
    |---------------|
    | "ValueB"      |
    +---------------+
    
[/code]

The output from the CALL uses the function name as the column name. You can use that column name in the query. The following example shows one additional compact version, in which the column is referenced by name instead of the column number:
[code] 
    CALL return_json();
    
[/code]
[code] 
    +--------------------------------------+
    | RETURN_JSON                          |
    |--------------------------------------|
    | {"keyA": "ValueA", "keyB": "ValueB"} |
    +--------------------------------------+
    
[/code]
[code] 
    SELECT json_col:keyB
      FROM (
           SELECT PARSE_JSON(RETURN_JSON::VARIANT) AS json_col
             FROM TABLE(RESULT_SCAN(LAST_QUERY_ID()))
           );
    
[/code]
[code] 
    +---------------+
    | JSON_COL:KEYB |
    |---------------|
    | "ValueB"      |
    +---------------+
    
[/code]

### Example with duplicate column names¶

The following example shows that RESULT_SCAN effectively references alternate column names when there are duplicate column names in the original query:

Create two tables that have at least one column with the same name:
[code] 
    CREATE TABLE employees (id INT);
    
    CREATE TABLE dependents (id INT, employee_id INT);
    
[/code]

Load data into the two tables:
[code] 
    INSERT INTO employees (id) VALUES (11);
    
    INSERT INTO dependents (id, employee_id) VALUES (101, 11);
    
[/code]

Now run a query for which the output will contain two columns with the same name:
[code] 
    SELECT *
      FROM employees INNER JOIN dependents
        ON dependents.employee_ID = employees.id
      ORDER BY employees.id, dependents.id;
    
[/code]
[code] 
    +----+-----+-------------+
    | ID |  ID | EMPLOYEE_ID |
    |----+-----+-------------|
    | 11 | 101 |          11 |
    +----+-----+-------------+
    
[/code]

Now call RESULT_SCAN to process the results of that query. If different columns that have the same name in the results, RESULT_SCAN uses the original name for the first column and assigns the second column a modified name that is unique. To make the name unique, RESULT_SCAN appends the suffix `__n_` to the name, where `_n_` is the next number available that produces a name that is different from the names of the previous columns.
[code] 
    SELECT id, id_1, employee_id
      FROM TABLE(RESULT_SCAN(LAST_QUERY_ID()))
      WHERE id_1 = 101;
    
[/code]
[code] 
    +----+------+-------------+
    | ID | ID_1 | EMPLOYEE_ID |
    |----+------+-------------|
    | 11 |  101 |          11 |
    +----+------+-------------+
    
[/code]
