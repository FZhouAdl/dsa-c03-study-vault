---
title: "SENTIMENT (SNOWFLAKE.CORTEX) | Snowflake Documentation"
source: https://docs.snowflake.com/sql-reference/functions/sentiment-snowflake-cortex
cert_domain: domain-2-data-preparation-and-feature-engineering
crawl_depth: 1
crawled: 2026-08-23
---

Categories:
    

[String & binary functions](/sql-reference/functions-string) (AI Functions)

# SENTIMENT (SNOWFLAKE.CORTEX)¶

Returns an overall sentiment score for the given English-language input text.

## Syntax¶
[code] 
    SNOWFLAKE.CORTEX.SENTIMENT(<text>)
    
[/code]

## Arguments¶

`_text_`
    

A string containing the text for which a sentiment score should be calculated.

## Returns¶

A floating-point number from -1 to 1 (inclusive) indicating the model’s level of certainty of any detected sentiment. A score close to 0 indicates that the function could not determine a clear sentiment in the text; this result can be considered neutral. A score close to 1 indicates positive sentiment, while a score close to -1 indicates negative sentiment. The chart below provides guidance on how to interpret the sentiment scores:

Sentiment| Sentiment Score  
---|---  
Positive| 0.5 to 1  
Neutral| -0.5 to 0.5  
Negative| -0.5 to -1  
  
The result _does not_ indicate the intensity of sentiment, but the polarity (positive, neutral, or negative) and certainty.

## Access control requirements¶

Users must use a role that has been granted the [SNOWFLAKE.CORTEX_USER database role](/sql-reference/snowflake-db-roles#label-snowflake-db-roles-cortex-user). See [Cortex LLM privileges](/user-guide/snowflake-cortex/aisql-privileges-and-access#label-cortex-llm-privileges) for more information on this privilege.

## Examples¶

The following example uses SENTIMENT to get the sentiment classification of a food service review, which we can infer as modestly positive, given the score of 0.54.
[code] 
    SELECT SNOWFLAKE.CORTEX.SENTIMENT('A tourist\'s delight, in low urban light,
      Recommended gem, a pizza night sight. Swift arrival, a pleasure so right,
      Yet, pockets felt lighter, a slight pricey bite. 💰🍕🚀');
    
[/code]

Response:
[code] 
    0.5424458
    
[/code]

In the following example, a table named `reviews` contains a column named `review_content` containing the text of reviews submitted by users. The query returns a sentiment score for each review.
[code] 
    SELECT SNOWFLAKE.CORTEX.SENTIMENT(review_content), review_content FROM reviews LIMIT 10;
    
[/code]

## Legal notices¶

Refer to [Snowflake AI and ML](/guides-overview-ai-features).
