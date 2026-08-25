---
title: "snowflake.snowpark.functions.ai_translate | Snowflake Documentation"
source: https://docs.snowflake.com/en/developer-guide/snowpark/reference/python/latest/snowpark/api/snowflake.snowpark.functions.ai_translate.html
cert_domain: domain-2-data-preparation-and-feature-engineering
crawl_depth: 1
crawled: 2026-08-23
---

# snowflake.snowpark.functions.ai_translate¶

snowflake.snowpark.functions.ai_translate(_text : Union[[Column](snowflake.snowpark.Column.html#snowflake.snowpark.Column "snowflake.snowpark.column.Column"), str]_, _source_language : Union[[Column](snowflake.snowpark.Column.html#snowflake.snowpark.Column "snowflake.snowpark.column.Column"), str]_, _target_language : Union[[Column](snowflake.snowpark.Column.html#snowflake.snowpark.Column "snowflake.snowpark.column.Column"), str]_, _return_error_details : Optional[bool] = None_) → [Column](snowflake.snowpark.Column.html#snowflake.snowpark.Column "snowflake.snowpark.column.Column")[[source]](https://github.com/snowflakedb/snowpark-python/blob/v1.54.0/src/snowflake/snowpark/functions.py#L14323-L14377)¶
    

Translates the given input text from one supported language to another.

Parameters:
    

  * **text** – A string or Column containing the text to be translated.

  * **source_language** – A string or Column specifying the language code for the source language. Specify an empty string `''` to automatically detect the source language.

  * **target_language** – A string or Column specifying the language code for the target language.

  * **return_error_details** – When `True`, returns an OBJECT with `value` and `error` fields instead of returning NULL on failure.



Returns:
    

A string containing a translation of the original text into the target language.

See details in [AI_TRANSLATE](https://docs.snowflake.com/en/sql-reference/functions/ai_translate).

Examples:
[code] 
    >>> # Translate literal text from English to German
    >>> df = session.range(1).select(
    ...     ai_translate('Hello world', 'en', 'de').alias('translation')
    ... )
    >>> df.collect()[0][0].lower()
    'hallo welt'
    
    >>> # Auto-detect source language and translate to English
    >>> df = session.range(1).select(
    ...     ai_translate('Hola mundo', '', 'en').alias('translation')
    ... )
    >>> df.collect()[0][0].lower()
    'hi world'
    
[/code]
