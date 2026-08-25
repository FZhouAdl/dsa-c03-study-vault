---
title: "SUMMARIZE (SNOWFLAKE.CORTEX) | Snowflake Documentation"
source: https://docs.snowflake.com/sql-reference/functions/summarize-snowflake-cortex
cert_domain: domain-1-data-science-concepts
crawl_depth: 1
crawled: 2026-08-23
---

Categories:
    

[String & binary functions](/sql-reference/functions-string) (AI Functions)

# SUMMARIZE (SNOWFLAKE.CORTEX)¶

Summarizes the given English-language input text.

## Syntax¶
[code] 
    SNOWFLAKE.CORTEX.SUMMARIZE(<text>)
    
[/code]

## Arguments¶

`_text_`
    

A string containing the English text from which a summary should be generated.

## Returns¶

A string containing a summary of the original text.

## Access control requirements¶

Users must use a role that has been granted the [SNOWFLAKE.CORTEX_USER database role](/sql-reference/snowflake-db-roles#label-snowflake-db-roles-cortex-user). See [Cortex LLM privileges](/user-guide/snowflake-cortex/aisql-privileges-and-access#label-cortex-llm-privileges) for more information on this privilege.

## Example¶

In this example, a table named `reviews` contains a column named `review_content` containing the text of reviews submitted by users. The query returns a summary of each review.
[code] 
    SELECT SNOWFLAKE.CORTEX.SUMMARIZE(review_content) FROM reviews LIMIT 10;
    
[/code]

## Legal notices¶

Refer to [Snowflake AI and ML](/guides-overview-ai-features).
