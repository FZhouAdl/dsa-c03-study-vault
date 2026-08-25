---
title: "snowflake.snowpark.functions.ai_agg | Snowflake Documentation"
source: https://docs.snowflake.com/en/developer-guide/snowpark/reference/python/latest/snowpark/api/snowflake.snowpark.functions.ai_agg.html
cert_domain: domain-2-data-preparation-and-feature-engineering
crawl_depth: 1
crawled: 2026-08-23
---

# snowflake.snowpark.functions.ai_agg¶

snowflake.snowpark.functions.ai_agg(_expr : Union[[Column](snowflake.snowpark.Column.html#snowflake.snowpark.Column "snowflake.snowpark.column.Column"), str]_, _task_description : Union[[Column](snowflake.snowpark.Column.html#snowflake.snowpark.Column "snowflake.snowpark.column.Column"), str]_) → [Column](snowflake.snowpark.Column.html#snowflake.snowpark.Column "snowflake.snowpark.column.Column")[[source]](https://github.com/snowflakedb/snowpark-python/blob/v1.54.0/src/snowflake/snowpark/functions.py#L13430-L13488)¶
    

Aggregates a column of text data using a natural language task description.

This function reduces a column of text by performing a natural language aggregation as described in the task description. For instance, it can summarize large datasets or extract specific insights.

Parameters:
    

  * **expr** – A column or literal string containing the text data on which the aggregation operation is to be performed.

  * **task_description** – A plain English string that describes the aggregation task, such as “Summarize the product reviews for a blog post targeting consumers” or “Identify the most positive review and translate it into French and Polish, one word only”.




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
    >>> summary_df = df.select(ai_agg(col("review"), "Summarize the product reviews for a blog post targeting consumers"))
    >>> summary_df.count()
    1
    >>> summary_df = df.group_by("product_id").agg(ai_agg(col("review"), "Summarize the product reviews for a blog post targeting consumers"))
    >>> summary_df.count()
    2
    
[/code]

Note

For optimal performance, follow these guidelines:

>   * Use plain English text for the task description.
> 
>   * Describe the text provided in the task description. For example, instead of a task description like “summarize”, use “Summarize the phone call transcripts”.
> 
>   * Describe the intended use case. For example, instead of “find the best review”, use “Find the most positive and well-written restaurant review to highlight on the restaurant website”.
> 
>   * Consider breaking the task description into multiple steps. For example, instead of “Summarize the new articles”, use “You will be provided with news articles from various publishers presenting events from different points of view. Please create a concise and elaborative summary of source texts without missing any crucial information.”.
> 
>
