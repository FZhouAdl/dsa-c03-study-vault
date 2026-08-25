---
title: "APPROX_COUNT_DISTINCT | Snowflake Documentation"
source: https://docs.snowflake.com/en/sql-reference/functions/approx_count_distinct
cert_domain: domain-2-data-preparation-and-feature-engineering
crawl_depth: 0
crawled: 2026-08-23
---

Categories:
    

[Aggregate functions](/sql-reference/functions-aggregation) (Cardinality Estimation) , [Window functions](/sql-reference/functions-window)

# APPROX_COUNT_DISTINCT¶

Uses HyperLogLog to return an approximation of the distinct cardinality of the input (i.e. `HLL(col1, col2, ... )` returns an approximation of `COUNT(DISTINCT col1, col2, ... )`).

For more information about HyperLogLog, see [Estimating the Number of Distinct Values](/user-guide/querying-approximate-cardinality).

Aliases:
    

[HLL](/sql-reference/functions/hll).

See also:
    

[HLL_ACCUMULATE](/sql-reference/functions/hll_accumulate) , [HLL_COMBINE](/sql-reference/functions/hll_combine) , [HLL_ESTIMATE](/sql-reference/functions/hll_estimate)

## Syntax¶

**Aggregate function**
[code] 
    APPROX_COUNT_DISTINCT( [ DISTINCT ] <expr1>  [ , ... ] )
    
    APPROX_COUNT_DISTINCT(*)
    
[/code]

**Window function**
[code] 
    APPROX_COUNT_DISTINCT( [ DISTINCT ] <expr1>  [ , ... ] ) OVER ( [ PARTITION BY <expr2> ] )
    
    APPROX_COUNT_DISTINCT(*) OVER ( [ PARTITION BY <expr2> ] )
    
[/code]

## Arguments¶

`_expr1_`
    

This is the expression for which you want to know the number of distinct values.

`_expr2_`
    

This is the optional expression used to group rows into partitions.

`*`
    

Returns an approximation of the total number of records, excluding records with NULL values.

When you pass a wildcard to the function, you can qualify the wildcard with the name or alias for the table. For example, to pass in all of the columns from the table named `mytable`, specify the following:
[code] 
    (mytable.*)
    
[/code]

You can also use the ILIKE and EXCLUDE keywords for filtering:

  * ILIKE filters for column names that match the specified pattern. Only one pattern is allowed. For example:
[code] (* ILIKE 'col1%')
        
[/code]

  * EXCLUDE filters out column names that don’t match the specified column or columns. For example:
[code] (* EXCLUDE col1)
        
        (* EXCLUDE (col1, col2))
        
[/code]




Qualifiers are valid when you use these keywords. The following example uses the ILIKE keyword to filter for all of the columns that match the pattern `col1%` in the table `mytable`:
[code] 
    (mytable.* ILIKE 'col1%')
    
[/code]

The ILIKE and EXCLUDE keywords can’t be combined in a single function call.

For this function, the ILIKE and EXCLUDE keywords are valid only in a SELECT list or GROUP BY clause.

For more information about the ILIKE and EXCLUDE keywords, see the “Parameters” section in [SELECT](/sql-reference/sql/select).

## Returns¶

The data type of the returned value is INTEGER.

## Usage notes¶

  * Although the computation is an approximation, it is deterministic. When this function is called with the same input data, this function returns the same results.
  * For information about NULL values and aggregate functions, see [Aggregate functions and NULL values](/sql-reference/functions-aggregation#label-aggregate-functions-and-null-values).



  * When this function is called as a window function, it does not support:
    * An ORDER BY clause within the OVER clause.
    * Explicit window frames.



## Examples¶

This example shows how to use APPROX_COUNT_DISTINCT and its alias HLL. This example calls both `COUNT(DISTINCT i)` and `APPROX_COUNT_DISTINCT(i)` to emphasize that the results of those two functions do not always match exactly.

The exact output of the following query might vary because APPROX_COUNT_DISTINCT returns an approximation, not an exact value.
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
