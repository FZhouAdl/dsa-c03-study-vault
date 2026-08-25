---
title: "HLL | Snowflake Documentation"
source: https://docs.snowflake.com/sql-reference/functions/hll
cert_domain: domain-2-data-preparation-and-feature-engineering
crawl_depth: 1
crawled: 2026-08-23
---

Categories:
    

[Aggregate functions](/sql-reference/functions-aggregation) (Cardinality Estimation) , [Window functions](/sql-reference/functions-window)

# HLL¶

Uses HyperLogLog to return an approximation of the distinct cardinality of the input (i.e. `HLL(col1, col2, ... )` returns an approximation of `COUNT(DISTINCT col1, col2, ... )`).

For more information about HyperLogLog, see [Estimating the Number of Distinct Values](/user-guide/querying-approximate-cardinality).

Aliases:
    

[APPROX_COUNT_DISTINCT](/sql-reference/functions/approx_count_distinct).

See also:
    

[HLL_ACCUMULATE](/sql-reference/functions/hll_accumulate) , [HLL_COMBINE](/sql-reference/functions/hll_combine) , [HLL_ESTIMATE](/sql-reference/functions/hll_estimate)

## Syntax¶

**Aggregate function**
[code] 
    HLL( [ DISTINCT ] <expr1> [ , ... ] )
    
    HLL(*)
    
[/code]

**Window function**
[code] 
    HLL( [ DISTINCT ] <expr1> [ , ... ] ) OVER ( [ PARTITION BY <expr2> ] )
    
    HLL(*) OVER ( [ PARTITION BY <expr2> ] )
    
[/code]

## Arguments¶

`_expr1_`
    

This is the expression for which you want to know the number of distinct values.

`_expr2_`
    

This is the optional expression used to group rows into partitions.

## Returns¶

The data type of the returned value is INTEGER.

## Usage notes¶

  * `DISTINCT` can be included as an argument, but has no effect.
  * For information about NULL values and aggregate functions, see [Aggregate functions and NULL values](/sql-reference/functions-aggregation#label-aggregate-functions-and-null-values).
  * When this function is called as a window function, it does not support:
    * An ORDER BY clause within the OVER clause.
    * Explicit window frames.



## Examples¶

This example shows how to use HLL and its alias APPROX_COUNT_DISTINCT. This example calls both `COUNT(DISTINCT i)` and `APPROX_COUNT_DISTINCT(i)` to emphasize that the results of these two functions do not always match exactly.

The exact output from the following query might vary because APPROX_COUNT_DISTINCT() returns an approximation, not an exact value.
[code] 
    SELECT COUNT(i), COUNT(DISTINCT i), APPROX_COUNT_DISTINCT(i), HLL(i)
      FROM sequence_demo;
    
[/code]
[code] 
    +----------+-------------------+--------------------------+--------+
    | COUNT(I) | COUNT(DISTINCT I) | APPROX_COUNT_DISTINCT(I) | HLL(I) |
    |----------+-------------------+--------------------------+--------|
    |     1024 |              1024 |                     1007 |   1007 |
    +----------+-------------------+--------------------------+--------+
    
[/code]
