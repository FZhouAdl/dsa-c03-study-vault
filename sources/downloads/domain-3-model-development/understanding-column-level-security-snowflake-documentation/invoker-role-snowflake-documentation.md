---
title: "INVOKER_ROLE | Snowflake Documentation"
source: https://docs.snowflake.com/sql-reference/functions/invoker_role
cert_domain: domain-3-model-development
crawl_depth: 1
crawled: 2026-08-23
---

Categories:
    

[Context functions](/sql-reference/functions-context) (Session Object)

# INVOKER_ROLE¶

Returns the name of the account-level role of the object executing the query or NULL if the name of the role is a database role.

See also:
    

[Advanced Column-level Security topics](/user-guide/security-column-advanced)

## Syntax¶
[code] 
    INVOKER_ROLE()
    
[/code]

## Arguments¶

None.

## Usage notes¶

  * If using the INVOKER_ROLE function with [masking policy](/user-guide/security-column-intro), verify that your Snowflake account is Enterprise Edition or higher.

  * The following table summarizes the relationship between the query context and the role the function evaluates.

Context| Evaluated role  
---|---  
User| [CURRENT_ROLE](/sql-reference/functions/current_role)  
Table| CURRENT_ROLE.  
View| View owner role.  
UDF| UDF owner role.  
Stored procedure with caller’s right| CURRENT_ROLE.  
Stored procedure with owner’s right| Stored procedure owner role.  
Task| Task owner role.  
Stream| The role that queries a given [stream](/user-guide/streams-intro#label-stream-required-privileges).  
  
  * The following diagram shows the relationship of a query performer, roles in Snowflake, and masking policies on tables or views.

Where:

    * `R0, R1, R2, R3`
    

Are roles in Snowflake.

    * `P1, P2, P3`
    

Are masking policies in Snowflake.

    * `V1, V2`
    

Are views in Snowflake.

    * `T`
    

Is a table in Snowflake.

Based on this diagram, the values of CURRENT_ROLE and INVOKER_ROLE in a query are as follows:

Policy| CURRENT_ROLE| INVOKER_ROLE  
---|---|---  
P1| R3| R1  
P2| R3| R2  
P3| R3| R3  
  



## Examples¶

The following examples show how to use the INVOKER_ROLE in a masking policy SQL expression.

Return NULL for unauthorized users:

> 
[code]
>     CREATE OR REPLACE MASKING POLICY mask_string AS
>     (val string) RETURNS string ->
>     CASE
>       WHEN INVOKER_ROLE() IN ('ANALYST') THEN val
>       ELSE NULL
>     END;
>     
[/code]

Return a static masked value for unauthorized users:

> 
[code]
>     CREATE OR REPLACE MASKING POLICY mask_string AS
>     (val string) RETURNS string ->
>     CASE
>       WHEN INVOKER_ROLE() IN ('ANALYST') THEN val
>       ELSE '********'
>     END;
>     
[/code]

Return a hash value using SHA2 , SHA2_HEX for unauthorized users:

> 
[code]
>     CREATE OR REPLACE MASKING POLICY mask_string AS
>     (val string) RETURNS string ->
>     CASE
>       WHEN INVOKER_ROLE() IN ('ANALYST') THEN val
>       ELSE SHA2(val)
>     END;
>     
[/code]
