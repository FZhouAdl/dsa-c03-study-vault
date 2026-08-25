---
title: "Bitwise expression functions | Snowflake Documentation"
source: https://docs.snowflake.com/sql-reference/expressions-byte-bit
cert_domain: domain-2-data-preparation-and-feature-engineering
crawl_depth: 1
crawled: 2026-08-23
---

# Bitwise expression functions¶

This family of functions can be used to perform bitwise operations on numbers or a group of numeric records.

Function Name| Syntax| Summary Description  
---|---|---  
[BITAND](/sql-reference/functions/bitand)| `BITAND(a, b)`| Bitwise AND of two numeric or binary expressions (`a` and `b`).  
[BITAND_AGG](/sql-reference/functions/bitand_agg)| `BITAND_AGG(a)`| Bitwise AND value of all non-NULL numeric records in a group `a`.  
[BITNOT](/sql-reference/functions/bitnot)| `BITNOT(a)`| Bitwise negation of `a` numeric or binary expression.  
[BITOR](/sql-reference/functions/bitor)| `BITOR(a, b)`| Bitwise OR of two numeric or binary expressions (`a` and `b`).  
[BITOR_AGG](/sql-reference/functions/bitor_agg)| `BITOR_AGG(a)`| Bitwise OR value of all non-NULL numeric records in a group `a`.  
[BITSHIFTLEFT](/sql-reference/functions/bitshiftleft)| `BITSHIFTLEFT(a, n)`| Shift the bits for `a` numeric or binary expression `n` positions to the left.  
[BITSHIFTRIGHT](/sql-reference/functions/bitshiftright)| `BITSHIFTRIGHT(a, n)`| Shift the bits for `a` numeric or binary expression `n` positions to the right, with sign extension.  
[BITXOR](/sql-reference/functions/bitxor)| `BITXOR(a, b)`| Bitwise XOR of two numeric or binary expressions (`a` and `b`).  
[BITXOR_AGG](/sql-reference/functions/bitxor_agg)| `BITXOR_AGG(a)`| Bitwise XOR value of all non-NULL numeric records in a group `a`.  
[GETBIT](/sql-reference/functions/getbit)| `GETBIT(a, n)`| Return the bit at position `n` in `a` numeric expression.
