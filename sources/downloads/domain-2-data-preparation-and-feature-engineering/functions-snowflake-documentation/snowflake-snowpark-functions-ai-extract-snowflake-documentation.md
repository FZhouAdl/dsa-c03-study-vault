---
title: "snowflake.snowpark.functions.ai_extract | Snowflake Documentation"
source: https://docs.snowflake.com/en/developer-guide/snowpark/reference/python/latest/snowpark/api/snowflake.snowpark.functions.ai_extract.html
cert_domain: domain-2-data-preparation-and-feature-engineering
crawl_depth: 1
crawled: 2026-08-23
---

# snowflake.snowpark.functions.ai_extract¶

snowflake.snowpark.functions.ai_extract(_input : Union[[Column](snowflake.snowpark.Column.html#snowflake.snowpark.Column "snowflake.snowpark.column.Column"), str]_, _response_format : Union[dict, list]_, _scores : Optional[bool] = None_, _config : Optional[dict] = None_) → [Column](snowflake.snowpark.Column.html#snowflake.snowpark.Column "snowflake.snowpark.column.Column")[[source]](https://github.com/snowflakedb/snowpark-python/blob/v1.54.0/src/snowflake/snowpark/functions.py#L12927-L13151)¶
    

Extracts information from an input string or file based on the specified response format.

Parameters:
    

  * **input** – Either: \- A string or Column containing text to extract information from \- A FILE type Column representing a document to extract from (e.g. from [`to_file()`](snowflake.snowpark.functions.to_file.html#snowflake.snowpark.functions.to_file "snowflake.snowpark.functions.to_file"))

  * **response_format** – 

Information to be extracted in one of the following formats:

    * Simple object schema (dict) mapping feature names to extraction prompts: `{'name': 'What is the last name of the employee?', 'address': 'What is the address of the employee?'}`

    * Array of strings containing the information to be extracted: `['What is the last name of the employee?', 'What is the address of the employee?']`

    * Array of arrays containing two strings (feature name and extraction prompt): `[['name', 'What is the last name of the employee?'], ['address', 'What is the address of the employee?']]`

    * Array of strings with colon-separated feature names and extraction prompts: `['name: What is the last name of the employee?', 'address: What is the address of the employee?']`

  * **scores** – Optional boolean. When `True`, the returned JSON includes a `scoring` object alongside `response`. Each extracted field gets a `score` between 0 and 1 indicating the model’s confidence in the extracted value. Requires the named-argument SQL syntax; providing this parameter automatically switches to named-argument form. Default is `None` (no scores).

  * **config** – 

Optional dict of configuration settings for file inputs. Supported keys:

    * `scale_factor`: A numeric value from 1.0 through 4.0 that scales pages before processing, which can enhance OCR quality for dense layouts or small text.

Only valid when `input` is a FILE column. Requires the named-argument SQL syntax; providing this parameter automatically switches to named-argument form.



Returns:
    

A Column containing a JSON object with the extracted information. When `scores=True`, the object also includes a `scoring` sub-object with per-field confidence scores.

Note

  * You can either ask questions in natural language or describe information to be extracted (e.g., ‘City, street, ZIP’ instead of ‘What is the address?’)

  * To extract a list, add ‘List:’ at the beginning of each question

  * Maximum of 100 features can be extracted

  * Documents must be no more than 125 pages long

  * Maximum output length is 512 tokens per question




Supported file formats: PDF, PNG, PPTX, EML, DOC, DOCX, JPEG, JPG, HTM, HTML, TEXT, TXT, TIF, TIFF, BMP, GIF, WEBP, MD Files must be less than 100 MB in size.

Examples:
[code] 
    >>> # Extract from text string
    >>> df = session.range(1).select(
    ...     ai_extract(
    ...         'John Smith lives in San Francisco and works for Snowflake',
    ...         {'name': 'What is the first name of the employee?', 'city': 'What is the address of the employee?'}
    ...     ).alias("extracted")
    ... )
    >>> df.show()
    --------------------------------
    |"EXTRACTED"                   |
    --------------------------------
    |{                             |
    |  "error": null,              |
    |  "response": {               |
    |    "city": "San Francisco",  |
    |    "name": "John"            |
    |  }                           |
    |}                             |
    --------------------------------
    
    
    >>> # Extract using array format
    >>> df = session.create_dataframe(
    ...     ["Alice Johnson works in Seattle", "Bob Williams works in Portland"],
    ...     schema=["text"]
    ... )
    >>> extracted_df = df.select(
    ...     col("text"),
    ...     ai_extract(col("text"), [['name', 'What is the first name?'], ['city', 'What city do they work in?']]).alias("info")
    ... )
    >>> extracted_df.show()
    ------------------------------------------------------------
    |"TEXT"                          |"INFO"                   |
    ------------------------------------------------------------
    |Alice Johnson works in Seattle  |{                        |
    |                                |  "error": null,         |
    |                                |  "response": {          |
    |                                |    "city": "Seattle",   |
    |                                |    "name": "Alice"      |
    |                                |  }                      |
    |                                |}                        |
    |Bob Williams works in Portland  |{                        |
    |                                |  "error": null,         |
    |                                |  "response": {          |
    |                                |    "city": "Portland",  |
    |                                |    "name": "Bob"        |
    |                                |  }                      |
    |                                |}                        |
    ------------------------------------------------------------
    
    
    >>> # Extract lists using List: prefix
    >>> df = session.range(1).select(
    ...     ai_extract(
    ...         'Python, Java, and JavaScript are popular programming languages',
    ...         [['languages', 'List: What programming languages are mentioned?']]
    ...     ).alias("extracted")
    ... )
    >>> df.show()
    ----------------------
    |"EXTRACTED"         |
    ----------------------
    |{                   |
    |  "error": null,    |
    |  "response": {     |
    |    "languages": [  |
    |      "Python",     |
    |      "Java",       |
    |      "JavaScript"  |
    |    ]               |
    |  }                 |
    |}                   |
    ----------------------
    
    
    >>> # Extract from file
    >>> _ = session.sql("CREATE OR REPLACE TEMP STAGE mystage ENCRYPTION = (TYPE = 'SNOWFLAKE_SSE')").collect()
    >>> _ = session.file.put("tests/resources/invoice.pdf", "@mystage", auto_compress=False)
    >>> df = session.range(1).select(
    ...     ai_extract(
    ...         to_file('@mystage/invoice.pdf'),
    ...         [['date', 'What is the date of the invoice?'], ['amount', 'What is the amount of the invoice?']]
    ...     ).alias("extracted")
    ... )
    >>> df.show()
    --------------------------------
    |"EXTRACTED"                   |
    --------------------------------
    |{                             |
    |  "error": null,              |
    |  "response": {               |
    |    "amount": "USD $950.00",  |
    |    "date": "Nov 26, 2016"    |
    |  }                           |
    |}                             |
    --------------------------------
    
    
    >>> # Extract with confidence scores
    >>> df = session.range(1).select(
    ...     ai_extract(
    ...         'John Smith lives in San Francisco',
    ...         {'name': 'What is the name?', 'city': 'What is the city?'},
    ...         scores=True
    ...     ).alias("extracted")
    ... )
    >>> result = df.collect()[0][0]
    >>> 'scoring' in result
    True
    
[/code]
