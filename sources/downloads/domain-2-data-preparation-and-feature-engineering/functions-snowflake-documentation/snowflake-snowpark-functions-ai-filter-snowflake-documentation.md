---
title: "snowflake.snowpark.functions.ai_filter | Snowflake Documentation"
source: https://docs.snowflake.com/en/developer-guide/snowpark/reference/python/latest/snowpark/api/snowflake.snowpark.functions.ai_filter.html
cert_domain: domain-2-data-preparation-and-feature-engineering
crawl_depth: 1
crawled: 2026-08-23
---

# snowflake.snowpark.functions.ai_filter¶

snowflake.snowpark.functions.ai_filter(_predicate : Union[[Column](snowflake.snowpark.Column.html#snowflake.snowpark.Column "snowflake.snowpark.column.Column"), str]_, _file : Optional[[Column](snowflake.snowpark.Column.html#snowflake.snowpark.Column "snowflake.snowpark.column.Column")] = None_, _return_error_details : Optional[bool] = None_) → [Column](snowflake.snowpark.Column.html#snowflake.snowpark.Column "snowflake.snowpark.column.Column")[[source]](https://github.com/snowflakedb/snowpark-python/blob/v1.54.0/src/snowflake/snowpark/functions.py#L13154-L13234)¶
    

Classifies free-form prompt inputs into a boolean. Currently supports both text and image filtering.

Parameters:
    

  * **predicate** – If you’re specifying an input string, it is string containing the text to be classified; If you’re filtering on one file, it is a string containing the instructions to classify the file input as either TRUE or FALSE.

  * **file** – The FILE type column that the file is classified by based on the instructions specified in `predicate`. You can use IMAGE FILE as an input to the AI_FILTER function.

  * **return_error_details** – When `True`, returns an OBJECT with `value` and `error` fields instead of returning NULL on failure.




Note

For more complicated prompts, especially with multiple file columns, you can use the [`prompt()`](snowflake.snowpark.functions.prompt.html#snowflake.snowpark.functions.prompt "snowflake.snowpark.functions.prompt") function to help with creating an input, which supports formatting across both strings and FILE datatypes.

Examples:
[code] 
    >>> # for text
    >>> session.range(1).select(ai_filter('Is Canada in North America?').alias("answer")).show()
    ------------
    |"ANSWER"  |
    ------------
    |True      |
    ------------
    
    >>> # use prompt function
    >>> df = session.create_dataframe(["Switzerland", "Korea"], schema=["country"])
    >>> df.select(
    ...     ai_filter(prompt("Is {0} in Asia?", col("country"))).as_("asia"),
    ...     ai_filter(prompt("Is {0} in Europe?", col("country"))).as_("europe"),
    ...     ai_filter(prompt("Is {0} in North America?", col("country"))).as_("north_america"),
    ...     ai_filter(prompt("Is {0} in Central America?", col("country"))).as_("central_america"),
    ... ).show()
    -----------------------------------------------------------
    |"ASIA"  |"EUROPE"  |"NORTH_AMERICA"  |"CENTRAL_AMERICA"  |
    -----------------------------------------------------------
    |False   |True      |False            |False              |
    |True    |False     |False            |False              |
    -----------------------------------------------------------
    
    >>> # for image
    >>> _ = session.sql("CREATE OR REPLACE TEMP STAGE mystage ENCRYPTION = (TYPE = 'SNOWFLAKE_SSE')").collect()
    >>> _ = session.file.put("tests/resources/dog.jpg", "@mystage", auto_compress=False)
    >>> df = session.range(1).select(ai_filter("is it a dog picture?", to_file("@mystage/dog.jpg")).alias("is_dog"))
    >>> df.show()
    ------------
    |"IS_DOG"  |
    ------------
    |True      |
    ------------
    
[/code]
