---
title: "snowflake.snowpark.functions.ai_summarize_agg | Snowflake Documentation"
source: https://docs.snowflake.com/en/developer-guide/snowpark/reference/python/latest/snowpark/api/snowflake.snowpark.functions.ai_summarize_agg.html
cert_domain: domain-2-data-preparation-and-feature-engineering
crawl_depth: 1
crawled: 2026-08-23
---

# snowflake.snowpark.functions.ai_summarize_agg¶

snowflake.snowpark.functions.ai_summarize_agg(_expr : Union[[Column](snowflake.snowpark.Column.html#snowflake.snowpark.Column "snowflake.snowpark.column.Column"), str]_) → [Column](snowflake.snowpark.Column.html#snowflake.snowpark.Column "snowflake.snowpark.column.Column")[[source]](https://github.com/snowflakedb/snowpark-python/blob/v1.54.0/src/snowflake/snowpark/functions.py#L13399-L13427)¶
    

Summarizes a column of text data.

Parameters:
    

**expr** – This is an expression that contains text for summarization, such as restaurant reviews or phone transcripts.

Example:
[code] 
    >>> df = session.create_dataframe([
    ...     [1, "Excellent"],
    ...     [1, "Excellent"],
    ...     [1, "Great"],
    ...     [1, "Mediocre"],
    ...     [2, "Terrible"],
    ...     [2, "Bad"],
    ... ], schema=["product_id", "review"])
    >>> summary_df = df.select(ai_summarize_agg(col("review")))
    >>> summary_df.count()
    1
    >>> summary_df = df.group_by("product_id").agg(ai_summarize_agg(col("review")))
    >>> summary_df.count()
    2
    
[/code]
