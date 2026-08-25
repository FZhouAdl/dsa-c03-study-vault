---
title: "AI_COUNT_TOKENS | Snowflake Documentation"
source: https://docs.snowflake.com/sql-reference/functions/ai_count_tokens
cert_domain: domain-1-data-science-concepts
crawl_depth: 1
crawled: 2026-08-23
---

Categories:
    

[String & binary functions](/sql-reference/functions-string) (AI Functions)

# AI_COUNT_TOKENS¶

Returns an estimate of the number of **input** tokens in a prompt for the specified large language model or task-specific function. For functions that can take additional inputs that affect the input token count, such as model name or categories/labels, those inputs can also be specified.

## Syntax¶

The syntax can vary based on the function used. In general, you pass the function name, model name if applicable, input text, and any additional options that affect token count.
[code] 
    AI_COUNT_TOKENS( <function_name>, <input_text> [, <return_error_details> ] )
    AI_COUNT_TOKENS( <function_name>, <model_name>, <input_text> [, <return_error_details> ] )
    AI_COUNT_TOKENS( <function_name>, <input_text>, <options> [, <return_error_details> ] )
    AI_COUNT_TOKENS( <function_name>, <model_name>, <input_text>, <options> [, <return_error_details> ] )
    
[/code]

AI_COUNT_TOKENS uses specific syntax variations for some functions. For example:
[code] 
    AI_COUNT_TOKENS( 'ai_similarity', <input_text_1>, <input_text_2>, <options> [, <return_error_details> ] )
    AI_COUNT_TOKENS( 'ai_classify', <input_text>, <categories> [, <return_error_details> ] )
    AI_COUNT_TOKENS( 'ai_translate', <input_text>, <source_language>, <target_language> [, <return_error_details> ] )
    
[/code]

See Examples for function specific usage patterns.

## Arguments¶

**Required:**

`_function_name_`
    

String containing the name of the function you want to base the token count on, such as `'ai_complete'` or `'ai_sentiment'`. The function’s name must begin with “ai_” and use only lowercase letters.

A complete list of supported functions is available in the [Regional availability](/user-guide/snowflake-cortex/aisql-regional-availability#label-cortex-llm-availability) table.

`_input_text_` or `_input_text_1_`, `_input_text_2_`
    

Input text to count the tokens in.

**Optional:**

`_model_name_`
    

String containing the name of the model you want to base the token content on. Required if the function specified by `_function_name_` requires you to choose the model to use, such as AI_COMPLETE or AI_EMBED.

A list of available LLM models is available in the [Regional availability](/user-guide/snowflake-cortex/aisql-regional-availability#label-cortex-llm-availability) table. Snowflake intends to add support for additional models over time.

`_categories_`
    

An array of VARIANT values that specify one or more categories or labels to use, for functions that require this data. Categories are included in the input token count.

`_options_`
    

A VARIANT that specifies additional options that affect how the function processes the input. For functions that take two text inputs, such as AI_SIMILARITY, options are used to specify the model.

`_return_error_details_`
    

A BOOLEAN flag that indicates whether to return error details in case of error. When set to TRUE, the function returns an OBJECT that contains the value and the error message, one of which is NULL depending on whether the function succeeded or failed. See Error behavior for details.

## Returns¶

An [INTEGER](/sql-reference/data-types-numeric#label-data-type-integer) value that is the estimated number of _input_ tokens for the input text and other parameter values you provide. This value does not include output (generated) tokens.

## Error behavior¶

By default, if AI_COUNT_TOKENS can’t process the input, the function returns NULL. If the query processes multiple rows, rows with errors return NULL and don’t prevent the query from completing.

The return value on error depends on the `return_error_details` argument. The following table shows the return value based on the `return_error_details` argument:

> `return_error_details`| Return value| Description  
> ---|---|---  
> FALSE Not passed| NULL|   
> TRUE| OBJECT with `value` and `error` fields| `value`: An INTEGER value that is the token count, or NULL if an error occurred. `error`: A VARCHAR value that contains the error message if an error occurred, or NULL if the function succeeded.  
  
For more information about error handling for AI functions, see [Snowflake Cortex AI Function: Multirow error handling improvements](/release-notes/bcr-bundles/2026_02/bcr-2184).

## Usage notes¶

  * Although function names are usually written in all uppercase, use only lowercase letters in function and model names.
  * AI_COUNT_TOKENS does not work with LLM functions in the SNOWFLAKE.CORTEX namespace or with fine-tuned models. You must specify a function name that begins with “ai_”.
  * AI_COUNT_TOKENS accepts only text, not image, audio, or video inputs.
  * AI_COUNT_TOKENS estimates input tokens only. It does not estimate output (generated) tokens, which also contribute to billing. To see actual billed input and output tokens for a query, use the [CORTEX_FUNCTIONS_QUERY_USAGE_HISTORY](/sql-reference/account-usage/cortex_functions_query_usage_history) view.
  * The accuracy of the token count depends on the model. See Token count accuracy for details.
  * When you use [AI_COMPLETE](/sql-reference/functions/ai_complete) with the `response_format` argument (structured output) on Anthropic Claude models, additional request content is generated to support structured output. This content is included in the billed input tokens but is not reflected in the AI_COUNT_TOKENS estimate. As a result, the billed input token count for these requests can be materially higher than the estimated token count.
  * AI_COUNT_TOKENS only incurs compute costs and does not bill based on token count.
  * AI_COUNT_TOKENS is available in all regions, even for models not available in a given region.



### Token count accuracy¶

The accuracy of the token count depends on the model. For most models, AI_COUNT_TOKENS returns an exact count. The following table shows the expected accuracy by model:

Model| Token count accuracy  
---|---  
Anthropic Claude| Estimate; relative error under 3%  
Google Gemini| Estimate; relative error under 3%  
OpenAI| Near-exact  
All other models| Exact  
  
Note

If you use structured outputs (the `response_format` argument, supported on Anthropic Claude models), the difference between the estimate and the billed input token count can be a lot larger than the values shown here. For details, see the structured output note in Usage notes.

## Examples¶

### AI_COMPLETE example¶

The following SQL statement counts the number of tokens in a prompt for AI_COMPLETE and the `llama3.3-70b` model:
[code] 
    SELECT AI_COUNT_TOKENS('ai_complete', 'llama3.3-70b', 'Summarize the insights from this
    call transcript in 20 words: "I finally splurged on these after months of hesitation about
    the price, and I\'m mostly impressed. The Nulu fabric really is as buttery-soft as everyone says,
    and they\'re incredibly comfortable for yoga and lounging. The high-rise waistband stays put
    and doesn\'t dig in, which is rare for me. However, I\'m already seeing some pilling after
    just a few wears, and they definitely require gentle care. They\'re also quite delicate -
    I snagged them slightly on my gym bag zipper. Great for low-impact activities, but I wouldn\'t
    recommend for high-intensity workouts. Worth it for the comfort factor"');
    
[/code]

Response:
[code] 
    158
    
[/code]

### AI_COMPLETE with structured output example¶

The following SQL statement estimates the input tokens for an AI_COMPLETE call that uses the `response_format` argument (structured output) on an Anthropic Claude model:
[code] 
    SELECT AI_COUNT_TOKENS(
      'ai_complete',
      'claude-sonnet-4-5',
      'Extract structured data from this customer interaction note: Customer Sarah Jones
      complained about the mobile app crashing during checkout. She tried to purchase 3 items:
      a red XL jacket (EUR 89.99), blue running shoes (EUR 129.50), and a fitness tracker
      (EUR 199.00). The app crashed after she entered her shipping address at 123 Main St,
      Portland OR, 97201. She has been a premium member since January 2024.',
      {
        'type': 'json',
        'schema': {
          'type': 'object',
          'properties': {
            'items_count': {'type': 'number'},
            'prices': {'type': 'array', 'items': {'type': 'string'}},
            'address': {'type': 'string'},
            'member_date': {'type': 'string'}
          },
          'required': ['items_count', 'prices', 'address', 'member_date']
        }
      }
    );
    
[/code]

Response:
[code] 
    296
    
[/code]

Note

Structured output on Claude models generates additional request content that is billed as input tokens but is not reflected in this estimate, so the billed input token count can be materially higher than the estimated token count. See Usage notes for details.

### AI_EMBED example¶

The following SQL statement counts the number of tokens in text being embedded using the AI_EMBED function and the `nv-embed-qa-4` model:
[code] 
    SELECT AI_COUNT_TOKENS('ai_embed', 'nv-embed-qa-4', '"I finally splurged on these after months
    of hesitation about the price, and I\'m mostly impressed. The Nulu fabric really is as buttery-soft
    as everyone says, and they\'re incredibly comfortable for yoga and lounging. The high-rise waistband
    stays put and doesn\'t dig in, which is rare for me. However, I\'m already seeing some pilling after
    just a few wears, and they definitely require gentle care. They\'re also quite delicate - I snagged
    them slightly on my gym bag zipper. Great for low-impact activities, but I wouldn\'t recommend for
    high-intensity workouts. Worth it for the comfort factor"');
    
[/code]

Response:
[code] 
    142
    
[/code]

### AI_CLASSIFY examples¶

This example calculates the total number of input tokens required for text classification with given input and labels:
[code] 
    SELECT AI_COUNT_TOKENS('ai_classify',
      'One day I will see the world and learn to cook my favorite dishes',
      [
          {'label': 'travel'},
          {'label': 'cooking'},
          {'label': 'reading'},
          {'label': 'driving'}
      ]
    );
    
[/code]

Response:
[code] 
    187
    
[/code]

The following example adds per-label descriptions and an overall task description to the previous example:
[code] 
    SELECT AI_COUNT_TOKENS('ai_classify',
      'One day I will see the world and learn to cook my favorite dishes',
      [
        {'label': 'travel', 'description': 'content related to traveling'},
        {'label': 'cooking','description': 'content related to food preparation'},
        {'label': 'reading','description': 'content related to reading'},
        {'label': 'driving','description': 'content related to driving a car'}
      ],
      {
        'task_description': 'Determine topics related to the given text'
      }
    );
    
[/code]

Response:
[code] 
    254
    
[/code]

The following example builds upon the previous two examples by adding label examples:
[code] 
    SELECT AI_COUNT_TOKENS('ai_classify',
      'One day I will see the world and learn to cook my favorite dishes',
      [
        {'label': 'travel', 'description': 'content related to traveling'},
        {'label': 'cooking','description': 'content related to food preparation'},
        {'label': 'reading','description': 'content related to reading'},
        {'label': 'driving','description': 'content related to driving a car'}
      ],
      {
        'task_description': 'Determine topics related to the given text',
        'examples': [
          {
            'input': 'i love traveling with a good book',
            'labels': ['travel', 'reading'],
            'explanation': 'the text mentions traveling and a good book which relates to reading'
          }
        ]
      }
    );
    
[/code]

Response:
[code] 
    298
    
[/code]

### AI_SENTIMENT examples¶

The following SQL statement counts the number of tokens in text being analyzed for sentiment using the AI_SENTIMENT function:
[code] 
    SELECT AI_COUNT_TOKENS('ai_sentiment',
      'This place makes the best truffle pizza in the world! Too bad I cannot afford it');
    
[/code]

Response:
[code] 
    139
    
[/code]

The following example adds labels to the previous example:
[code] 
    SELECT AI_COUNT_TOKENS('ai_sentiment',
      'This place makes the best truffle pizza in the world! Too bad I cannot afford it',
      [
        {'label': 'positive'},
        {'label': 'negative'},
        {'label': 'neutral'}
      ]
    );
    
[/code]

Response:
[code] 
    148
    
[/code]

### AI_SIMILARITY examples¶

The following SQL statement counts the number of tokens in an AI_SIMILARITY call that uses the default model.
[code] 
    SELECT AI_COUNT_TOKENS('ai_similarity',
      'The plot is fast and the characters feel real. This book kept me awake all night
      because the mystery is so deep. I love how the author handles the ending. It is a
      great read for anyone who likes suspense.',
      'The story is quick and the people feel true. This novel kept me awake all night
      because the puzzle is so big. I love how the writer handles the finale. It is a
      solid choice for anyone who enjoys suspense.');
    
[/code]

Response:
[code] 
    101
    
[/code]

The following SQL statement counts the number of tokens in an AI_SIMILARITY that uses the `e5-base-v2` model:
[code] 
    SELECT AI_COUNT_TOKENS('ai_similarity',
      'The plot is fast and the characters feel real. This book kept me awake all night
      because the mystery is so deep. I love how the author handles the ending. It is a
      great read for anyone who likes suspense.',
      'The story is quick and the people feel true. This novel kept me awake all night
      because the puzzle is so big. I love how the writer handles the finale. It is a
      solid choice for anyone who enjoys suspense.', {'model': 'e5-base-v2'});
    
[/code]

Response:
[code] 
    92
    
[/code]

### AI_TRANSLATE example¶

The following SQL statement counts the number of tokens used by AI_TRANSLATE when translating text from English to German.
[code] 
    SELECT AI_COUNT_TOKENS('ai_translate',
      'The plot is fast and the characters feel real. This book kept me awake all night
      because the mystery is so deep. I love how the author handles the ending. It is a
      great read for anyone who likes suspense.', 'en', 'de');
    
[/code]

Response:
[code] 
    51
    
[/code]

### AI_REDACT examples¶

The following SQL statement counts the number of input tokens for a basic AI_REDACT request:
[code] 
    SELECT AI_COUNT_TOKENS('ai_redact',
      'My name is John Smith and I live at twenty third street, San Francisco.');
    
[/code]

Response:
[code] 
    442
    
[/code]

The following example includes a `categories` argument to estimate tokens when redacting only names and email addresses:
[code] 
    SELECT AI_COUNT_TOKENS('ai_redact',
      'My name is John and I live at twenty third street, San Francisco.',
      ['NAME', 'EMAIL']);
    
[/code]

Response:
[code] 
    441
    
[/code]

Note

AI_COUNT_TOKENS is the updated version of [COUNT_TOKENS](/sql-reference/functions/count_tokens-snowflake-cortex). For the latest functionality, use AI_COUNT_TOKENS.

## Legal notices¶

Refer to [Snowflake AI and ML](/guides-overview-ai-features).
