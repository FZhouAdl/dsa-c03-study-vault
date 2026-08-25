---
title: "Query syntax | Snowflake Documentation"
source: https://docs.snowflake.com/sql-reference/constructs
cert_domain: domain-2-data-preparation-and-feature-engineering
crawl_depth: 1
crawled: 2026-08-23
---

# Query syntax¶

Snowflake supports querying using standard [SELECT](/sql-reference/sql/select) statements and the following basic syntax:
[code] 
    [ WITH ... ]
    SELECT
      [ TOP <n> ]
       ...
    [ INTO ... ]
    [ FROM ...
       [ AT | BEFORE ... ]
       [ CHANGES ... ]
       [ CONNECT BY ... ]
       [ JOIN ... ]
       [ ASOF JOIN ... ]
       [ LATERAL ... ]
       [ MATCH_RECOGNIZE ... ]
       [ PIVOT | UNPIVOT ... ]
       [ VALUES ... ]
       [ SAMPLE ... ]
       [ RESAMPLE ... ]
       [ SEMANTIC_VIEW( ... ) ] ]
    [ WHERE ... ]
    [ GROUP BY ... ]
    [ HAVING ... ]
    [ QUALIFY ... ]
    [ ORDER BY ... ]
    [ LIMIT ... ]
    [ FOR UPDATE ... ]
    
[/code]

**Next Topics:**

  * [WITH](/sql-reference/constructs/with)

  * [TOP <n>](/sql-reference/constructs/top_n)

  * [INTO](/sql-reference/constructs/into)

  * [FROM](/sql-reference/constructs/from)

    * [AT | BEFORE](/sql-reference/constructs/at-before)
    * [CHANGES](/sql-reference/constructs/changes)
    * [CONNECT BY](/sql-reference/constructs/connect-by)
    * [JOIN](/sql-reference/constructs/join)
    * [ASOF JOIN](/sql-reference/constructs/asof-join)
    * [LATERAL](/sql-reference/constructs/join-lateral)
    * [MATCH_RECOGNIZE](/sql-reference/constructs/match_recognize)
    * [PIVOT](/sql-reference/constructs/pivot)
    * [UNPIVOT](/sql-reference/constructs/unpivot)
    * [VALUES](/sql-reference/constructs/values)
    * [SAMPLE / TABLESAMPLE](/sql-reference/constructs/sample)
    * [RESAMPLE](/sql-reference/constructs/resample)
    * [SEMANTIC_VIEW](/sql-reference/constructs/semantic_view)
  * [WHERE](/sql-reference/constructs/where)

  * [GROUP BY](/sql-reference/constructs/group-by)

    * [GROUP BY CUBE](/sql-reference/constructs/group-by-cube)
    * [GROUP BY GROUPING SETS](/sql-reference/constructs/group-by-grouping-sets)
    * [GROUP BY ROLLUP](/sql-reference/constructs/group-by-rollup)
    * [HAVING](/sql-reference/constructs/having)
  * [QUALIFY](/sql-reference/constructs/qualify)

  * [ORDER BY](/sql-reference/constructs/order-by)

  * [LIMIT / FETCH](/sql-reference/constructs/limit)

  * [FOR UPDATE](/sql-reference/constructs/for-update)
