---
title: "snowflake.snowpark.functions.ai_embed | Snowflake Documentation"
source: https://docs.snowflake.com/en/developer-guide/snowpark/reference/python/latest/snowpark/api/snowflake.snowpark.functions.ai_embed.html
cert_domain: domain-2-data-preparation-and-feature-engineering
crawl_depth: 1
crawled: 2026-08-23
---

# snowflake.snowpark.functions.ai_embed¶

snowflake.snowpark.functions.ai_embed(_model : str_, _input : Union[[Column](snowflake.snowpark.Column.html#snowflake.snowpark.Column "snowflake.snowpark.column.Column"), str]_) → [Column](snowflake.snowpark.Column.html#snowflake.snowpark.Column "snowflake.snowpark.column.Column")[[source]](https://github.com/snowflakedb/snowpark-python/blob/v1.54.0/src/snowflake/snowpark/functions.py#L14072-L14146)¶
    

Creates an embedding vector from text or an image.

Parameters:
    

  * **model** – 

A string specifying the vector embedding model to be used. Supported models:

For text embeddings:
    
    * ’snowflake-arctic-embed-l-v2.0’: Arctic large model (default for text)

    * ’snowflake-arctic-embed-l-v2.0-8k’: Arctic large model with 8K context

    * ’nv-embed-qa-4’: NVIDIA embedding model for Q&A

    * ’multilingual-e5-large’: Multilingual embedding model

    * ’voyage-multilingual-2’: Voyage multilingual model

For image embeddings:
    
    * ’voyage-multimodal-3’: Voyage multimodal model (only for images)

  * **input** – The string or image (as a FILE object) to generate an embedding from. Can be a string with text or a FILE column containing an image.



Returns:
    

A VECTOR containing the embedding representation of the input.

Examples:
[code] 
    >>> # Text embedding
    >>> df = session.range(1).select(
    ...     ai_embed('snowflake-arctic-embed-l-v2.0', 'Hello, world!').alias("embedding")
    ... )
    >>> result = df.collect()[0][0]
    >>> len(result) > 0
    True
    
    >>> # Text embedding with multilingual model
    >>> df = session.create_dataframe([
    ...     ['Hello world'],
    ...     ['Bonjour le monde'],
    ...     ['Hola mundo']
    ... ], schema=["text"])
    >>> df = df.select(
    ...     col("text"),
    ...     ai_embed('multilingual-e5-large', col("text")).alias("embedding")
    ... )
    >>> embeddings = df.collect()
    >>> all(len(row[1]) > 0 for row in embeddings)
    True
    
    >>> # Image embedding
    >>> _ = session.sql("CREATE OR REPLACE TEMP STAGE mystage ENCRYPTION = (TYPE = 'SNOWFLAKE_SSE')").collect()
    >>> _ = session.file.put("tests/resources/dog.jpg", "@mystage", auto_compress=False)
    >>> df = session.range(1).select(
    ...     ai_embed('voyage-multimodal-3', to_file('@mystage/dog.jpg')).alias("image_embedding")
    ... )
    >>> result = df.collect()[0][0]
    >>> len(result) > 0
    True
    
[/code]
