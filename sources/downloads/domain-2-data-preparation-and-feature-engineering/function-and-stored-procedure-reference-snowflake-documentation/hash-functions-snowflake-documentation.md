---
title: "Hash functions | Snowflake Documentation"
source: https://docs.snowflake.com/sql-reference/functions-hash-scalar
cert_domain: domain-2-data-preparation-and-feature-engineering
crawl_depth: 1
crawled: 2026-08-23
---

# Hash functions¶

Snowflake provides hash functions, which take input value(s) and return a signed 64-bit numeric value. Hash functions are deterministic. Snowflake provides both a scalar hash function and an aggregate hash function, both of which are listed here.

Note

The hash functions are not cryptographic hash functions.

For cryptographic functions, use the SHA families of functions (see [String & binary functions](/sql-reference/functions-string)).

Function Name| Notes  
---|---  
[HASH](/sql-reference/functions/hash)|   
[HASH_AGG](/sql-reference/functions/hash_agg)| [Aggregate function](/sql-reference/functions-aggregation).
