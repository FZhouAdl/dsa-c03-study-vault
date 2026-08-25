---
title: "AI_FILTER | Snowflake Documentation"
source: https://docs.snowflake.com/sql-reference/functions/ai_filter
cert_domain: domain-1-data-science-concepts
crawl_depth: 1
crawled: 2026-08-23
---

Categories:
    

[String & binary functions](/sql-reference/functions-string) (AI Functions)

# AI_FILTER¶

Classifies free-form prompt inputs into a boolean. Currently supports both text and image filtering.

## Region availability¶

The following table shows the regions where you can use the AI_FILTER function for both text and images:

Data type| AWS US West 2 (Oregon)| AWS US East 1 (N. Virginia)| AWS Europe Central 1 (Frankfurt)| AWS Europe West 1 (Ireland)| AWS AP Southeast 2 (Sydney)| AWS AP Northeast 1 (Tokyo)| Azure East US 2 (Virginia)| Azure West Europe (Netherlands)| AWS (Cross-Region)  
---|---|---|---|---|---|---|---|---|---  
TEXT| ✔| ✔| ✔| ✔| ✔| ✔| ✔| ✔| ✔  
IMAGE| ✔| ✔| ✔| | | | | | ✔  
  
## Syntax¶

Applying AI_FILTER to an input string:
[code] 
    AI_FILTER( <input> [, <return_error_details> ] )
    
[/code]

Applying AI_FILTER to single image:
[code] 
    AI_FILTER( <predicate> , <input> [, <return_error_details> ] )
    
[/code]

Applying AI_FILTER to multiple columns with both text and images, leveraging the [PROMPT](/sql-reference/functions/prompt):
[code] 
    AI_FILTER( PROMPT('<template_string>',  <col_1>, … ) [, <return_error_details> ] )
    
[/code]

## Arguments¶

**Required:**

**If you’re specifying an input string:**

`_input_`
    

A string containing the text to be classified.

**If you’re filtering on one file:**

`_predicate_`
    

A string containing the instructions to classify the file input as either `TRUE` or `FALSE`.

`_file_`
    

The column that the file is classified by based on the instructions specified in `predicate`. You can use IMAGE FILE as an input to the AI_FILTER function.

**If you’re using the PROMPT() function to format the inputs:**

For more complicated prompts, especially with multiple file columns, you can use the [PROMPT](/sql-reference/functions/prompt) to help with creating an `input`.

The PROMPT() function supports formatting across both strings and FILE datatypes. For detailed usage, see Examples.

**Optional:**

`_return_error_details_`
    

A BOOLEAN flag that indicates whether to return error details in case of error. When set to TRUE, the function returns an OBJECT that contains the value and the error message, one of which is NULL depending on whether the function succeeded or failed. See Error behavior for details.

## Returns¶

Returns a Boolean value that indicates whether the statement evaluates to TRUE or FALSE for the specified text.

## Error behavior¶

By default, if AI_FILTER can’t process the input, the function returns NULL. If the query processes multiple rows, rows with errors return NULL and don’t prevent the query from completing.

The return value on error depends on the `return_error_details` argument. The following table shows the return value based on the `return_error_details` argument:

> `return_error_details`| Return value| Description  
> ---|---|---  
> FALSE Not passed| NULL|   
> TRUE| OBJECT with `value` and `error` fields| `value`: A BOOLEAN value indicating the filter result, or NULL if an error occurred. `error`: A VARCHAR value that contains the error message if an error occurred, or NULL if the function succeeded.  
  
For more information about error handling for AI functions, see [Snowflake Cortex AI Function: Multirow error handling improvements](/release-notes/bcr-bundles/2026_02/bcr-2184).

## Access control requirements¶

Users must use a role that has been granted the [SNOWFLAKE.CORTEX_USER database role](/sql-reference/snowflake-db-roles#label-snowflake-db-roles-cortex-user). See [Cortex LLM privileges](/user-guide/snowflake-cortex/aisql-privileges-and-access#label-cortex-llm-privileges) for more information on this privilege.

## Performance and cost optimization¶

By default, AI_FILTER includes a built-in performance optimization on qualifying queries. This optimization can provide 2 to 10 times faster performance and up to 60% lower token usage with a minimal impact on quality.

This optimization is triggered automatically when the query engine detects a suitable pattern. Similar to other query optimizations, Snowflake doesn’t guarantee that this optimization will be applied for every query. The engine leverages adaptive routing and context-aware rewriting to execute more efficient AI operations where possible.

> To disable this optimization for your account, contact your account manager.

## Usage notes¶

For optimal performance, follow these guidelines:

  * Make sure the columns sent into AI_FILTER don’t contain NULL values.
  * Use plain text in English for the input string or for PROMPT() arguments.
  * Provide details for the input text instruction. For example, instead of a statement like “sounds satisfied”, use “In the following support transcript, the customer sounds satisfied”.
  * Consider phrasing the input in the form of a question. For example, “In the following support transcript, does the customer sound satisfied?”



## Examples¶

### AI_FILTER: Text¶

Can be called as a simple scalar Boolean function on string constants.
[code] 
    SELECT AI_FILTER('Is Canada in North America?');
    
[/code]
[code] 
    TRUE
    
[/code]

You can [CONCAT , ||](/sql-reference/functions/concat) instructions with text columns to use this function:
[code] 
    WITH reviews AS (
                SELECT 'Wow... Loved this place.' AS review
      UNION ALL SELECT 'The pizza is not good.'
    )
    SELECT * FROM reviews
    WHERE AI_FILTER(CONCAT('The reviewer enjoyed the restaurant: ', review));
    
[/code]

For easier templated formatting across multiple columns, Snowflake provides [PROMPT](/sql-reference/functions/prompt); for example:
[code] 
    WITH reviews AS (
    
    SELECT 'Wow... Loved this place.' AS review
    UNION ALL SELECT 'The pizza is not good.'
    )
    SELECT * FROM reviews
    WHERE AI_FILTER(PROMPT('The reviewer enjoyed the restaurant: {0}', review));
    
[/code]
[code] 
    +--------------------------+
    | REVIEW                   |
    |--------------------------+
    | Wow... Loved this place. |
    +--------------------------+
    
[/code]

While evaluating the quality of AI_FILTER, it can be helpful to compare candidate predicates across columns.
[code] 
    WITH country AS (
              SELECT 'Switzerland' AS country,
    UNION ALL SELECT 'Korea'
    ),
    region AS (
                SELECT 'Asia' AS region,
      UNION ALL SELECT 'Europe'
    )
    SELECT country,
          region,
          AI_FILTER(PROMPT('{0} is in {1}', country, region)) AS result
    FROM country CROSS JOIN region ;
    
[/code]
[code] 
    +-------------+-------+--------+
    | COUNTRY     |REGION | RESULT |
    |-------------+-------+--------+
    | Switzerland |Europe | TRUE   |
    |-------------+-------+--------+
    | Switzerland | Asia  | FALSE  |
    |-------------+-------+--------+
    | Korea       |Europe | FALSE  |
    +-------------+-------+--------+
    | Korea       | Asia  | TRUE   |
    +-------------+-------+--------+
    
[/code]

### Using AI_FILTER with a JOIN¶

You can use AI_FILTER with a JOIN to express linking two tables with a natural language prompt that AI can reason on.

The following example joins the RESUMES table with the JOBS table using a prompt with the AI_FILTER function.
[code] 
    SELECT *
    FROM RESUMES
    JOIN JOBS
    ON AI_FILTER(PROMPT('Evaluate if this resume {0} fits this job description {1}', RESUME.contents, JOBS.jd));
    
[/code]

### AI_FILTER: Images¶

The following examples filter image files based on an instruction.

Filter images by providing an instruction predicate and the image file column:
[code] 
    WITH pictures AS (
      SELECT
          TO_FILE(file_url) AS img
      FROM DIRECTORY(@file_stage)
    )
    SELECT
    FL_GET_RELATIVE_PATH(img) AS file_path FROM pictures
    WHERE AI_FILTER('Is this a picture of a cat?', img);
    
[/code]
[code] 
    WITH pictures AS (
      SELECT
          TO_FILE(file_url) AS img
      FROM DIRECTORY(@file_stage)
    )
    SELECT
        FL_GET_RELATIVE_PATH(img) AS file_path FROM pictures
    WHERE AI_FILTER(PROMPT('{0} is a cat picture', img));
    
[/code]
[code] 
    +--------------------------+
    |        FILE_PATH         |
    |--------------------------+
    |        2cats.jpg         |
    +--------------------------+
    |        cat1.png          |
    +--------------------------+
    |      orange_cat.jpg      |
    +--------------------------+
    
[/code]

## Limitations¶

  * Snowflake AI functions don’t work on FILEs created from stage files from the following stage types:
    * Internal stages with encryption mode `TYPE = 'SNOWFLAKE_FULL'`

    * External stages with any customer-side encrypted mode:

      * `TYPE = 'AWS_CSE'`
      * `TYPE = 'AZURE_CSE'`
    * User stage, table stage

    * Stage with double-quoted names




## Legal notices¶

Refer to [Snowflake AI and ML](/guides-overview-ai-features) for legal notices.
