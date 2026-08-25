---
title: "Vector functions | Snowflake Documentation"
source: https://docs.snowflake.com/sql-reference/functions-vector
cert_domain: domain-2-data-preparation-and-feature-engineering
crawl_depth: 1
crawled: 2026-08-23
---

# Vector functions¶

Snowflake provides both similarity and element-wise aggregation functions for the [VECTOR](/sql-reference/data-types-vector) data type. These functions allow for finding vectors nearest to a source vector, used for semantic search and fine-tuning generative responses from LLMs and generative AI.

Similarity functions operate on two VECTOR arguments of equal element type and dimension, computing the specified metric. Snowflake provides the following vector similarity functions:

>   * [VECTOR_INNER_PRODUCT](/sql-reference/functions/vector_inner_product)
>   * [VECTOR_L1_DISTANCE](/sql-reference/functions/vector_l1_distance)
>   * [VECTOR_L2_DISTANCE](/sql-reference/functions/vector_l2_distance)
>   * [VECTOR_COSINE_SIMILARITY](/sql-reference/functions/vector_cosine_similarity)
> 


Vector manipulation functions take an existing vector and return a new vector with different properties, such as truncation or normalization. Snowflake provides the following vector manipulation functions:

>   * [VECTOR_TRUNCATE](/sql-reference/functions/vector_truncate)
>   * [VECTOR_NORMALIZE](/sql-reference/functions/vector_normalize)
> 


Vector aggregate functions operate on columns of VECTOR values to perform element-wise mathematical operations such as sum, average, minimum, and maximum across all vectors in a group. Snowflake provides the following vector aggregation functions:

>   * [VECTOR_SUM](/sql-reference/functions/vector_sum)
>   * [VECTOR_MIN](/sql-reference/functions/vector_min)
>   * [VECTOR_MAX](/sql-reference/functions/vector_max)
>   * [VECTOR_AVG](/sql-reference/functions/vector_avg)
> 


Note

Vector functions on Snowflake are optimized in a way that can reduce floating point precision. These functions have a margin of error up to `1e-4`.

## List of functions¶

Function Name| Notes  
---|---  
[VECTOR_INNER_PRODUCT](/sql-reference/functions/vector_inner_product)|   
[VECTOR_L1_DISTANCE](/sql-reference/functions/vector_l1_distance)|   
[VECTOR_L2_DISTANCE](/sql-reference/functions/vector_l2_distance)|   
[VECTOR_COSINE_SIMILARITY](/sql-reference/functions/vector_cosine_similarity)| Not supported in Snowpark API.  
[VECTOR_TRUNCATE](/sql-reference/functions/vector_truncate)|   
[VECTOR_NORMALIZE](/sql-reference/functions/vector_normalize)|   
[VECTOR_SUM](/sql-reference/functions/vector_sum)|   
[VECTOR_MIN](/sql-reference/functions/vector_min)|   
[VECTOR_MAX](/sql-reference/functions/vector_max)|   
[VECTOR_AVG](/sql-reference/functions/vector_avg)|
