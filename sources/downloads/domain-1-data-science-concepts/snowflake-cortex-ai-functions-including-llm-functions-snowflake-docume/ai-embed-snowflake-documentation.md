---
title: "AI_EMBED | Snowflake Documentation"
source: https://docs.snowflake.com/sql-reference/functions/ai_embed
cert_domain: domain-1-data-science-concepts
crawl_depth: 1
crawled: 2026-08-23
---

Categories:
    

[String & binary functions](/sql-reference/functions-string) (AI Functions)

# AI_EMBED¶

Creates an embedding vector from text or an image. Embeddings are abstract numerical representations of the features of a piece of text or an image that can be used to determine the degree of similarity between pieces of text or images, which can be used for semantic search, clustering, classification, and other tasks.

The following table shows the regions where you can use the AI_EMBED function for text and images:

Data type| AWS US West 2 (Oregon)| AWS US East 1 (N. Virginia)| AWS Europe Central 1 (Frankfurt)| AWS Europe West 1 (Ireland)| AWS AP Southeast 2 (Sydney)| AWS AP Northeast 1 (Tokyo)| Azure East US 2 (Virginia)| Azure West Europe (Netherlands)| AWS (Cross-Region)  
---|---|---|---|---|---|---|---|---|---  
Text| ✔| ✔| ✔| ✔| ✔| ✔| ✔| ✔| ✔  
Image| ✔| ✔| ✔| | | | | ✔| ✔  
  
## Syntax¶
[code] 
    AI_EMBED( <model> , <input> )
    
[/code]

## Arguments¶

**Required:**

`_model_`
    

A string specifying the vector embedding model to be used to generate an embedding.

For text, you can provide the following values:

  * `snowflake-arctic-embed-l-v2.0`
  * `snowflake-arctic-embed-l-v2.0-8k`
  * `nv-embed-qa-4`
  * `multilingual-e5-large`
  * `voyage-multilingual-2`
  * `snowflake-arctic-embed-m-v1.5`
  * `snowflake-arctic-embed-m`
  * `e5-base-v2`



For images, you can provide only the following value:

  * `voyage-multimodal-3`



Supported models might have different [costs](/user-guide/snowflake-cortex/aisql-cost#label-cortex-llm-cost-considerations).

`_input_`
    

The string or image (as a [FILE object](/sql-reference/functions/to_file)) to generate an embedding from. Images must be:

  * In JPEG, WEBP, PNG, or BMP format
  * No more than 10 MB in size
  * No more than 8,000 x 8,000 pixels



## Returns¶

An embedding vector of type VECTOR derived from the input text or image.

## Access control requirements¶

You must use a role that has been granted the SNOWFLAKE.CORTEX_USER database role _or_ the SNOWFLAKE.CORTEX_EMBED_USER database role to call this function. See [Cortex LLM privileges](/user-guide/snowflake-cortex/aisql-privileges-and-access#label-cortex-llm-privileges) for more information on granting one of these privileges.

## Examples¶

### Text example¶

In this example, a vector embedding is generated for the phrase `hello world` using the `snowflake-arctic-embed-l-v2.0` model:
[code] 
    SELECT AI_EMBED('snowflake-arctic-embed-l-v2.0', 'hello world');
    
[/code]

In this example, a vector embedding is generated for a staged image using the `voyage-multimodal-3` model:
[code]
    SELECT AI_EMBED('voyage-multimodal-3',
            TO_FILE ('@my_images', 'CITY_WALKING1.PNG'));
    
[/code]

## Limitations¶

  * Snowflake AI functions don’t work on FILE objects created from files in the following kinds of stages:
    * Internal stages with encryption mode `TYPE = 'SNOWFLAKE_FULL'`
    * External stages with any customer-side encrypted mode, such as `AWS_CSE` or `AZURE_CSE`.
    * User stage
    * Table stage
    * Stage with double-quoted names



Note

AI_EMBED is the updated version of [EMBED_TEXT_1024](/sql-reference/functions/embed_text_1024-snowflake-cortex) and [EMBED_TEXT_768](/sql-reference/functions/embed_text-snowflake-cortex). For the latest functionality, use AI_EMBED.

## Legal notices¶

Refer to [Snowflake AI and ML](/guides-overview-ai-features).
