---
title: "IS_GRANTED_TO_INVOKER_ROLE | Snowflake Documentation"
source: https://docs.snowflake.com/sql-reference/functions/is_granted_to_invoker_role
cert_domain: domain-3-model-development
crawl_depth: 1
crawled: 2026-08-23
---

Categories:
    

[Context functions](/sql-reference/functions-context) (Session Object)

# IS_GRANTED_TO_INVOKER_ROLE¶

Returns TRUE if the role returned by the INVOKER_ROLE function inherits the privileges of the specified role in the argument based on the context in which the function is called.

The INVOKER_ROLE function only identifies and returns the account role of the object executing a SQL statement. Database roles are not supported.

## Syntax¶
[code] 
    IS_GRANTED_TO_INVOKER_ROLE( '<string_literal>' )
    
[/code]

## Arguments¶

`'_string_literal_ '`
    

The name of the role.

## Usage notes¶

  * If using the IS_GRANTED_TO_INVOKER_ROLE function with [masking policy](/user-guide/security-column-intro) or a [row access policy](/user-guide/security-row-intro), verify that your Snowflake account is Enterprise Edition or higher.

  * Only one role name can be passed as an argument.

  * The following table summarizes the context in which you can call the function and the role hierarchy Snowflake evaluates.

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
  
  * If prefer to evaluate the role hierarchy for the current session, call [IS_ROLE_IN_SESSION](/sql-reference/functions/is_role_in_session) instead.




## Example¶

Call the function directly:

> 
[code]
>     IS_GRANTED_TO_INVOKER_ROLE('ANALYST')
>     
>     --------------------------------------+
>     IS_GRANTED_TO_INVOKER_ROLE('ANALYST') |
>     --------------------------------------+
>                     TRUE                  |
>     --------------------------------------+
>     
[/code]

Specify the function in the masking policy body:
[code] 
    CREATE OR REPLACE MASKING POLICY mask_string AS
    (val string) RETURNS string ->
    CASE
      WHEN IS_GRANTED_TO_INVOKER_ROLE('ANALYST') then val
      ELSE '*******'
    END;
    
[/code]
