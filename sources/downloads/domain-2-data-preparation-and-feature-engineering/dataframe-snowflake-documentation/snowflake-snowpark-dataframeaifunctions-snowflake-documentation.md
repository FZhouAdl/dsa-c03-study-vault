---
title: "snowflake.snowpark.DataFrameAIFunctions | Snowflake Documentation"
source: https://docs.snowflake.com/en/developer-guide/snowpark/reference/python/latest/snowpark/api/snowflake.snowpark.DataFrameAIFunctions.html
cert_domain: domain-2-data-preparation-and-feature-engineering
crawl_depth: 1
crawled: 2026-08-23
---

# snowflake.snowpark.DataFrameAIFunctions¶

_class _snowflake.snowpark.DataFrameAIFunctions(_dataframe : [snowflake.snowpark.DataFrame](snowflake.snowpark.DataFrame.html#snowflake.snowpark.DataFrame "snowflake.snowpark.DataFrame")_)[[source]](https://github.com/snowflakedb/snowpark-python/blob/v1.54.0/src/snowflake/snowpark/dataframe_ai_functions.py#L43-L2198)¶
    

Bases: `object`

Provides AI-powered functions for a [`DataFrame`](snowflake.snowpark.DataFrame.html#snowflake.snowpark.DataFrame "snowflake.snowpark.DataFrame").

Methods

[`agg`](snowflake.snowpark.DataFrameAIFunctions.agg.html#snowflake.snowpark.DataFrameAIFunctions.agg "snowflake.snowpark.DataFrameAIFunctions.agg")(task_description, input_column, *[, ...]) | Aggregate a column of text data using a natural language task description.  
---|---  
[`classify`](snowflake.snowpark.DataFrameAIFunctions.classify.html#snowflake.snowpark.DataFrameAIFunctions.classify "snowflake.snowpark.DataFrameAIFunctions.classify")(input_column, categories, *[, ...]) | Classify text or images into specified categories using AI.  
[`complete`](snowflake.snowpark.DataFrameAIFunctions.complete.html#snowflake.snowpark.DataFrameAIFunctions.complete "snowflake.snowpark.DataFrameAIFunctions.complete")(prompt, input_columns, model, *[, ...]) | Generate a response (completion) on each row using the specified language model.  
[`count_tokens`](snowflake.snowpark.DataFrameAIFunctions.count_tokens.html#snowflake.snowpark.DataFrameAIFunctions.count_tokens "snowflake.snowpark.DataFrameAIFunctions.count_tokens")(function_name, prompt, *[, ...]) | Count the number of tokens in text for a specified AI function.  
[`embed`](snowflake.snowpark.DataFrameAIFunctions.embed.html#snowflake.snowpark.DataFrameAIFunctions.embed "snowflake.snowpark.DataFrameAIFunctions.embed")(input_column, model, *[, output_column]) | Generate embedding vectors from text or images.  
[`extract`](snowflake.snowpark.DataFrameAIFunctions.extract.html#snowflake.snowpark.DataFrameAIFunctions.extract "snowflake.snowpark.DataFrameAIFunctions.extract")(input_column, *[, response_format, ...]) | Extract structured information from text or files using a response schema.  
[`filter`](snowflake.snowpark.DataFrameAIFunctions.filter.html#snowflake.snowpark.DataFrameAIFunctions.filter "snowflake.snowpark.DataFrameAIFunctions.filter")(predicate, input_columns, *) | Filter rows using AI-powered boolean classification.  
`multi_embed`(input_column, model, *[, ...]) | Generate multimodal embedding vectors from text, images, audio, or video.  
[`parse_document`](snowflake.snowpark.DataFrameAIFunctions.parse_document.html#snowflake.snowpark.DataFrameAIFunctions.parse_document "snowflake.snowpark.DataFrameAIFunctions.parse_document")(input_column, *[, ...]) | Extract content from a document (OCR or layout parsing) as JSON text.  
`redact`(input_column, *[, categories, mode, ...]) | Detect and redact personally identifiable information (PII) from text.  
[`sentiment`](snowflake.snowpark.DataFrameAIFunctions.sentiment.html#snowflake.snowpark.DataFrameAIFunctions.sentiment "snowflake.snowpark.DataFrameAIFunctions.sentiment")(input_column[, categories, ...]) | Extract sentiment analysis from text content.  
[`similarity`](snowflake.snowpark.DataFrameAIFunctions.similarity.html#snowflake.snowpark.DataFrameAIFunctions.similarity "snowflake.snowpark.DataFrameAIFunctions.similarity")(input1, input2, *[, output_column]) | Compute similarity scores between two columns using AI-powered embeddings.  
[`split_text_markdown_header`](snowflake.snowpark.DataFrameAIFunctions.split_text_markdown_header.html#snowflake.snowpark.DataFrameAIFunctions.split_text_markdown_header "snowflake.snowpark.DataFrameAIFunctions.split_text_markdown_header")(text_to_split, ...) | Split Markdown-formatted text into structured chunks based on header levels.  
[`split_text_recursive_character`](snowflake.snowpark.DataFrameAIFunctions.split_text_recursive_character.html#snowflake.snowpark.DataFrameAIFunctions.split_text_recursive_character "snowflake.snowpark.DataFrameAIFunctions.split_text_recursive_character")(...[, ...]) | Split text into chunks using recursive character-based splitting.  
[`summarize_agg`](snowflake.snowpark.DataFrameAIFunctions.summarize_agg.html#snowflake.snowpark.DataFrameAIFunctions.summarize_agg "snowflake.snowpark.DataFrameAIFunctions.summarize_agg")(input_column, *[, output_column]) | Summarize a column of text data using AI.  
[`transcribe`](snowflake.snowpark.DataFrameAIFunctions.transcribe.html#snowflake.snowpark.DataFrameAIFunctions.transcribe "snowflake.snowpark.DataFrameAIFunctions.transcribe")(input_column, *[, output_column, ...]) | Transcribe text from an audio file with optional timestamps and speaker labels.  
`translate`(input_column, source_language, ...) | Translate text from one language to another.
