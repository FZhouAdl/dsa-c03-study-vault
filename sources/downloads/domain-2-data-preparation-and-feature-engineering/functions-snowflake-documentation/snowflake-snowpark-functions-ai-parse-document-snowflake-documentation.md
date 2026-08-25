---
title: "snowflake.snowpark.functions.ai_parse_document | Snowflake Documentation"
source: https://docs.snowflake.com/en/developer-guide/snowpark/reference/python/latest/snowpark/api/snowflake.snowpark.functions.ai_parse_document.html
cert_domain: domain-2-data-preparation-and-feature-engineering
crawl_depth: 1
crawled: 2026-08-23
---

# snowflake.snowpark.functions.ai_parse_document¶

snowflake.snowpark.functions.ai_parse_document(_file : [Column](snowflake.snowpark.Column.html#snowflake.snowpark.Column "snowflake.snowpark.column.Column")_, _return_error_details : Optional[bool] = None_, _** kwargs_) → [Column](snowflake.snowpark.Column.html#snowflake.snowpark.Column "snowflake.snowpark.column.Column")[[source]](https://github.com/snowflakedb/snowpark-python/blob/v1.54.0/src/snowflake/snowpark/functions.py#L13596-L13718)¶
    

Returns the extracted content from a document as a JSON-formatted string. This function supports two types of extraction: Optical Character Recognition (OCR), and layout.

Parameters:
    

  * **file** – A FILE type column containing the document to parse. The document must be on a Snowflake stage that uses server-side encryption and is accessible to the user.

  * **return_error_details** – When `True`, returns an OBJECT with `value`, `error`, and `metadata` fields. `metadata` contains document-level metadata such as page count that is otherwise not surfaced.

  * ****kwargs** – 

Configuration settings specified as key/value pairs. Supported keys:

    * mode: Specifies the parsing mode. Supported modes are:
    
      * ’OCR’: The function extracts text only. This is the default mode.

      * ’LAYOUT’: The function extracts layout as well as text, including structural content such as tables.

    * extract_images: If set to True, the function extracts images embedded in the document. Requires `mode='LAYOUT'`. Each extracted image is returned in the `images` array with fields `id`, bounding-box coordinates, and `image_base64`.

    * page_split: If set to True, the function splits the document into pages and processes each page separately. This feature supports only PDF, PowerPoint (.pptx), and Word (.docx) documents. Documents in other formats return an error. The default is False. Tip: To process long documents that exceed the token limit, set this option to True.

    * page_filter: An array of page-range objects specifying which pages of a multi-page document to process. Each object must have `start` (inclusive, 0-based) and `end` (exclusive) fields. For example, `[{'start': 0, 'end': 2}]` processes the first two pages. Specifying `page_filter` implies `page_split`.



Returns:
    

A JSON object (as a string) that contains the extracted data and associated metadata. The options argument determines the structure of the returned object.

If `page_split` is set (or `page_filter` is provided), the output contains:
    

  * pages: An array of JSON objects, each containing text extracted from the document.



If `page_split` is False or not present, the output contains:
    

  * content: Plain text (in OCR mode) or Markdown-formatted text (in LAYOUT mode).



If `extract_images` is True (requires `mode='LAYOUT'`), the output additionally contains:
    

  * images: An array of objects, each with `id`, `top_left_x`, `top_left_y`, `bottom_right_x`, `bottom_right_y`, and `image_base64` fields.




Examples:
[code] 
    >>> import json
    >>> # Parse a PDF document with default OCR mode
    >>> _ = session.sql("CREATE OR REPLACE TEMP STAGE mystage ENCRYPTION = (TYPE = 'SNOWFLAKE_SSE')").collect()
    >>> _ = session.file.put("tests/resources/doc.pdf", "@mystage", auto_compress=False)
    >>> df = session.range(1).select(
    ...     ai_parse_document(to_file("@mystage/doc.pdf")).alias("parsed_content")
    ... )
    >>> result = json.loads(df.collect()[0][0])
    >>> "Sample PDF" in result["content"]
    True
    
    >>> # Parse with LAYOUT mode to extract tables and structure
    >>> _ = session.file.put("tests/resources/invoice.pdf", "@mystage", auto_compress=False)
    >>> df = session.range(1).select(
    ...     ai_parse_document(
    ...         to_file("@mystage/invoice.pdf"),
    ...         mode='LAYOUT'
    ...     ).alias("parsed_content")
    ... )
    >>> result = json.loads(df.collect()[0][0])
    >>> "| Customer Name |" in result["content"] and "| Country |" in result["content"]  # Markdown format 
    True
    
    >>> # Parse with page splitting for documents
    >>> df = session.range(1).select(
    ...     ai_parse_document(
    ...         to_file("@mystage/doc.pdf"),
    ...         page_split=True
    ...     ).alias("parsed_content")
    ... )
    >>> result = json.loads(df.collect()[0][0])
    >>> len(result["pages"]) 
    3
    >>> 'Sample PDF' in result["pages"][0]["content"] 
    True
    >>> result["pages"][0]["index"] 
    0
    
[/code]
