---
title: "snowflake.snowpark.functions.ai_similarity | Snowflake Documentation"
source: https://docs.snowflake.com/en/developer-guide/snowpark/reference/python/latest/snowpark/api/snowflake.snowpark.functions.ai_similarity.html
cert_domain: domain-2-data-preparation-and-feature-engineering
crawl_depth: 1
crawled: 2026-08-23
---

# snowflake.snowpark.functions.ai_similarity¶

snowflake.snowpark.functions.ai_similarity(_input1 : Union[[Column](snowflake.snowpark.Column.html#snowflake.snowpark.Column "snowflake.snowpark.column.Column"), str]_, _input2 : Union[[Column](snowflake.snowpark.Column.html#snowflake.snowpark.Column "snowflake.snowpark.column.Column"), str]_, _** kwargs_) → [Column](snowflake.snowpark.Column.html#snowflake.snowpark.Column "snowflake.snowpark.column.Column")[[source]](https://github.com/snowflakedb/snowpark-python/blob/v1.54.0/src/snowflake/snowpark/functions.py#L13491-L13593)¶
    

Computes a similarity score based on the vector cosine similarity value of the inputs’ embedding vectors. Currently supports both text and image similarity computation.

Parameters:
    

  * **input1** – The first input for comparison. Can be a string with text or an image (FILE data type).

  * **input2** – The second input for comparison. Can be a string with text or an image (FILE data type). Must be the same type as input1 (both text or both images).

  * ****kwargs** – 

Configuration settings specified as key/value pairs. Supported keys:

    * model: The embedding model used for embedding. For STRING input, defaults to ‘snowflake-arctic-embed-l-v2’.
    

For IMAGE input, defaults to ‘voyage-multimodal-3’. Supported values include: ‘snowflake-arctic-embed-l-v2’, ‘nv-embed-qa-4’, ‘multilingual-e5-large’, ‘voyage-multilingual-2’, ‘snowflake-arctic-embed-m-v1.5’, ‘snowflake-arctic-embed-m’, ‘e5-base-v2’, ‘voyage-multimodal-3’ (for images).



Returns:
    

A float value of range -1 to 1 that represents the similarity score computed using vector similarity between two embedding vectors for the inputs.

Note

AI_SIMILARITY does not support computing the similarity between text and image inputs. Both inputs must be of the same type:

>   * They are both Python `str`.
> 
>   * They are both `Column` objects representing a FILE data type.
> 
> 


Examples:
[code] 
    >>> # Text similarity
    >>> df = session.range(1).select(ai_similarity('I like this dish', 'This dish is very good').alias("similarity"))
    >>> df.collect()[0][0] > 0.8
    True
    
    >>> # Text similarity with custom model
    >>> df = session.range(1).select(
    ...     ai_similarity(
    ...         'I love programming',
    ...         '我喜欢编程',
    ...         model='multilingual-e5-large'
    ...     ).alias("similarity")
    ... )
    >>> df.collect()[0][0] > 0.8
    True
    
    >>> # Using columns
    >>> df = session.create_dataframe([
    ...     ['Hello world', 'Hi there'],
    ...     ['Good morning', 'Good evening'],
    ... ], schema=["text1", "text2"])
    >>> df = df.select(ai_similarity(col("text1"), col("text2")).alias("similarity"))
    >>> result = df.collect()
    >>> result[0][0] < 0.6
    True
    >>> result[1][0] > 0.7
    True
    
    >>> # Image similarity
    >>> _ = session.sql("CREATE OR REPLACE TEMP STAGE mystage ENCRYPTION = (TYPE = 'SNOWFLAKE_SSE')").collect()
    >>> _ = session.file.put("tests/resources/dog.jpg", "@mystage", auto_compress=False)
    >>> _ = session.file.put("tests/resources/cat.jpeg", "@mystage", auto_compress=False)
    >>> df = session.range(1).select(
    ...     ai_similarity(
    ...         to_file("@mystage/dog.jpg"),
    ...         to_file("@mystage/cat.jpeg")
    ...     ).alias("similarity")
    ... )
    >>> df.collect()[0][0] < 0.5
    True
    
[/code]
