---
title: "AI_SUMMARIZE_AGG | Snowflake Documentation"
source: https://docs.snowflake.com/sql-reference/functions/ai_summarize_agg
cert_domain: domain-1-data-science-concepts
crawl_depth: 1
crawled: 2026-08-23
---

Categories:
    

[Aggregate functions](/sql-reference/functions-aggregation) (General) [String & binary functions](/sql-reference/functions-string) (AI Functions)

# AI_SUMMARIZE_AGG¶

Summarizes a column of text data.

For example, `AI_SUMMARIZE_AGG(churn_reason)` will return a summary of the `churn_reason` column.

Unlike [AI_COMPLETE](/sql-reference/functions/ai_complete) and [SUMMARIZE (SNOWFLAKE.CORTEX)](/sql-reference/functions/summarize-snowflake-cortex), this function supports datasets larger than the maximum language model context window.

See also:
    

[AI_AGG](/sql-reference/functions/ai_agg)

## Syntax¶
[code] 
    AI_SUMMARIZE_AGG( <expr> )
    
[/code]

## Arguments¶

**Required:**

`_expr_`
    

This is an expression that contains text for summarization, such as restaurant reviews or phone transcripts.

## Returns¶

Returns a string summary of the expression.

## Usage notes¶

This function provides a general purpose summary. For a more specific summary, use [AI_AGG](/sql-reference/functions/ai_agg).

## Examples¶

AI_SUMMARIZE_AGG can be used as a simple scalar function on string constants.
[code] 
    SELECT AI_SUMMARIZE_AGG('The restaurant was excellent. I especially enjoyed the pizza and ice cream. My grandma didnt like it though.');
    
[/code]
[code] 
    The restaurant received mixed reviews from our group. While I thoroughly enjoyed the pizza and ice cream, my grandma did not have a positive experience.
    
[/code]

AI_SUMMARIZE_AGG can be used on a column of data.
[code] 
    WITH reviews AS (
                SELECT 'The restaurant was excellent.' AS review
      UNION ALL SELECT 'Excellent! I loved the pizza!'
      UNION ALL SELECT 'It was great, but the service was meh.'
      UNION ALL SELECT 'Mediocre food and mediocre service'
    )
    SELECT AI_SUMMARIZE_AGG(review)
      FROM reviews;
    
[/code]
[code] 
    The restaurant received mixed reviews. Some customers had a great experience, enjoying the pizza and finding the restaurant excellent. However, others had a more neutral experience, describing the food and service as mediocre, with one customer specifically mentioning that the service was subpar.
    
[/code]

AI_SUMMARIZE_AGG can be used on multiple columns of data using `CONCAT` or the `||` operator.
[code] 
    WITH reviews AS (
                SELECT 'The restaurant was excellent.' AS review, 'Pizza' AS menu_item
      UNION ALL SELECT 'Excellent! I loved the pizza!', 'Pizza'
      UNION ALL SELECT 'It was great, but the service was meh.', 'Burger'
      UNION ALL SELECT 'Mediocre food and mediocre service', 'Pancakes'
    )
    SELECT AI_SUMMARIZE_AGG('Menu Item: ' || menu_item || '\nReview: ' || review)
      FROM reviews;
    
[/code]
[code] 
    The restaurant received positive reviews for its pizza, with one reviewer describing it as "excellent" and another stating they "loved" it. In contrast, the burger received a mixed review, with the food being "great" but the service being "meh." The pancakes were rated as "mediocre" in terms of both food and service. Overall, the restaurant's performance varied depending on the menu item, with pizza being a highlight.
    
[/code]

AI_SUMMARIZE_AGG can also be used in combination with GROUP BY.
[code] 
    WITH reviews AS (
                SELECT 1 AS product_id, 'The restaurant was excellent.' AS review
      UNION ALL SELECT 1, 'Excellent! I loved the pizza!'
      UNION ALL SELECT 1, 'It was great, but the service was meh.'
      UNION ALL SELECT 1, 'Mediocre food and mediocre service'
      UNION ALL SELECT 2, 'Terrible quality ingredients, I should have eaten at home.'
      UNION ALL SELECT 2, 'Bad restaurant, I would avoid this place.'
    )
    SELECT product_id,
           AI_SUMMARIZE_AGG(review) AS summarized_review
      FROM reviews
     GROUP BY 1;
    
[/code]
[code] 
    +------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
    | PRODUCT_ID | SUMMARIZED_REVIEW                                                                                                                                                                                                                                                                                         |
    |------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
    | 1          | The restaurant received mixed reviews. Some customers had a great experience, enjoying the pizza and finding the restaurant excellent. However, others had a more neutral experience, describing the food and service as mediocre, with one customer specifically mentioning that the service was subpar. |
    +------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
    | 2          | The reviewer had a poor experience at the restaurant, citing the use of low-quality ingredients and expressing regret over not eating at home instead. They strongly advise against visiting this establishment.                                                                                          |
    +------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
    
[/code]

See also [AI_AGG](/sql-reference/functions/ai_agg).

## Legal notices¶

Refer to [Snowflake AI and ML](/guides-overview-ai-features) for legal notices.
