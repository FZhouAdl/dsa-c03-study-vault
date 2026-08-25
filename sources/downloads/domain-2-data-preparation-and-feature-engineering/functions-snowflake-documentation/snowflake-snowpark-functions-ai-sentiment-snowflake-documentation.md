---
title: "snowflake.snowpark.functions.ai_sentiment | Snowflake Documentation"
source: https://docs.snowflake.com/en/developer-guide/snowpark/reference/python/latest/snowpark/api/snowflake.snowpark.functions.ai_sentiment.html
cert_domain: domain-2-data-preparation-and-feature-engineering
crawl_depth: 1
crawled: 2026-08-23
---

# snowflake.snowpark.functions.ai_sentiment¶

snowflake.snowpark.functions.ai_sentiment(_text : Union[[Column](snowflake.snowpark.Column.html#snowflake.snowpark.Column "snowflake.snowpark.column.Column"), str]_, _categories : Optional[List[str]] = None_, _return_error_details : Optional[bool] = None_) → [Column](snowflake.snowpark.Column.html#snowflake.snowpark.Column "snowflake.snowpark.column.Column")[[source]](https://github.com/snowflakedb/snowpark-python/blob/v1.54.0/src/snowflake/snowpark/functions.py#L14149-L14320)¶
    

Returns overall and category sentiment in the given input text.

Parameters:
    

  * **text** – A string containing the text in which sentiment is detected.

  * **categories** – An array containing up to ten categories (also called entities or aspects) for which sentiment should be extracted. Each category is a string. For example, if extracting sentiment from a restaurant review, you might specify `['cost', 'quality', 'service', 'wait time']` as the categories. Each category may be a maximum of 30 characters long. If you do not provide this argument, AI_SENTIMENT returns only the overall sentiment.

  * **return_error_details** – When `True`, returns an OBJECT with `value` and `error` fields instead of returning NULL on failure.



Returns:
    

  * `name`: The name of the category. The category names match the categories specified in the `categories` argument.

  * `sentiment`: The sentiment of the category. Each sentiment result is one of the following strings.

>     * `unknown`: The category was not mentioned in the text.
> 
>     * `positive`: The category was mentioned positively in the text.
> 
>     * `negative`: The category was mentioned negatively in the text.
> 
>     * `neutral`: The category was mentioned in the text, but neither positively nor negatively.
> 
>     * `mixed`: The category was mentioned both positively and negatively in the text.




The `overall` category record is always included and contains the overall sentiment of the text.

Return type:
    

An OBJECT value containing a `categories` field. `categories` is an array of category records. Each category includes these fields

Note

AI_SENTIMENT can analyze sentiment in English, French, German, Hindi, Italian, Spanish, and Portuguese. You can specify categories in the language of the text or in English.

Examples:
[code] 
    >>> # Get overall sentiment only
    >>> session.range(1).select(
    ...     ai_sentiment("A tourist's delight, in low urban light, Recommended gem, a pizza night sight. Swift arrival, a pleasure so right, Yet, pockets felt lighter, a slight pricey bite. 💰🍕🚀").alias("sentiment")
    ... ).show()
    ------------------------------
    |"SENTIMENT"                 |
    ------------------------------
    |{                           |
    |  "categories": [           |
    |    {                       |
    |      "name": "overall",    |
    |      "sentiment": "mixed"  |
    |    }                       |
    |  ]                         |
    |}                           |
    ------------------------------
    
    
    >>> # Extract sentiment for specific categories
    >>> df = session.create_dataframe([
    ...     ["The movie had amazing visual effects but the plot was terrible."],
    ...     ["The food was delicious but the service was slow."],
    ...     ["The movie was great, but the acting was terrible."]
    ... ], schema=["review"])
    >>> df.select("review", ai_sentiment(col("review"), ['plot', 'visual effects', 'acting']).alias("sentiment")).show()
    ----------------------------------------------------------------------------------------
    |"REVIEW"                                            |"SENTIMENT"                      |
    ----------------------------------------------------------------------------------------
    |The movie had amazing visual effects but the pl...  |{                                |
    |                                                    |  "categories": [                |
    |                                                    |    {                            |
    |                                                    |      "name": "overall",         |
    |                                                    |      "sentiment": "mixed"       |
    |                                                    |    },                           |
    |                                                    |    {                            |
    |                                                    |      "name": "acting",          |
    |                                                    |      "sentiment": "neutral"     |
    |                                                    |    },                           |
    |                                                    |    {                            |
    |                                                    |      "name": "plot",            |
    |                                                    |      "sentiment": "negative"    |
    |                                                    |    },                           |
    |                                                    |    {                            |
    |                                                    |      "name": "visual effects",  |
    |                                                    |      "sentiment": "positive"    |
    |                                                    |    }                            |
    |                                                    |  ]                              |
    |                                                    |}                                |
    |The food was delicious but the service was slow.    |{                                |
    |                                                    |  "categories": [                |
    |                                                    |    {                            |
    |                                                    |      "name": "overall",         |
    |                                                    |      "sentiment": "mixed"       |
    |                                                    |    },                           |
    |                                                    |    {                            |
    |                                                    |      "name": "acting",          |
    |                                                    |      "sentiment": "unknown"     |
    |                                                    |    },                           |
    |                                                    |    {                            |
    |                                                    |      "name": "plot",            |
    |                                                    |      "sentiment": "unknown"     |
    |                                                    |    },                           |
    |                                                    |    {                            |
    |                                                    |      "name": "visual effects",  |
    |                                                    |      "sentiment": "unknown"     |
    |                                                    |    }                            |
    |                                                    |  ]                              |
    |                                                    |}                                |
    |The movie was great, but the acting was terrible.   |{                                |
    |                                                    |  "categories": [                |
    |                                                    |    {                            |
    |                                                    |      "name": "overall",         |
    |                                                    |      "sentiment": "mixed"       |
    |                                                    |    },                           |
    |                                                    |    {                            |
    |                                                    |      "name": "acting",          |
    |                                                    |      "sentiment": "negative"    |
    |                                                    |    },                           |
    |                                                    |    {                            |
    |                                                    |      "name": "plot",            |
    |                                                    |      "sentiment": "positive"    |
    |                                                    |    },                           |
    |                                                    |    {                            |
    |                                                    |      "name": "visual effects",  |
    |                                                    |      "sentiment": "positive"    |
    |                                                    |    }                            |
    |                                                    |  ]                              |
    |                                                    |}                                |
    ----------------------------------------------------------------------------------------
    
[/code]
