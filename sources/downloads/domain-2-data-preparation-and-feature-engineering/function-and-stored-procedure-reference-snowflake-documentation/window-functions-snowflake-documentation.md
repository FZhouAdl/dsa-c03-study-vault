---
title: "Window functions | Snowflake Documentation"
source: https://docs.snowflake.com/sql-reference/functions-window
cert_domain: domain-2-data-preparation-and-feature-engineering
crawl_depth: 1
crawled: 2026-08-23
---

# Window functions¶

Window functions are analytic functions that you can use for various calculations such as running totals, moving averages, and rankings.

For general syntax rules, see [Window function syntax and usage](/sql-reference/functions-window-syntax). For syntax specific to individual functions, go to the links in the following table.

Sub-category| Notes  
---|---  
**General window**|   
[ANY_VALUE](/sql-reference/functions/any_value)|   
[AVG](/sql-reference/functions/avg)|   
[CONDITIONAL_CHANGE_EVENT](/sql-reference/functions/conditional_change_event)|   
[CONDITIONAL_TRUE_EVENT](/sql-reference/functions/conditional_true_event)|   
[CORR](/sql-reference/functions/corr)|   
[COUNT](/sql-reference/functions/count)|   
[COUNT_IF](/sql-reference/functions/count_if)|   
[COVAR_POP](/sql-reference/functions/covar_pop)|   
[COVAR_SAMP](/sql-reference/functions/covar_samp)|   
[INTERPOLATE_BFILL, INTERPOLATE_FFILL, INTERPOLATE_LINEAR](/sql-reference/functions/interpolate_bfill)|   
[LISTAGG](/sql-reference/functions/listagg)| Uses WITHIN GROUP syntax.  
[MAX](/sql-reference/functions/max)|   
[MEDIAN](/sql-reference/functions/median)|   
[MIN](/sql-reference/functions/min)|   
[MODE](/sql-reference/functions/mode)|   
[PERCENTILE_CONT](/sql-reference/functions/percentile_cont)| Uses WITHIN GROUP syntax.  
[PERCENTILE_DISC](/sql-reference/functions/percentile_disc)| Uses WITHIN GROUP syntax.  
[RATIO_TO_REPORT](/sql-reference/functions/ratio_to_report)|   
[STDDEV, STDDEV_SAMP](/sql-reference/functions/stddev)| STDDEV and STDDEV_SAMP are aliases.  
[STDDEV_POP](/sql-reference/functions/stddev_pop)|   
[SUM](/sql-reference/functions/sum)|   
[VAR_POP](/sql-reference/functions/var_pop)|   
[VAR_SAMP](/sql-reference/functions/var_samp)|   
[VARIANCE_POP](/sql-reference/functions/variance_pop)| Alias for [VAR_POP](/sql-reference/functions/var_pop).  
[VARIANCE , VARIANCE_SAMP](/sql-reference/functions/variance)| Alias for [VAR_SAMP](/sql-reference/functions/var_samp).  
**Ranking**|   
[CUME_DIST](/sql-reference/functions/cume_dist)|   
[DENSE_RANK](/sql-reference/functions/dense_rank)|   
[FIRST_VALUE](/sql-reference/functions/first_value)|   
[LAG](/sql-reference/functions/lag)|   
[LAST_VALUE](/sql-reference/functions/last_value)|   
[LEAD](/sql-reference/functions/lead)|   
[NTH_VALUE](/sql-reference/functions/nth_value)|   
[NTILE](/sql-reference/functions/ntile)|   
[PERCENT_RANK](/sql-reference/functions/percent_rank)| Supports only RANGE BETWEEN window frames without explicit offsets.  
[RANK](/sql-reference/functions/rank)|   
[ROW_NUMBER](/sql-reference/functions/row_number)|   
**Bitwise aggregation**|   
[BITAND_AGG](/sql-reference/functions/bitand_agg)|   
[BITOR_AGG](/sql-reference/functions/bitor_agg)|   
[BITXOR_AGG](/sql-reference/functions/bitxor_agg)|   
**Boolean aggregation**|   
[BOOLAND_AGG](/sql-reference/functions/booland_agg)|   
[BOOLOR_AGG](/sql-reference/functions/boolor_agg)|   
[BOOLXOR_AGG](/sql-reference/functions/boolxor_agg)|   
**Hash**|   
[HASH_AGG](/sql-reference/functions/hash_agg)|   
**Semi-structured data aggregation**|   
[ARRAY_AGG](/sql-reference/functions/array_agg)|   
[OBJECT_AGG](/sql-reference/functions/object_agg)|   
**Counting distinct values**|   
[ARRAY_UNION_AGG](/sql-reference/functions/array_union_agg)|   
[ARRAY_UNIQUE_AGG](/sql-reference/functions/array_unique_agg)|   
**Linear regression**|   
[REGR_AVGX](/sql-reference/functions/regr_avgx)|   
[REGR_AVGY](/sql-reference/functions/regr_avgy)|   
[REGR_COUNT](/sql-reference/functions/regr_count)|   
[REGR_INTERCEPT](/sql-reference/functions/regr_intercept)|   
[REGR_R2](/sql-reference/functions/regr_r2)|   
[REGR_SLOPE](/sql-reference/functions/regr_slope)|   
[REGR_SXX](/sql-reference/functions/regr_sxx)|   
[REGR_SXY](/sql-reference/functions/regr_sxy)|   
[REGR_SYY](/sql-reference/functions/regr_syy)|   
**Statistics and probability**|   
[KURTOSIS](/sql-reference/functions/kurtosis)|   
**Cardinality estimation**   
(**using** [HyperLogLog](/user-guide/querying-approximate-cardinality))|   
[APPROX_COUNT_DISTINCT](/sql-reference/functions/approx_count_distinct)| Alias for [HLL](/sql-reference/functions/hll).  
[HLL](/sql-reference/functions/hll)|   
[HLL_ACCUMULATE](/sql-reference/functions/hll_accumulate)|   
[HLL_COMBINE](/sql-reference/functions/hll_combine)|   
[HLL_ESTIMATE](/sql-reference/functions/hll_estimate)| Not an aggregate function; uses scalar input from [HLL_ACCUMULATE](/sql-reference/functions/hll_accumulate) or [HLL_COMBINE](/sql-reference/functions/hll_combine).  
[HLL_EXPORT](/sql-reference/functions/hll_export)|   
[HLL_IMPORT](/sql-reference/functions/hll_import)|   
**Similarity estimation**   
(**using** [MinHash](/user-guide/querying-approximate-similarity))|   
[APPROXIMATE_JACCARD_INDEX](/sql-reference/functions/approximate_jaccard_index)| Alias for [APPROXIMATE_SIMILARITY](/sql-reference/functions/approximate_similarity).  
[APPROXIMATE_SIMILARITY](/sql-reference/functions/approximate_similarity)|   
[MINHASH](/sql-reference/functions/minhash)|   
[MINHASH_COMBINE](/sql-reference/functions/minhash_combine)|   
**Frequency estimation**   
(**using** [Space-Saving](/user-guide/querying-approximate-frequent-values))|   
[APPROX_TOP_K](/sql-reference/functions/approx_top_k)|   
[APPROX_TOP_K_ACCUMULATE](/sql-reference/functions/approx_top_k_accumulate)|   
[APPROX_TOP_K_COMBINE](/sql-reference/functions/approx_top_k_combine)|   
[APPROX_TOP_K_ESTIMATE](/sql-reference/functions/approx_top_k_estimate)| Not an aggregate function; uses scalar input from [APPROX_TOP_K_ACCUMULATE](/sql-reference/functions/approx_top_k_accumulate) or [APPROX_TOP_K_COMBINE](/sql-reference/functions/approx_top_k_combine).  
**Percentile estimation**   
(**using** [t-Digest](/user-guide/querying-approximate-percentile-values))|   
[APPROX_PERCENTILE](/sql-reference/functions/approx_percentile)|   
[APPROX_PERCENTILE_ACCUMULATE](/sql-reference/functions/approx_percentile_accumulate)|   
[APPROX_PERCENTILE_COMBINE](/sql-reference/functions/approx_percentile_combine)|   
[APPROX_PERCENTILE_ESTIMATE](/sql-reference/functions/approx_percentile_estimate)| Not an aggregate function; uses scalar input from [APPROX_PERCENTILE_ACCUMULATE](/sql-reference/functions/approx_percentile_accumulate) or [APPROX_PERCENTILE_COMBINE](/sql-reference/functions/approx_percentile_combine).
