---
title: "Aggregate functions | Snowflake Documentation"
source: https://docs.snowflake.com/sql-reference/functions-aggregation
cert_domain: domain-2-data-preparation-and-feature-engineering
crawl_depth: 1
crawled: 2026-08-23
---

# Aggregate functions¶

Aggregate functions operate on values across rows to perform mathematical calculations such as sum, average, counting, minimum/maximum values, standard deviation, and estimation, as well as some non-mathematical operations.

An aggregate function takes multiple rows (actually, zero, one, or more rows) as input and produces a single output. In contrast, scalar functions take one row as input and produce one row (one value) as output.

An aggregate function always returns exactly one row, even when the input contains zero rows. Typically, if the input contains zero rows, the output is NULL. However, an aggregate function could return `0`, an empty string, or some other value when passed zero rows.

## List of functions (by sub-category)¶

Function Name| Notes  
---|---  
**General Aggregation**|   
[ANY_VALUE](/sql-reference/functions/any_value)|   
[AVG](/sql-reference/functions/avg)|   
[CORR](/sql-reference/functions/corr)|   
[COUNT](/sql-reference/functions/count)|   
[COUNT_IF](/sql-reference/functions/count_if)|   
[COVAR_POP](/sql-reference/functions/covar_pop)|   
[COVAR_SAMP](/sql-reference/functions/covar_samp)|   
[LISTAGG](/sql-reference/functions/listagg)|   
[MAX](/sql-reference/functions/max)|   
[MAX_BY](/sql-reference/functions/max_by)|   
[MEDIAN](/sql-reference/functions/median)|   
[MIN](/sql-reference/functions/min)|   
[MIN_BY](/sql-reference/functions/min_by)|   
[MODE](/sql-reference/functions/mode)|   
[PERCENTILE_CONT](/sql-reference/functions/percentile_cont)| Uses different syntax than the other aggregate functions.  
[PERCENTILE_DISC](/sql-reference/functions/percentile_disc)| Uses different syntax than the other aggregate functions.  
[STDDEV, STDDEV_SAMP](/sql-reference/functions/stddev)| STDDEV and STDDEV_SAMP are aliases.  
[STDDEV_POP](/sql-reference/functions/stddev_pop)|   
[SUM](/sql-reference/functions/sum)|   
[VAR_POP](/sql-reference/functions/var_pop)|   
[VAR_SAMP](/sql-reference/functions/var_samp)|   
[VARIANCE_POP](/sql-reference/functions/variance_pop)| Alias for [VAR_POP](/sql-reference/functions/var_pop).  
[VARIANCE , VARIANCE_SAMP](/sql-reference/functions/variance)| Alias for [VAR_SAMP](/sql-reference/functions/var_samp).  
**Bitwise Aggregation**|   
[BITAND_AGG](/sql-reference/functions/bitand_agg)|   
[BITOR_AGG](/sql-reference/functions/bitor_agg)|   
[BITXOR_AGG](/sql-reference/functions/bitxor_agg)|   
**Boolean Aggregation**|   
[BOOLAND_AGG](/sql-reference/functions/booland_agg)|   
[BOOLOR_AGG](/sql-reference/functions/boolor_agg)|   
[BOOLXOR_AGG](/sql-reference/functions/boolxor_agg)|   
**Hash**|   
[HASH_AGG](/sql-reference/functions/hash_agg)|   
**Semi-structured Data Aggregation**|   
[ARRAY_AGG](/sql-reference/functions/array_agg)|   
[OBJECT_AGG](/sql-reference/functions/object_agg)|   
**Linear Regression**|   
[REGR_AVGX](/sql-reference/functions/regr_avgx)|   
[REGR_AVGY](/sql-reference/functions/regr_avgy)|   
[REGR_COUNT](/sql-reference/functions/regr_count)|   
[REGR_INTERCEPT](/sql-reference/functions/regr_intercept)|   
[REGR_R2](/sql-reference/functions/regr_r2)|   
[REGR_SLOPE](/sql-reference/functions/regr_slope)|   
[REGR_SXX](/sql-reference/functions/regr_sxx)|   
[REGR_SXY](/sql-reference/functions/regr_sxy)|   
[REGR_SYY](/sql-reference/functions/regr_syy)|   
**Statistics and Probability**|   
[KURTOSIS](/sql-reference/functions/kurtosis)|   
[SKEW](/sql-reference/functions/skew)|   
**Counting Distinct Values**|   
[ARRAY_UNION_AGG](/sql-reference/functions/array_union_agg)|   
[ARRAY_UNIQUE_AGG](/sql-reference/functions/array_unique_agg)|   
[BITMAP_ABSOLUTE_POSITION](/sql-reference/functions/bitmap_absolute_position)|   
[BITMAP_AND](/sql-reference/functions/bitmap_and)|   
[BITMAP_AND_AGG](/sql-reference/functions/bitmap_and_agg)|   
[BITMAP_BIT_POSITION](/sql-reference/functions/bitmap_bit_position)|   
[BITMAP_BUCKET_NUMBER](/sql-reference/functions/bitmap_bucket_number)|   
[BITMAP_COUNT](/sql-reference/functions/bitmap_count)|   
[BITMAP_CONSTRUCT_AGG](/sql-reference/functions/bitmap_construct_agg)|   
[BITMAP_OR](/sql-reference/functions/bitmap_or)|   
[BITMAP_OR_AGG](/sql-reference/functions/bitmap_or_agg)|   
[BITMAP_TO_ARRAY](/sql-reference/functions/bitmap_to_array)|   
**Cardinality Estimation**   
(**using** [HyperLogLog](/user-guide/querying-approximate-cardinality))|   
[APPROX_COUNT_DISTINCT](/sql-reference/functions/approx_count_distinct)| Alias for [HLL](/sql-reference/functions/hll).  
[DATASKETCHES_HLL](/sql-reference/functions/datasketches_hll)|   
[DATASKETCHES_HLL_ACCUMULATE](/sql-reference/functions/datasketches_hll_accumulate)|   
[DATASKETCHES_HLL_COMBINE](/sql-reference/functions/datasketches_hll_combine)|   
[DATASKETCHES_HLL_ESTIMATE](/sql-reference/functions/datasketches_hll_estimate)| Not an aggregate function; uses scalar input from [DATASKETCHES_HLL_ACCUMULATE](/sql-reference/functions/datasketches_hll_accumulate) or [DATASKETCHES_HLL_COMBINE](/sql-reference/functions/datasketches_hll_combine).  
[HLL](/sql-reference/functions/hll)|   
[HLL_ACCUMULATE](/sql-reference/functions/hll_accumulate)|   
[HLL_COMBINE](/sql-reference/functions/hll_combine)|   
[HLL_ESTIMATE](/sql-reference/functions/hll_estimate)| Not an aggregate function; uses scalar input from [HLL_ACCUMULATE](/sql-reference/functions/hll_accumulate) or [HLL_COMBINE](/sql-reference/functions/hll_combine).  
[HLL_EXPORT](/sql-reference/functions/hll_export)|   
[HLL_IMPORT](/sql-reference/functions/hll_import)|   
**Similarity Estimation**   
(**using** [MinHash](/user-guide/querying-approximate-similarity))|   
[APPROXIMATE_JACCARD_INDEX](/sql-reference/functions/approximate_jaccard_index)| Alias for [APPROXIMATE_SIMILARITY](/sql-reference/functions/approximate_similarity).  
[APPROXIMATE_SIMILARITY](/sql-reference/functions/approximate_similarity)|   
[MINHASH](/sql-reference/functions/minhash)|   
[MINHASH_COMBINE](/sql-reference/functions/minhash_combine)|   
**Frequency Estimation**   
(**using** [Space-Saving](/user-guide/querying-approximate-frequent-values))|   
[APPROX_TOP_K](/sql-reference/functions/approx_top_k)|   
[APPROX_TOP_K_ACCUMULATE](/sql-reference/functions/approx_top_k_accumulate)|   
[APPROX_TOP_K_COMBINE](/sql-reference/functions/approx_top_k_combine)|   
[APPROX_TOP_K_ESTIMATE](/sql-reference/functions/approx_top_k_estimate)| Not an aggregate function; uses scalar input from [APPROX_TOP_K_ACCUMULATE](/sql-reference/functions/approx_top_k_accumulate) or [APPROX_TOP_K_COMBINE](/sql-reference/functions/approx_top_k_combine).  
**Percentile Estimation**   
(**using** [t-Digest](/user-guide/querying-approximate-percentile-values))|   
[APPROX_PERCENTILE](/sql-reference/functions/approx_percentile)|   
[APPROX_PERCENTILE_ACCUMULATE](/sql-reference/functions/approx_percentile_accumulate)|   
[APPROX_PERCENTILE_COMBINE](/sql-reference/functions/approx_percentile_combine)|   
[APPROX_PERCENTILE_ESTIMATE](/sql-reference/functions/approx_percentile_estimate)| Not an aggregate function; uses scalar input from [APPROX_PERCENTILE_ACCUMULATE](/sql-reference/functions/approx_percentile_accumulate) or [APPROX_PERCENTILE_COMBINE](/sql-reference/functions/approx_percentile_combine).  
**Aggregation Utilities**|   
[GROUPING](/sql-reference/functions/grouping)| Not an aggregate function, but can be used in conjunction with aggregate functions to determine the level of aggregation for a row produced by a [GROUP BY](/sql-reference/constructs/group-by) query.  
[GROUPING_ID](/sql-reference/functions/grouping_id)| Alias for [GROUPING](/sql-reference/functions/grouping).  
**AI Functions**|   
[AI_AGG](/sql-reference/functions/ai_agg)|   
[AI_SUMMARIZE_AGG](/sql-reference/functions/ai_summarize_agg)|   
**Vector Aggregation**|   
[VECTOR_AVG](/sql-reference/functions/vector_avg)|   
[VECTOR_MAX](/sql-reference/functions/vector_max)|   
[VECTOR_MIN](/sql-reference/functions/vector_min)|   
[VECTOR_SUM](/sql-reference/functions/vector_sum)|   
**Semantic views**|   
[AGG](/sql-reference/functions/agg)|   
  
## Introductory example¶

The following example illustrates the difference between an aggregate function ([AVG](/sql-reference/functions/avg)) and a scalar function ([COS](/sql-reference/functions/cos)). The scalar function returns one output row for each input row, while the aggregate function returns one output row for multiple input rows:

Create a table and populate it with values:
[code] 
    CREATE TABLE simple (x INTEGER, y INTEGER);
    INSERT INTO simple (x, y) VALUES
        (10, 20),
        (20, 44),
        (30, 70);
    
[/code]

Query the table:
[code] 
    SELECT x, y
        FROM simple
        ORDER BY x,y;
    
[/code]
[code] 
    +----+----+
    |  X |  Y |
    |----+----|
    | 10 | 20 |
    | 20 | 44 |
    | 30 | 70 |
    +----+----+
    
[/code]

The scalar function returns one output row for each input row.
[code] 
    SELECT COS(x)
        FROM simple
        ORDER BY x;
    
[/code]
[code] 
    +---------------+
    |        COS(X) |
    |---------------|
    | -0.8390715291 |
    |  0.4080820618 |
    |  0.1542514499 |
    +---------------+
    
[/code]

The aggregate function returns one output row for multiple input rows:
[code] 
    SELECT SUM(x)
        FROM simple;
    
[/code]
[code] 
    +--------+
    | SUM(X) |
    |--------|
    |     60 |
    +--------+
    
[/code]

## Aggregate functions and NULL values¶

Some aggregate functions ignore NULL values. For example, [AVG](/sql-reference/functions/avg) calculates the average of values `1`, `5`, and `NULL` to be `3`, based on the following formula:

> `(1 + 5) / 2 = 3`

In both the numerator and the denominator, only the two non-NULL values are used.

If all of the values passed to the aggregate function are NULL, then the aggregate function returns NULL.

Some aggregate functions can be passed more than one column. For example:
[code] 
    SELECT COUNT(col1, col2) FROM table1;
    
[/code]

In these instances, the aggregate function ignores a row if any individual column is NULL.

For example, in the following query, [COUNT](/sql-reference/functions/count) returns `1`, not `4`, because three of the four rows contain at least one NULL value in the selected columns:

Create a table and populate it with values:
[code] 
    CREATE OR REPLACE TABLE test_null_aggregate_functions (x INT, y INT);
    INSERT INTO test_null_aggregate_functions (x, y) VALUES
      (1, 2),         -- No NULLs.
      (3, NULL),      -- One but not all columns are NULL.
      (NULL, 6),      -- One but not all columns are NULL.
      (NULL, NULL);   -- All columns are NULL.
    
[/code]

Query the table:
[code] 
    SELECT COUNT(x, y) FROM test_null_aggregate_functions;
    
[/code]
[code] 
    +-------------+
    | COUNT(X, Y) |
    |-------------|
    |           1 |
    +-------------+
    
[/code]

If [SUM](/sql-reference/functions/sum) is called with an expression that references two or more columns, and if one or more of those columns is NULL, then the expression evaluates to NULL, and the row is ignored:
[code] 
    SELECT SUM(x + y) FROM test_null_aggregate_functions;
    
[/code]
[code] 
    +------------+
    | SUM(X + Y) |
    |------------|
    |          3 |
    +------------+
    
[/code]

This behavior differs from the behavior of [GROUP BY](/sql-reference/constructs/group-by), which does not discard rows when some columns are NULL:
[code] 
    SELECT x AS X_COL, y AS Y_COL
      FROM test_null_aggregate_functions
      GROUP BY x, y;
    
[/code]
[code] 
    +-------+-------+
    | X_COL | Y_COL |
    |-------+-------|
    |     1 |     2 |
    |     3 |  NULL |
    |  NULL |     6 |
    |  NULL |  NULL |
    +-------+-------+
    
[/code]
