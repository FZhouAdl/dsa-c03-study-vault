---
title: "STARTSWITH | Snowflake Documentation"
source: https://docs.snowflake.com/sql-reference/functions/startswith
cert_domain: domain-3-model-development
crawl_depth: 1
crawled: 2026-08-23
---

Categories:
    

[String & binary functions](/sql-reference/functions-string) (Matching/Comparison)

# STARTSWITH¶

Returns true if `_expr1_` starts with `_expr2_`. Both expressions must be text or binary expressions.

Tip

You can use the search optimization service to improve the performance of queries that call this function. For details, see [Search optimization service](/user-guide/search-optimization-service).

## Syntax¶
[code] 
    STARTSWITH( <expr1> , <expr2> )
    
[/code]

## Returns¶

Returns a BOOLEAN. The value is TRUE if `_expr1_` starts with `_expr2_`. Returns NULL if either input expression is NULL. Otherwise, returns FALSE.

## Collation details¶

The [collation specifications](/sql-reference/collation#label-collation-specification) of all input arguments must be compatible.

This function does not support the following collation specifications:

  * `pi` (punctuation-insensitive).
  * `cs-ai` (case-sensitive, accent-insensitive).



## Examples¶
[code] 
    select * from strings;
    
    ---------+
        S    |
    ---------+
     coffee  |
     ice tea |
     latte   |
     tea     |
     [NULL]  |
    ---------+
    
    select * from strings where startswith(s, 'te');
    
    -----+
      S  |
    -----+
     tea |
    -----+
    
[/code]
