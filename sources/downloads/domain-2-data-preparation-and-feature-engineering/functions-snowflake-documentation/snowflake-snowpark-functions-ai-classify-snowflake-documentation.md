---
title: "snowflake.snowpark.functions.ai_classify | Snowflake Documentation"
source: https://docs.snowflake.com/en/developer-guide/snowpark/reference/python/latest/snowpark/api/snowflake.snowpark.functions.ai_classify.html
cert_domain: domain-2-data-preparation-and-feature-engineering
crawl_depth: 1
crawled: 2026-08-23
---

# snowflake.snowpark.functions.ai_classify¶

snowflake.snowpark.functions.ai_classify(_expr : Union[[Column](snowflake.snowpark.Column.html#snowflake.snowpark.Column "snowflake.snowpark.column.Column"), str]_, _list_of_categories : Union[[Column](snowflake.snowpark.Column.html#snowflake.snowpark.Column "snowflake.snowpark.column.Column"), List[str]]_, _return_error_details : Optional[bool] = None_, _** kwargs_) → [Column](snowflake.snowpark.Column.html#snowflake.snowpark.Column "snowflake.snowpark.column.Column")[[source]](https://github.com/snowflakedb/snowpark-python/blob/v1.54.0/src/snowflake/snowpark/functions.py#L13237-L13396)¶
    

Classifies text or images into categories that you specify.

Parameters:
    

  * **expr** – The string, image, or a SQL object from [`prompt()`](snowflake.snowpark.functions.prompt.html#snowflake.snowpark.functions.prompt "snowflake.snowpark.functions.prompt") that you’re classifying. If you’re classifying text, the input string is case sensitive. You might get different results if you use different capitalization.

  * **list_of_categories** – An array of strings that represents the different categories. Categories are case-sensitive. The array must contain at least 2 and no more than 100 categories. If the requirements aren’t met, the function returns an error.

  * **return_error_details** – When `True`, returns an OBJECT with `value` and `error` fields instead of returning NULL on failure.

  * ****kwargs** – 

Configuration settings specified as key/value pairs. Supported keys:

    * task_description: A explanation of the classification task that is 50 words or fewer.
    

This can help the model understand the context of the classification task and improve accuracy.

    * output_mode: Set to `multi` for multi-label classification. Defaults to single for single-label classification.

    * examples: A list of example objects for few-shot learning. Each example must include:

>       * input: Example text to classify.
> 
>       * labels: List of correct categories for the input.
> 
>       * explanation: Explanation of why the input maps to those categories.



Returns:
    

A serialized object. The object’s `label` field is a string that specifies the category to which the input belongs. If you specify invalid values for the arguments, an error is returned.

Examples:
[code] 
    >>> # for text
    >>> session.range(1).select(ai_classify('One day I will see the world', ['travel', 'cooking']).alias("answer")).show()
    -----------------
    |"ANSWER"       |
    -----------------
    |{              |
    |  "labels": [  |
    |    "travel"   |
    |  ]            |
    |}              |
    -----------------
    
    >>> df = session.create_dataframe([
    ...     ['France', ['North America', 'Europe', 'Asia']],
    ...     ['Singapore', ['North America', 'Europe', 'Asia']],
    ...     ['one day I will see the world', ['travel', 'cooking', 'dancing']],
    ...     ['my lobster bisque is second to none', ['travel', 'cooking', 'dancing']]
    ... ], schema=["data", "category"])
    >>> df.select("data", ai_classify(col("data"), col("category"))["labels"][0].alias("class")).sort("data").show()
    ---------------------------------------------------
    |"DATA"                               |"CLASS"    |
    ---------------------------------------------------
    |France                               |"Europe"   |
    |Singapore                            |"Asia"     |
    |my lobster bisque is second to none  |"cooking"  |
    |one day I will see the world         |"travel"   |
    ---------------------------------------------------
    
    
    >>> # using kwargs for advanced configuration
    >>> session.range(1).select(
    ...     ai_classify(
    ...         'One day I will see the world and learn to cook my favorite dishes',
    ...         ['travel', 'cooking', 'reading', 'driving'],
    ...         task_description='Determine topics related to the given text',
    ...         output_mode='multi',
    ...         examples=[{
    ...             'input': 'i love traveling with a good book',
    ...             'labels': ['travel', 'reading'],
    ...             'explanation': 'the text mentions traveling and a good book which relates to reading'
    ...         }]
    ...     ).alias("answer")
    ... ).show()
    -----------------
    |"ANSWER"       |
    -----------------
    |{              |
    |  "labels": [  |
    |    "travel",  |
    |    "cooking"  |
    |  ]            |
    |}              |
    -----------------
    
    
    >>> # for image
    >>> _ = session.sql("CREATE OR REPLACE TEMP STAGE mystage ENCRYPTION = (TYPE = 'SNOWFLAKE_SSE')").collect()
    >>> _ = session.file.put("tests/resources/dog.jpg", "@mystage", auto_compress=False)
    >>> df = session.range(1).select(
    ...     ai_classify(
    ...         prompt("Please help me classify the dog within this image {0}", to_file("@mystage/dog.jpg")),
    ...         ["French Bulldog", "Golden Retriever", "Bichon", "Cavapoo", "Beagle"]
    ...     ).alias("classes")
    ... )
    >>> df.show()
    -----------------
    |"CLASSES"      |
    -----------------
    |{              |
    |  "labels": [  |
    |    "Cavapoo"  |
    |  ]            |
    |}              |
    -----------------
    
[/code]
