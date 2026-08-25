---
title: "AI_SIMILARITY | Snowflake Documentation"
source: https://docs.snowflake.com/sql-reference/functions/ai_similarity
cert_domain: domain-1-data-science-concepts
crawl_depth: 1
crawled: 2026-08-23
---

Categories:
    

[String & binary functions](/sql-reference/functions-string) (AI Functions)

# AI_SIMILARITY¶

Computes a similarity score based on the vector cosine similarity value of the inputs’ embedding vectors. Currently supports both text and image similarity computation.

## Syntax¶

Applying AI_SIMILARITY to string or image inputs:
[code] 
    AI_SIMILARITY( <input1>, <input2> )
    
[/code]

Specifying the config object:
[code] 
    AI_SIMILARITY( <input1>, <input2>, <config_object> )
    
[/code]

## Arguments¶

**Required:**

If you’re specifying input strings:

`_input1_`, `_input2_`
    

The strings with the text that you’re comparing and using to compute the similarity score.

If you’re specifying input images:

`_input1_`, `_input2_`
    

[FILE data type](/user-guide/unstructured-intro#label-unstructured-data-file-data-type) referencing the images to be compared.

Note

AI_SIMILARITY does not support computing the similarity between text and image inputs.

**Optional:**

`config_object`
    

An [OBJECT](/sql-reference/data-types-semistructured#label-data-type-object) containing key-value pairs used to configure the model.

Key| Type| Default| Description  
---|---|---|---  
`model`| [STRING](/sql-reference/data-types-text#label-character-datatypes)| For STRING input, default to _‘snowflake-arctic-embed-l-v2.0’_. For IMAGE input, default to _‘voyage-multimodal-3’_|  The embedding model used for embedding. Supported values are:

  * `'snowflake-arctic-embed-l-v2.0'`
  * `'nv-embed-qa-4'`
  * `'multilingual-e5-large'`
  * `'voyage-multilingual-2'`
  * `'snowflake-arctic-embed-m-v1.5'`
  * `'snowflake-arctic-embed-m'`
  * `'e5-base-v2'`
  * `'voyage-multimodal-3'` (IMAGE)

  
  
## Returns¶

Returns a float value of range -1 to 1 that represents the similarity score computed using vector similarity between two embedding vectors for the inputs.

## Access control requirements¶

Users must use a role that has been granted the [SNOWFLAKE.CORTEX_USER database role](/sql-reference/snowflake-db-roles#label-snowflake-db-roles-cortex-user). See [Cortex LLM privileges](/user-guide/snowflake-cortex/aisql-privileges-and-access#label-cortex-llm-privileges) for more information on this privilege.

## Examples¶

### AI_SIMILARITY: Text¶

In this example, the function is computing a similarity score between the two statement inputs _‘I like this dish’_ and _‘This dish is very good’_.
[code] 
    SELECT AI_SIMILARITY('I like this dish', 'This dish is very good');
    
[/code]

We can also compute similarity on text columns.
[code] 
    SELECT
        review
    FROM restaurant_reviews
    ORDER BY AI_SIMILARITY(review, 'I love the food here!');
    
[/code]

### AI_SIMILARITY: Images¶

In this example, the function computes a similarity score between the two images, `cat.jpg` and `2cats.jpg`, stored in a Snowflake stage `@file_stage`.
[code] 
    SELECT AI_SIMILARITY(TO_FILE('@file_stage', 'cat.jpg'), TO_FILE('@file_stage', '2cats.jpg'));
    
[/code]

We can also compute similarity among the images using Snowflake Directory Table for the stage containing the images.
[code] 
    SELECT
        to_file('@file_stage', relative_path)
    FROM directory(@file_stage)
    WHERE AI_SIMILARITY(f, to_file(@file_stage, 'cat.jpg')) >= 0.5;
    
[/code]

## Limitations¶

  * Snowflake AI functions don’t work on FILEs created from stage files from the following stage types:
    * Internal stages with encryption mode `TYPE = 'SNOWFLAKE_FULL'`

    * External stages with any customer-side encrypted mode:

      * `TYPE = 'AWS_CSE'`
      * `TYPE = 'AZURE_CSE'`
    * User stage, table stage

    * Stage with double-quoted names




## Billing¶

_AI_SIMILARITY_ is currently billed under the _AI_EMBED_ line item in SNOWFLAKE.ACCOUNT_USAGE.CORTEX_FUNCTIONS_USAGE_HISTORY view.

## Legal notices¶

Refer to [Snowflake AI and ML](/guides-overview-ai-features) for legal notices.
