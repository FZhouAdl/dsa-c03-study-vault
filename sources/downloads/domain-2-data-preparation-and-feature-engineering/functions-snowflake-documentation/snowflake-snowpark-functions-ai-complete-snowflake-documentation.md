---
title: "snowflake.snowpark.functions.ai_complete | Snowflake Documentation"
source: https://docs.snowflake.com/en/developer-guide/snowpark/reference/python/latest/snowpark/api/snowflake.snowpark.functions.ai_complete.html
cert_domain: domain-2-data-preparation-and-feature-engineering
crawl_depth: 1
crawled: 2026-08-23
---

# snowflake.snowpark.functions.ai_complete¶

snowflake.snowpark.functions.ai_complete(_model : str_, _prompt : ColumnOrLiteralStr_, _model_parameters : Optional[dict] = None_, _response_format : Optional[dict] = None_, _show_details : Optional[bool] = None_, _return_error_details : Optional[bool] = None_, __emit_ast : bool = True_) → [Column](snowflake.snowpark.Column.html#snowflake.snowpark.Column "snowflake.snowpark.Column")[[source]](https://github.com/snowflakedb/snowpark-python/blob/v1.54.0/src/snowflake/snowpark/functions.py#L13888-L14069)¶
snowflake.snowpark.functions.ai_complete(_model : str_, _prompt : str_, _file : [Column](snowflake.snowpark.Column.html#snowflake.snowpark.Column "snowflake.snowpark.Column")_, _model_parameters : Optional[dict] = None_, _response_format : Optional[dict] = None_, _show_details : Optional[bool] = None_, _return_error_details : Optional[bool] = None_, __emit_ast : bool = True_) → [Column](snowflake.snowpark.Column.html#snowflake.snowpark.Column "snowflake.snowpark.Column")
    

Generates a response (completion) for a text prompt using a supported language model.

This function supports three main usage patterns: 1\. String prompt only: AI_COMPLETE(model, prompt, …) 2\. Prompt with file: AI_COMPLETE(model, prompt, file, …) 3\. Prompt object: AI_COMPLETE(model, prompt_object, …)

Parameters:
    

  * **model** – A string specifying the model to be used. Different input types have different supported models. See details in [AI_COMPLETE](https://docs.snowflake.com/en/sql-reference/functions/ai_complete).

  * **prompt** – A string prompt, or a Column object from [`prompt()`](snowflake.snowpark.functions.prompt.html#snowflake.snowpark.functions.prompt "snowflake.snowpark.functions.prompt").

  * **file** – The FILE type column representing an image. When provided, prompt should be a string.

  * **model_parameters** – 

Optional object containing model hyperparameters:

    * temperature: A value from 0 to 1 controlling randomness (default: `0`)

    * top_p: A value from 0 to 1 controlling diversity (default: `0`)

    * max_tokens: Maximum number of output tokens (default: `4096`, max: `8192`)

    * guardrails: Enable Cortex Guard filtering (default: `FALSE`)

  * **response_format** – Optional JSON schema that the response should follow for structured outputs.

  * **show_details** – Optional boolean flag to return detailed inference information (default: `FALSE`).

  * **return_error_details** – When `True`, returns an OBJECT with `value` and `error` fields instead of returning NULL on failure.



Returns:
    

`response_format` and `show_details` are only supported for single string. When `show_details` is `FALSE` and `response_format` is not specified: Returns a string containing the response. When `show_details` is `FALSE` and `response_format` is specified: Returns an object following the provided format. When `show_details` is `TRUE`: Returns a JSON object with detailed inference information including choices, created timestamp, model name, and token usage statistics. See details in [AI_COMPLETE Returns](https://docs.snowflake.com/en/sql-reference/functions/ai_complete-single-string#returns).

Examples:
[code] 
    >>> # Basic completion with string prompt
    >>> df = session.range(1).select(
    ...     ai_complete('llama3.1-8b', 'What are large language models?').alias("response")
    ... )
    >>> len(df.collect()[0][0]) > 10
    True
    
    >>> # Using model parameters
    >>> df = session.range(1).select(
    ...     ai_complete(
    ...         model='llama3.3-70b',
    ...         prompt='How does a snowflake get its unique pattern?',
    ...         model_parameters={
    ...             'temperature': 0.7,
    ...             'max_tokens': 100
    ...         }
    ...     ).alias("response")
    ... )
    >>> result = df.collect()[0][0]
    >>> len(result) > 0
    True
    
    >>> # With detailed output
    >>> df = session.range(1).select(
    ...     ai_complete(
    ...         model='mistral-large2',
    ...         prompt='Explain AI in one sentence.',
    ...         show_details=True
    ...     ).alias("detailed_response")
    ... )
    >>> result = df.collect()[0][0]
    >>> 'choices' in result and 'usage' in result
    True
    
    >>> # Prompt with image processing
    >>> _ = session.sql("CREATE OR REPLACE TEMP STAGE mystage ENCRYPTION = (TYPE = 'SNOWFLAKE_SSE')").collect()
    >>> _ = session.file.put("tests/resources/kitchen.png", "@mystage", auto_compress=False)
    >>> df = session.range(1).select(
    ...     ai_complete(
    ...         model='claude-4-sonnet',
    ...         prompt='Extract the kitchen appliances identified in this image. Respond in JSON only with the identified appliances.',
    ...         file=to_file('@mystage/kitchen.png'),
    ...     )
    ... )
    >>> result = df.collect()[0][0]
    >>> "microwave" in result and "refrigerator" in result
    True
    
    >>> # Structured output with response format
    >>> response_schema = {
    ...     'type': 'json',
    ...     'schema': {
    ...         'type': 'object',
    ...         'properties': {
    ...             'sentiment': {'type': 'string'},
    ...             'confidence': {'type': 'number'}
    ...         },
    ...         'required': ['sentiment', 'confidence']
    ...     }
    ... }
    >>> df = session.range(1).select(
    ...     ai_complete(
    ...         model='llama3.3-70b',
    ...         prompt='Analyze the sentiment of this text: I love this product!',
    ...         response_format=response_schema
    ...     ).alias("structured_result")
    ... )
    >>> result = df.collect()[0][0]
    >>> 'sentiment' in result and 'confidence' in result
    True
    
    >>> # Using prompt object from prompt() function
    >>> df = session.range(1).select(
    ...     ai_complete(
    ...         model='claude-4-sonnet',
    ...         prompt=prompt("Extract the kitchen appliances identified in this image. Respond in JSON only with the identified appliances? {0}", to_file('@mystage/kitchen.png')),
    ...     )
    ... )
    >>> result = df.collect()[0][0]
    >>> "microwave" in result and "refrigerator" in result
    True
    
[/code]
