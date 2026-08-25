---
title: "AI_SENTIMENT | Snowflake Documentation"
source: https://docs.snowflake.com/sql-reference/functions/ai_sentiment
cert_domain: domain-1-data-science-concepts
crawl_depth: 1
crawled: 2026-08-23
---

Categories:
    

[String & binary functions](/sql-reference/functions-string) (AI Functions)

# AI_SENTIMENT¶

Returns overall and category [sentiment](/user-guide/snowflake-cortex/ai-sentiment) in the given input text.

## Syntax¶
[code] 
    AI_SENTIMENT( <text> [ , <categories> ] [, <return_error_details> ] )
    
[/code]

## Arguments¶

**Required:**

`_text_`
    

A string containing the text in which sentiment is detected.

**Optional:**

`_categories_`
    

An array containing up to ten categories (also called entities or aspects) for which sentiment should be extracted. Each category is a string. For example, if extracting sentiment from a restaurant review, you might specify `['cost', 'quality', 'service', 'wait time']` as the categories. Each category may be a maximum of 30 characters long.

If you do not provide this argument, AI_SENTIMENT returns only the overall sentiment.

`_return_error_details_`
    

A BOOLEAN flag that indicates whether to return error details in case of error. When set to TRUE, the function returns an OBJECT that contains the value and the error message, one of which is NULL depending on whether the function succeeded or failed. See Error behavior for details.

## Returns¶

An OBJECT value containing a `categories` field. `categories` is an array of category records. Each category includes these fields:

  * `name`: The name of the category. The category names match the categories specified in the `categories` argument.
  * `sentiment`: The sentiment of the category. Each sentiment result is one of the following strings.
    * `unknown`: The category was not mentioned in the text.
    * `positive`: The category was mentioned positively in the text.
    * `negative`: The category was mentioned negatively in the text.
    * `neutral`: The category was mentioned in the text, but neither positively nor negatively.
    * `mixed`: The category was mentioned both positively and negatively in the text.



The `overall` category record is always included and contains the overall sentiment of the text.

Example:
[code] 
    {
      "categories": [
        {
          "name": "overall",
          "sentiment": "mixed"
        },
        {
          "name": "Brand",
          "sentiment": "unknown"
        },
        {
          "name": "Cost",
          "sentiment": "negative"
        },
        {
          "name": "Professionalism",
          "sentiment": "unknown"
        }
      ]
    }
    
[/code]

## Error behavior¶

By default, if AI_SENTIMENT can’t process the input, the function returns NULL. If the query processes multiple rows, rows with errors return NULL and don’t prevent the query from completing.

The return value on error depends on the `return_error_details` argument. The following table shows the return value based on the `return_error_details` argument:

> `return_error_details`| Return value| Description  
> ---|---|---  
> FALSE Not passed| NULL|   
> TRUE| OBJECT with `value` and `error` fields| `value`: An OBJECT containing the sentiment analysis result, or NULL if an error occurred. `error`: A VARCHAR value that contains the error message if an error occurred, or NULL if the function succeeded.  
  
For more information about error handling for AI functions, see [Snowflake Cortex AI Function: Multirow error handling improvements](/release-notes/bcr-bundles/2026_02/bcr-2184).

## Access control requirements¶

Users must use a role that has been granted the [SNOWFLAKE.CORTEX_USER database role](/sql-reference/snowflake-db-roles#label-snowflake-db-roles-cortex-user). See [Cortex LLM privileges](/user-guide/snowflake-cortex/aisql-privileges-and-access#label-cortex-llm-privileges) for more information on this role.

## Usage notes¶

AI_SENTIMENT can analyze sentiment in English, French, German, Hindi, Italian, Spanish, and Portuguese. You can specify categories in the language of the text or in English.

## Examples¶

The following example uses AI_SENTIMENT to get the overall sentiment of a food service review.
[code] 
    SELECT AI_SENTIMENT('A tourist\'s delight, in low urban light,
        Recommended gem, a pizza night sight. Swift arrival, a pleasure so right,
        Yet, pockets felt lighter, a slight pricey bite. 💰🍕🚀');
    
[/code]

Return value:
[code] 
    {
      "categories": [
        {
          "name": "overall",
          "sentiment": "positive"
        }
      ]
    }
    
[/code]

In this example, a table named `reviews` contains a column named `review_content` containing the text of movie reviews submitted by users. The query returns the sentiment of several facets of up to ten reviews.
[code] 
    SELECT
      AI_SENTIMENT(
        review_content,
        ['concept', 'performance', 'script', 'cinematography', 'soundtrack']
      ),
      review_content
      FROM reviews LIMIT 10;
    
[/code]

## Regional availability¶

AI_SENTIMENT is available in the following regions:

Function (Model)| AWS US West 2 (Oregon)| AWS US East 1 (N. Virginia)| AWS Europe Central 1 (Frankfurt)| AWS Europe West 1 (Ireland)| AWS AP Southeast 2 (Sydney)| AWS AP Northeast 1 (Tokyo)| Azure East US 2 (Virginia)| Azure West Europe (Netherlands)| AWS (Cross-Region)  
---|---|---|---|---|---|---|---|---|---  
AI_SENTIMENT| ✔| ✔| ✔| | | ✔| ✔| ✔| ✔  
  
Note

AI_SENTIMENT is the updated version of [ENTITY_SENTIMENT](/sql-reference/functions/entity_sentiment-snowflake-cortex). For the latest functionality, use AI_SENTIMENT.

## Legal notices¶

Refer to [Snowflake AI and ML](/guides-overview-ai-features).
