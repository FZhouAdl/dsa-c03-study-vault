---
title: "HLL_ACCUMULATE | Snowflake Documentation"
source: https://docs.snowflake.com/sql-reference/functions/hll_accumulate
cert_domain: domain-2-data-preparation-and-feature-engineering
crawl_depth: 1
crawled: 2026-08-23
---

Categories:
    

[Aggregate functions](/sql-reference/functions-aggregation) (Cardinality Estimation) , [Window functions](/sql-reference/functions-window-syntax) (Cardinality Estimation)

# HLL_ACCUMULATE¶

Returns the HyperLogLog state at the end of aggregation.

For more information about HyperLogLog, see [Estimating the Number of Distinct Values](/user-guide/querying-approximate-cardinality).

[HLL](/sql-reference/functions/hll) discards its intermediate state when the final cardinality estimate is returned. In advanced use cases, such as incremental cardinality estimation during bulk loading, one may want to keep the intermediate state. The intermediate state can later be combined (merged) with other intermediate states, or can be exported to external tools.

In contrast to [HLL](/sql-reference/functions/hll), HLL_ACCUMULATE does not return a cardinality estimate. Instead, it skips the final estimation step and returns the algorithm state itself. The state is a binary of at most 4096 Bytes. For more information, see [Estimating the Number of Distinct Values](/user-guide/querying-approximate-cardinality).

See also:
    

[HLL_COMBINE](/sql-reference/functions/hll_combine) , [HLL_ESTIMATE](/sql-reference/functions/hll_estimate)

## Syntax¶

**Aggregate function**
[code] 
    HLL_ACCUMULATE( [ DISTINCT ] <expr> )
    
    HLL_ACCUMULATE(*)
    
[/code]

**Window function**
[code] 
    HLL_ACCUMULATE( [ DISTINCT ] <expr> ) OVER ( [ PARTITION BY <expr1> ] )
    
    HLL_ACCUMULATE(*) OVER ( [ PARTITION BY <expr1> ] )
    
[/code]

For details about the OVER clause, see [Window function syntax and usage](/sql-reference/functions-window-syntax).

## Arguments¶

`_expr_`
    

The expression for which you want to estimate cardinality (number of distinct values). This is typically a column name, but can be a more general expression.

## Usage notes¶

  * This function can be used as an [aggregate function](/sql-reference/functions-aggregation) or a [window function](/sql-reference/functions-window-syntax).
  * DISTINCT is supported syntactically, but has no effect.



## Examples¶

This shows one step towards estimating the number of distinct postal codes in province(s) of Canada. In this step, we calculate the approximate number of distinct postal codes in Manitoba and store an internal representation of the “state” of the calculation, which we can later combine with similar information for other provinces:
[code] 
    CREATE TABLE temporary_hll_state_for_manitoba AS
      SELECT HLL_ACCUMULATE(postal_code) AS h_a_p_c
        FROM postal_data
        WHERE province = 'Manitoba';
    
[/code]

Here is another example. This example shows how to use the three related functions HLL_ACCUMULATE, HLL_ESTIMATE, and HLL_COMBINE.

Create a simple table and data:
[code] 
    CREATE OR REPLACE SEQUENCE seq92;
    CREATE OR REPLACE TABLE sequence_demo (c1 INTEGER DEFAULT seq92.nextval, dummy SMALLINT);
    INSERT INTO sequence_demo (dummy) VALUES (0);
    
    INSERT INTO sequence_demo (dummy) SELECT dummy FROM sequence_demo;
    INSERT INTO sequence_demo (dummy) SELECT dummy FROM sequence_demo;
    INSERT INTO sequence_demo (dummy) SELECT dummy FROM sequence_demo;
    
[/code]

Create a table that contains the “state” that represents the current approximate cardinality information for the table named `sequence_demo`:
[code] 
    CREATE OR REPLACE TABLE resultstate1 AS (
      SELECT HLL_ACCUMULATE(c1) AS rs1
        FROM sequence_demo);
    
[/code]

Now create a second table and add data. (In a more realistic situation, the user could have loaded more data into the first table and divided the data into non-overlapping sets based on the time that the data was loaded.)
[code] 
    CREATE OR REPLACE TABLE test_table2 (c1 INTEGER);
    INSERT INTO test_table2 (c1) SELECT c1 + 4 FROM sequence_demo;
    
[/code]

Get the “state” information for just the new data.
[code] 
    CREATE OR REPLACE TABLE resultstate2 AS
      (SELECT HLL_ACCUMULATE(c1) AS rs1
         FROM test_table2);
    
[/code]

Combine the “state” information for the two batches of rows:
[code] 
    CREATE OR REPLACE TABLE combined_resultstate (c1) AS
      SELECT HLL_COMBINE(rs1) AS apc1
        FROM (
          SELECT rs1 FROM resultstate1
          UNION ALL
          SELECT rs1 FROM resultstate2
        );
    
[/code]

Get the approximate cardinality of the combined set of rows:
[code] 
    SELECT HLL_ESTIMATE(c1)
      FROM combined_resultstate;
    
[/code]
[code] 
    +------------------+
    | HLL_ESTIMATE(C1) |
    |------------------|
    |               12 |
    +------------------+
    
[/code]
