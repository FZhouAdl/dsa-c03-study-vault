---
title: "AI_EXTRACT | Snowflake Documentation"
source: https://docs.snowflake.com/sql-reference/functions/ai_extract
cert_domain: domain-1-data-science-concepts
crawl_depth: 1
crawled: 2026-08-23
---

Categories:
    

[String & binary functions](/sql-reference/functions-string) (AI Functions)

# AI_EXTRACT¶

Extracts information from an input string or file.

## Syntax¶

**Extract information from an input string:**
[code] 
    AI_EXTRACT( <text>, <responseFormat> )
    
[/code]
[code] 
    AI_EXTRACT( text => <text>,
                responseFormat => <responseFormat>,
                [ scores => TRUE | FALSE ] )
    
[/code]

**Extract information from a file:**
[code] 
    AI_EXTRACT( <file>, <responseFormat> )
    
[/code]
[code] 
    AI_EXTRACT( file => <file>,
                responseFormat => <responseFormat>,
                [ config => <config_object> ],
                [ scores => TRUE | FALSE ] )
    
[/code]

## Arguments¶

`_text_`
    

An input string for extraction.

`_file_`
    

A [FILE](/sql-reference/data-types-unstructured#label-data-types-file) for extraction.

Supported file formats:

  * PDF
  * PNG
  * PPTX, PPT
  * EML
  * DOC, DOCX
  * JPEG, JPG
  * HTM, HTML
  * TEXT, TXT
  * TIF, TIFF
  * BMP, GIF, WEBP
  * MD



The files must be less than 100 MB in size.

`_responseFormat_`
    

Information to be extracted. The format depends on the type of extraction.

**Entity extraction formats**

Extract single values by providing one of the following formats:

  * Simple object schema that maps the label and information to be extracted:
[code] {'name': 'What is the last name of the employee?', 'address': 'What is the address of the employee?'}
        
[/code]

  * An array of strings that contain the information to be extracted:
[code] ['What is the last name of the employee?', 'What is the address of the employee?']
        
[/code]

  * An array of arrays that contain two strings (label and the information to be extracted):
[code] [['name', 'What is the last name of the employee?'], ['address', 'What is the address of the employee?']]
        
[/code]

  * A JSON schema with `'type': 'string'` on the sub-object:
[code] {
          'schema': {
         'type': 'object',
         'properties': {
           'title': {
             'description': 'What is the title of the document?',
             'type': 'string'
           }
         }
          }
        }
        
[/code]




**List extraction format**

Extract arrays of values using a JSON schema with `'type': 'array'` on the sub-object:
[code]
    {
      'schema': {
        'type': 'object',
        'properties': {
          'employees': {
            'description': 'What are the names of employees?',
            'type': 'array'
          }
        }
      }
    }
    
[/code]

**Table extraction format**

Extract tabular data using a JSON schema with `'type': 'object'` and `column_ordering`. Each column is defined as a nested property with `'type': 'array'` and a `description` that matches the column name in the file:
[code]
    {
      'schema': {
        'type': 'object',
        'properties': {
          'income_table': {
            'description': 'Income for FY2026Q2',
            'type': 'object',
            'column_ordering': ['month', 'income'],
            'properties': {
              'month': {
                'description': 'Month',
                'type': 'array'
              },
              'income': {
                'description': 'Income',
                'type': 'array'
              }
            }
          }
        }
      }
    }
    
[/code]

Note

  * You can’t combine the JSON schema format with other response formats. If `responseFormat` contains the `schema` key, you must define all questions within the JSON schema. Additional keys are not supported.

  * The model only accepts certain shapes of JSON schema. Top level type must always be an object, which contains independently extracted sub-objects. Sub-objects may be a table (object of lists of strings representing columns), a list of strings, or a string.

String is currently the only supported scalar type.

  * Use the `description` field to provide context to the model; for example, to help the model localize the right table in a document. You can enter the column header name, or describe the column in another way.

  * Use the `column_ordering` field to specify the order of all columns in the extracted table. The `column_ordering` field is case-sensitive and must match the column names defined in the `properties` field. The order should reflect the order of the columns in the document.




`scores => { boolean }`
    

Optional. Supported only in the named-argument syntax shown above. A BOOLEAN that controls whether the function returns scores for extracted values. The default is `FALSE`. When `TRUE`, the JSON result includes a `scoring` object in addition to `response`. For the output format, SQL examples, and limitations, see Extraction scores.

`config => _config_object_`
    

An [OBJECT](/sql-reference/data-types-semistructured#label-data-type-object) value that specifies the configuration settings. You can use an [OBJECT constant](/sql-reference/data-types-semistructured#label-object-constant) to specify this object.

You can specify the following key-value pairs in this object:

Key| Description  
---|---  
`scale_factor`| A numeric value from 1.0 through 4.0. Scales pages of an input file before they are processed by the underlying model, which can enhance OCR quality and improve extraction results.Use `scale_factor` if you receive unexpected or unclear responses in the following scenarios:

  * Documents with page sizes larger than A4
  * Documents containing small text, detailed visual elements, or dense layouts
  * Extracted text contains typos or character-level OCR errors



> If omitted, AI_EXTRACT uses the default value (`'scale_factor': 1.0`).  
  
## Returns¶

A JSON object containing the extracted information. The structure of the response depends on the type of extraction.

### Entity extraction¶

Returns a JSON object with key-value pairs for each extracted entity:
[code] 
    {
      "error": null,
      "response": {
        "title": "Financial report"
      }
    }
    
[/code]

### List extraction¶

Returns a JSON object with arrays of extracted values:
[code] 
    {
      "error": null,
      "response": {
        "employees": [
          "Smith",
          "Johnson",
          "Doe"
        ]
      }
    }
    
[/code]

### Table extraction¶

Returns a JSON object with column arrays representing the extracted table:
[code] 
    {
      "error": null,
      "response": {
        "income_table": {
          "income": ["$120 678","$130 123","$150 998"],
          "month": ["February", "March", "April"]
        }
      }
    }
    
[/code]

### Combined extraction¶

When extracting entities, lists, and tables in a single call, the response contains all extraction types:
[code] 
    {
      "error": null,
      "response": {
        "employees": [
          "Smith",
          "Johnson",
          "Doe"
        ],
        "income_table": {
          "income": ["$120 678","$130 123","$150 998"],
          "month": ["February", "March", "April"]
        },
        "title": "Financial report"
      }
    }
    
[/code]

## Extraction scores¶

When you use AI_EXTRACT, you can request scores that indicate the model’s certainty about each extracted value. You can use these scores to set thresholds for business logic, such as flagging low-scoring extractions for human review.

A higher score indicates a higher likelihood that the extracted value is correct. You can compare scores for extracting a given entity across different documents to identify which values are more or less reliable, and use them to build deterministic processing logic such as thresholds, fallback mechanisms, and human-in-the-loop workflows.

### How scores work¶

When you set the `scores` parameter to `TRUE`, AI_EXTRACT returns a `scoring` object alongside the standard `response` object. The `scoring` object contains a score for each extracted field.

The `scores` parameter is optional, and it is set to `FALSE` by default. Use the optional `scores` argument in the named-argument syntax shown in Arguments.

### Scoring output format¶

When `scores => TRUE`, the returned JSON includes a `scoring` object:
[code] 
    {
      "response": {
        "name": "John Smith",
        "address": "123 Main St, San Francisco"
      },
      "scoring": {
        "scores": {
          "name": {
            "score": 0.95
          },
          "address": {
            "score": 0.82
          }
        }
      },
      "error": null
    }
    
[/code]

Each field in `scoring.scores` corresponds to a field in `response` and contains a `score` value between 0 and 1.

For list extraction, the `scoring` object returns an aggregate score for the entire list:
[code] 
    {
      "response": {
        "employees": ["Smith", "Johnson", "Doe"]
      },
      "scoring": {
        "scores": {
          "employees": {
            "score": 0.77
          }
        }
      },
      "error": null
    }
    
[/code]

For table extraction, the `scoring` object returns an aggregate score for the entire table:
[code] 
    {
      "response": {
        "income_table": {
          "month": ["February", "March", "April"],
          "income": ["$120 678", "$130 123", "$150 998"]
        }
      },
      "scoring": {
        "scores": {
          "income_table": {
            "score": 0.85
          }
        }
      },
      "error": null
    }
    
[/code]

### Scoring usage notes¶

  * Requesting scores does not incur additional cost. For general information on AI_EXTRACT costs, see Cost considerations.
  * Per-element scores for individual list items and table cells are not available.
  * Scores are supported for fine-tuned models.



### Examples with extraction scores¶

The following example extracts information from a file and returns scores for each extracted field:
[code] 
    SELECT AI_EXTRACT(
      file => TO_FILE('@db.schema.files', 'document.pdf'),
      responseFormat => {'name': 'What is the last name of the employee?', 'date': 'What is the inspection date?'},
      scores => TRUE
    );
    
[/code]

Result:
[code] 
    {
      "response": {
        "date": "2022-04-01",
        "name": "Johnson"
      },
      "scoring": {
        "scores": {
          "date": {
            "score": 0.96
          },
          "name": {
            "score": 0.99
          }
        }
      },
      "error": null
    }
    
[/code]

The following example extracts a list of buyer names and returns an aggregate score:
[code] 
    SELECT AI_EXTRACT(
      file => TO_FILE('@db.schema.files', 'agreement.pdf'),
      responseFormat => {
        'schema': {
          'type': 'object',
          'properties': {
            'buyer_list': {
              'description': 'What are the buyer names?',
              'type': 'array'
            }
          }
        }
      },
      scores => TRUE
    );
    
[/code]

Result:
[code] 
    {
      "response": {
        "buyer_list": [
          "John Davis",
          "Jane Davis"
        ]
      },
      "scoring": {
        "scores": {
          "buyer_list": {
            "score": 0.91
          }
        }
      },
      "error": null
    }
    
[/code]

The following example extracts a table and returns an aggregate score:
[code] 
    SELECT AI_EXTRACT(
      file => TO_FILE('@db.schema.files', 'report.pdf'),
      responseFormat => {
        'schema': {
          'type': 'object',
          'properties': {
            'income_table': {
              'description': 'Income for FY2026Q2',
              'type': 'object',
              'column_ordering': ['month', 'income'],
              'properties': {
                'month': {
                  'description': 'Month',
                  'type': 'array'
                },
                'income': {
                  'description': 'Income',
                  'type': 'array'
                }
              }
            }
          }
        }
      },
      scores => TRUE
    );
    
[/code]

Result:
[code] 
    {
      "response": {
        "income_table": {
          "income": ["$120 678", "$130 123", "$150 998"],
          "month": ["February", "March", "April"]
        }
      },
      "scoring": {
        "scores": {
          "income_table": {
            "score": 0.88
          }
        }
      },
      "error": null
    }
    
[/code]

## Access control requirements¶

Users must use a role that has been granted the [SNOWFLAKE.CORTEX_USER database role](/sql-reference/snowflake-db-roles#label-snowflake-db-roles-cortex-user). For information about granting this privilege, see [Cortex LLM privileges](/user-guide/snowflake-cortex/aisql-privileges-and-access#label-cortex-llm-privileges).

## Usage notes¶

  * AI_EXTRACT is optimized for both digital-born and scanned documents.

  * You can’t use both `text` and `file` parameters simultaneously in the same function call.

  * You can either ask questions in natural language or describe information to be extracted (such as city, street, ZIP code); for example:
[code] {'address': 'City, street, ZIP', 'name': 'First and last name'}
        
[/code]

  * The following languages are supported:




  * Arabic
  * Bengali
  * Burmese
  * Cebuano
  * Chinese
  * Czech
  * Dutch
  * English
  * French
  * German
  * Hebrew
  * Hindi
  * Indonesian
  * Italian
  * Japanese
  * Khmer
  * Korean
  * Lao
  * Malay
  * Persian
  * Polish
  * Portuguese
  * Russian
  * Spanish
  * Tagalog
  * Thai
  * Turkish
  * Urdu
  * Vietnamese



  * The documents must be no more than 125 pages long.

  * In a single AI_EXTRACT call, you can ask a maximum of 100 questions for entity extraction, and a maximum of 10 questions for table extraction.

A table extraction question is equal to 10 entity extraction questions. For example, you can ask 4 table extraction questions and 60 entity extraction questions in a single AI_EXTRACT call.

  * The maximum output length for entity extraction is 512 tokens per question. For table extraction, the model returns answers that are a maximum of 4096 tokens.

  * You can request optional extraction scores by using named arguments and passing `scores => TRUE`. For details, see Extraction scores.




AI_EXTRACT supports documents on stages that use client-side or server-side encryption, including in accounts that use PrivateLink or other [network policies](/user-guide/network-policies) that restrict public network access to stages. Files on external stages that use client-side encryption configured in Snowflake (for example, by using a COPY INTO statement) are supported.

## Cost considerations¶

  * The Cortex AI_EXTRACT function incurs compute cost based on the number of pages per document, input prompt tokens, and output tokens processed.

    * For paged file formats (PDF, DOCX, TIF, TIFF), each page is counted as 970 tokens.
    * For image file formats (JPEG, JPG, PNG), each individual image file is billed as a page and counted as 970 tokens.
  * Using the `scale_factor` parameter changes how many tokens are consumed and how many pages can be processed per call:

    * The number of input tokens consumed increases proportionally with `scale_factor`.
    * The maximum number of pages per document that can be processed by AI_EXTRACT decreases by `scale_factor`.

**Relationship of scale_factor to number of tokens and pages**

> `scale_factor` value| Token count per page| Max. number of pages per document  
> ---|---|---  
> 2| 970 * 2 = 1940 tokens| 125/2 = 62.5 (rounded down to 62)  
> 2.5| 970 * 2.5 = 2425 tokens| 125/2.5 = 50  
> 4| 970 * 4 = 3880 tokens| 125/4 = 31.25 (rounded down to 31)  
  
  * Snowflake recommends executing queries that call the Cortex AI_EXTRACT function in a smaller warehouse (no larger than MEDIUM). Larger warehouses don’t increase performance.




## Regional availability¶

AI_EXTRACT is available to accounts in the following regions:

Cloud platform| Region name  
---|---  
Amazon Web Services (AWS)| 

  * US East (N. Virginia)
  * US West (Oregon)
  * Canada (Central)
  * South America (Sao Paulo)
  * EU (Ireland)
  * EU (Frankfurt)
  * Asia Pacific (Tokyo)
  * Asia Pacific (Sydney)

  
Microsoft Azure| 

  * East US 2 (Virginia)
  * West US 2 (Washington)
  * South Central US (Texas)
  * North Europe (Ireland)
  * West Europe (Netherlands)
  * Southeast Asia (Singapore)
  * Australia East (New South Wales)
  * Central India (Pune)
  * Japan East (Tokyo)

  
Google Cloud Platform| 

  * Middle East Central2 (Dammam)

  
  
AI_EXTRACT has cross-region support. For information on enabling Cortex AI cross-region support, see [Cross-region inference](/user-guide/snowflake-cortex/cross-region-inference).

## Error conditions¶

AI_EXTRACT can produce the following error messages:

Message| Explanation  
---|---  
`Internal error.`| A system error occurred. Wait and try again. If the error persists, contact Snowflake support.  
`Not found.`| The file was not found.  
`Provided file cannot be found.`| The file was not found.  
`Provided file cannot be accessed.`| The current user does not have sufficient privileges to access the file.  
`The provided file format {file_extension} isn't supported.`| The document is not in a supported format.  
`The provided file isn't in the expected format or is corrupted.`| The document is corrupted or isn’t in a supported format.  
`Empty request.`| No parameters were provided.  
`Missing or empty response format.`| No response format was provided.  
`Invalid response format.`| The response format is not valid JSON.  
`Duplicate feature name found: {feature_name}.`| The response format contains one or more duplicate feature names.  
`Too many questions: {number} complex and {number} simple = {number} total, complex question weight {number}`.| The number of questions exceeds the allowed limit.  
`Maximum number of 125 pages exceeded. The document has {actual_pages} pages.`| The document exceeds the 125-page limit.  
`Page size in pixels exceeds 10000x10000. The page size is {actual_px} pixels.`| Image input or a converted document page is larger than the supported dimensions.  
`Page size in inches exceeds 50x50 (3600x3600 pt). The page size is {actual_in} inches ({actual_pt} pt).`| Page is larger than the supported dimensions.  
`Maximum file size of 104857600 bytes exceeded. The file size is {actual_size} bytes.`| The document is larger than 100 MB.  
  
## Examples¶

### Entity extraction¶

  * The following example extracts entities from the input text using a simple object schema:
[code] SELECT AI_EXTRACT(
          text => 'John Smith lives in San Francisco and works for Snowflake',
          responseFormat => {'name': 'What is the first name of the employee?', 'city': 'What is the address of the employee?'}
        );
        
[/code]

  * The following example extracts and parses entities from the input text:
[code] SELECT AI_EXTRACT(
          text => 'John Smith lives in San Francisco and works for Snowflake',
          responseFormat => PARSE_JSON('{"name": "What is the first name of the employee?", "address": "What is the address of the employee?"}')
        );
        
[/code]

  * The following example extracts entities from the `document.pdf` file:
[code] SELECT AI_EXTRACT(
          file => TO_FILE('@db.schema.files','document.pdf'),
          responseFormat => [['name', 'What is the first name of the employee?'], ['city', 'Where does the employee live?']]
        );
        
[/code]

  * The following example extracts entities from all files in a directory on a stage:

Note

Ensure that the directory table is enabled. For more information, see [Manage directory tables](/user-guide/data-load-dirtables-manage).
[code] SELECT AI_EXTRACT(
          file => TO_FILE('@db.schema.files', relative_path),
          responseFormat => [
         'What is the document ID?',
         'What is the address of the company?'
          ]
        ) FROM DIRECTORY (@db.schema.files);
        
[/code]

  * The following example extracts the `title` entity from the `report.pdf` file using a JSON schema:
[code] SELECT AI_EXTRACT(
          file => TO_FILE('@db.schema.files', 'report.pdf'),
          responseFormat => {
         'schema': {
           'type': 'object',
           'properties': {
             'title': {
               'description': 'What is the title of document?',
               'type': 'string'
             }
           }
         }
          }
        );
        
[/code]




### List extraction¶

The following example extracts the `employees` list from the `report.pdf` file:
[code] 
    SELECT AI_EXTRACT(
      file => TO_FILE('@db.schema.files', 'report.pdf'),
      responseFormat => {
        'schema': {
          'type': 'object',
          'properties': {
            'employees': {
              'description': 'What are the surnames of employees?',
              'type': 'array'
            }
          }
        }
      }
    );
    
[/code]

### Table extraction¶

The following example extracts the `income_table` table from the `report.pdf` file:
[code] 
    SELECT AI_EXTRACT(
      file => TO_FILE('@db.schema.files', 'report.pdf'),
      responseFormat => {
        'schema': {
          'type': 'object',
          'properties': {
            'income_table': {
              'description': 'Income for FY2026Q2',
              'type': 'object',
              'column_ordering': ['month', 'income'],
              'properties': {
                'month': {
                  'description': 'Month',
                  'type': 'array'
                },
                'income': {
                  'description': 'Income',
                  'type': 'array'
                }
              }
            }
          }
        }
      }
    );
    
[/code]

### Combined extraction¶

The following example extracts a table (`income_table`), entity (`title`), and list (`employees`) from the `report.pdf` file in a single call:
[code] 
    SELECT AI_EXTRACT(
      file => TO_FILE('@db.schema.files', 'report.pdf'),
      responseFormat => {
        'schema': {
          'type': 'object',
          'properties': {
            'income_table': {
              'description': 'Income for FY2026Q2',
              'type': 'object',
              'column_ordering': ['month', 'income'],
              'properties': {
                'month': {
                  'description': 'Month',
                  'type': 'array'
                },
                'income': {
                  'description': 'Income',
                  'type': 'array'
                }
              }
            },
            'title': {
              'description': 'What is the title of document?',
              'type': 'string'
            },
            'employees': {
              'description': 'What are the surnames of employees?',
              'type': 'array'
            }
          }
        }
      }
    );
    
[/code]

### Extraction with a custom scale factor¶

The following example extracts the `employees` array from the `report.pdf` file using a scale factor of 2.0:
[code] 
    SELECT AI_EXTRACT(
      file => TO_FILE('@db.schema.files', 'report.pdf'),
      responseFormat => {
        'schema': {
          'type': 'object',
          'properties': {
            'employees': {
              'description': 'What are the surnames of employees?',
              'type': 'array'
            }
          }
        }
      },
      config => {'scale_factor': 2.0}
    );
    
[/code]

### Extraction using a fine-tuned `arctic-extract` model¶

To use the fine-tuned `arctic-extract` model for inference with the [AI_EXTRACT](/sql-reference/functions/ai_extract) function, specify the model using the `model` parameter as shown in the following example:
[code] 
    SELECT AI_EXTRACT(
      model => 'db.schema.my_tuned_model',
      file => TO_FILE('@db.schema.files','document.pdf')
    );
    
[/code]

You can overwrite questions used for fine-tuning by using the `responseFormat` parameter as shown in the following example:
[code] 
    SELECT AI_EXTRACT(
      model => 'db.schema.my_tuned_model',
      file => TO_FILE('@db.schema.files','document.pdf'),
      responseFormat => [['name', 'What is the first name of the employee?'], ['city', 'Where does the employee live?']]
    );
    
[/code]

The following example extracts data from the `invoice.pdf` file, using a fine-tuned `arctic-extract` model and a scale factor of 2.0:
[code] 
    SELECT AI_EXTRACT(
      model => 'db.schema.my_tuned_model',
      file => TO_FILE('@db.schema.files', 'invoice.pdf'),
      config => {'scale_factor': 2.0}
    );
    
[/code]

For more information, see [Fine-tuning arctic-extract models](/user-guide/snowflake-cortex/arctic-extract-finetuning).

Note

AI_EXTRACT is the updated version of [EXTRACT_ANSWER](/sql-reference/functions/extract_answer-snowflake-cortex). For the latest functionality, use AI_EXTRACT.

## Legal notices¶

Refer to [Snowflake AI and ML](/guides-overview-ai-features) for legal notices.
