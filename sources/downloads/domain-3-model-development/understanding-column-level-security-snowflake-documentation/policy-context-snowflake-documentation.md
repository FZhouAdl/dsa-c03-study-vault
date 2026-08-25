---
title: "POLICY_CONTEXT | Snowflake Documentation"
source: https://docs.snowflake.com/sql-reference/functions/policy_context
cert_domain: domain-3-model-development
crawl_depth: 1
crawled: 2026-08-23
---

Categories:
    

[Context functions](/sql-reference/functions-context)

# POLICY_CONTEXT¶

Simulates the results of a query based on the value of one or more context functions or [SYS_CONTEXT](/sql-reference/functions/sys_context) namespace properties, which lets you determine how policies affect query results. Context functions and SYS_CONTEXT properties return a value based on the current context of a query: for example, who is executing the query, which roles are activated, or whether an agent is invoking the query. Policy bodies often use these values to determine which value to return from the policy.

This function evaluates the following policies to determine the query results:

  * [Masking policies](/user-guide/security-column-intro)
  * [Row access policies](/user-guide/security-row-intro)
  * [Aggregation policies](/user-guide/aggregation-policies)
  * [Join policies](/user-guide/join-policies)
  * [Projection policies](/user-guide/projection-policies)



## Syntax¶
[code] 
    EXECUTE USING
    POLICY_CONTEXT(
      <arg> => '<string_literal>'
      [ , <arg> => '<string_literal>' , ... ]
      [ , <arg> => ( '<string_literal>' [ , '<string_literal>' , ... ] ) ]
    )
    AS
    SELECT <query>
    
[/code]

## Arguments¶

You can specify context function arguments, SYS_CONTEXT property arguments, or both. You must specify at least one argument.

The following table summarizes all supported arguments:

Argument| Type| Description  
---|---|---  
`CURRENT_USER`| Context function| Current user executing the query  
`CURRENT_ROLE`| Context function| Current role in use  
`CURRENT_AVAILABLE_ROLES`| Context function| Available roles for the current user  
`CURRENT_ACCOUNT`| Context function| Current account  
`SNOWFLAKE$SESSION_ROLE`| SYS_CONTEXT property| Primary role for the session  
`SNOWFLAKE$SESSION_PRINCIPAL_NAME`| SYS_CONTEXT property| Name of the principal that started the session  
`SNOWFLAKE$SESSION_PRINCIPAL_TYPE`| SYS_CONTEXT property| Type of the principal that started the session  
`SNOWFLAKE$SESSION_DATABASE`| SYS_CONTEXT property| Current database in use for the session  
`SNOWFLAKE$SESSION_SCHEMA`| SYS_CONTEXT property| Current schema in use for the session  
`SNOWFLAKE$SESSION_WAREHOUSE`| SYS_CONTEXT property| Current warehouse in use for the session  
`SNOWFLAKE$SESSION_ACTIVATED_ROLES`| SYS_CONTEXT list| Set of activated account roles in the session  
`SNOWFLAKE$SESSION_ACTIVATED_DATABASE_ROLES`| SYS_CONTEXT list| Set of activated database roles in the session  
`SNOWFLAKE$CURRENT_ACTIVATED_ROLES`| SYS_CONTEXT list| Set of activated account roles in the current execution context  
`SNOWFLAKE$CURRENT_ACTIVATED_DATABASE_ROLES`| SYS_CONTEXT list| Set of activated database roles in the current execution context  
  
### Context function arguments¶

`_context_function_ => '_string_literal_ '`
    

Specifies a context function and its value as a string.

Snowflake supports the following context functions and their values as arguments:

  * [CURRENT_USER](/sql-reference/functions/current_user)
  * [CURRENT_ROLE](/sql-reference/functions/current_role)
  * [CURRENT_AVAILABLE_ROLES](/sql-reference/functions/current_available_roles)
  * [CURRENT_ACCOUNT](/sql-reference/functions/current_account)



To determine the format to use as a string value, execute a query using the function. For example:

> 
[code]
>     SELECT CURRENT_USER();
>     
>     +----------------+
>     | CURRENT_USER() |
>     |----------------|
>     | JSMITH         |
>     +----------------+
>     
[/code]

The string value should be `'JSMITH'`.

Note that if specifying CURRENT_AVAILABLE_ROLES and multiple role values, such as `ROLE1` and `ROLE2`, enclose the list of roles in square brackets as follows:

> `['ROLE1', 'ROLE2']`

### SYS_CONTEXT property arguments¶

`_sys_context_key_ => '_string_literal_ '`
    

Specifies a [SYS_CONTEXT](/sql-reference/functions/sys_context) namespace property and its simulated value as a string.

The argument name combines the namespace and property, separated by an underscore. For example, `SNOWFLAKE$SESSION_ROLE` corresponds to the `ROLE` property in the [SNOWFLAKE$SESSION namespace](/sql-reference/functions/sys_context_snowflake_session).

Snowflake supports the following SYS_CONTEXT property arguments:

Argument| Description  
---|---  
`SNOWFLAKE$SESSION_ROLE`| Simulates the value of the `ROLE` property in the [SNOWFLAKE$SESSION namespace](/sql-reference/functions/sys_context_snowflake_session).  
`SNOWFLAKE$SESSION_PRINCIPAL_NAME`| Simulates the value of the `PRINCIPAL_NAME` property in the [SNOWFLAKE$SESSION namespace](/sql-reference/functions/sys_context_snowflake_session).  
`SNOWFLAKE$SESSION_PRINCIPAL_TYPE`| Simulates the value of the `PRINCIPAL_TYPE` property in the [SNOWFLAKE$SESSION namespace](/sql-reference/functions/sys_context_snowflake_session).  
`SNOWFLAKE$SESSION_DATABASE`| Simulates the value of the `DATABASE` property in the [SNOWFLAKE$SESSION namespace](/sql-reference/functions/sys_context_snowflake_session).  
`SNOWFLAKE$SESSION_SCHEMA`| Simulates the value of the `SCHEMA` property in the [SNOWFLAKE$SESSION namespace](/sql-reference/functions/sys_context_snowflake_session).  
`SNOWFLAKE$SESSION_WAREHOUSE`| Simulates the value of the `WAREHOUSE` property in the [SNOWFLAKE$SESSION namespace](/sql-reference/functions/sys_context_snowflake_session).  
  
### SYS_CONTEXT list arguments¶

`_sys_context_key_ => ( '_string_literal_ ' [ , '_string_literal_ ' , ... ] )`
    

Specifies a [SYS_CONTEXT](/sql-reference/functions/sys_context) namespace property and a list of simulated values. Enclose the values in parentheses to form a tuple. You can also specify a single value without parentheses.

Snowflake supports the following SYS_CONTEXT list arguments:

Argument| Description  
---|---  
`SNOWFLAKE$SESSION_ACTIVATED_ROLES`| Simulates the set of activated account roles in the [SNOWFLAKE$SESSION namespace](/sql-reference/functions/sys_context_snowflake_session).  
`SNOWFLAKE$SESSION_ACTIVATED_DATABASE_ROLES`| Simulates the set of activated database roles in the [SNOWFLAKE$SESSION namespace](/sql-reference/functions/sys_context_snowflake_session).  
`SNOWFLAKE$CURRENT_ACTIVATED_ROLES`| Simulates the set of activated account roles in the [SNOWFLAKE$CURRENT namespace](/sql-reference/functions/sys_context_snowflake_current) (the current execution context).  
`SNOWFLAKE$CURRENT_ACTIVATED_DATABASE_ROLES`| Simulates the set of activated database roles in the [SNOWFLAKE$CURRENT namespace](/sql-reference/functions/sys_context_snowflake_current) (the current execution context).  
  
`_query_`
    

Specifies the SQL expression to query one or more tables or views.

Required.

## Usage notes¶

  * This function requires the following:

    * At least one argument that specifies a supported context function or SYS_CONTEXT property and its value.
    * If a table is protected by a policy, the specified user or role must be granted the following privileges:
      * OWNERSHIP on the table or view, and
      * The APPLY privilege for the policy, either at the account level or on the policy itself:
        * APPLY MASKING POLICY on ACCOUNT or APPLY on MASKING POLICY `_policy_name_`
        * APPLY ROW ACCESS POLICY on ACCOUNT or APPLY on ROW ACCESS POLICY `_policy_name_`
        * APPLY AGGREGATION POLICY on ACCOUNT or APPLY on AGGREGATION POLICY `_policy_name_`
        * APPLY JOIN POLICY on ACCOUNT or APPLY on JOIN POLICY `_policy_name_`
        * APPLY PROJECTION POLICY on ACCOUNT or APPLY on PROJECTION POLICY `_policy_name_`
  * Snowflake returns an error message if any of the following conditions are true:

    * Using one or more unsupported arguments. Snowflake only supports the arguments listed in the Arguments section.
    * Not specifying a value properly, including using a string for a value that does not exist (for example, no account, user, or role).
    * The SELECT `_query_` expression does not query a table or view properly (for example, not specifying a table or view at all).
    * Certain data sharing use cases (see the next bullet).
  * Data sharing:

    * A data sharing consumer cannot use this function to simulate query results on tables or views that were made available by the data sharing provider.

Additionally, if the consumer `_query_` expression includes a table or view made available through [Secure Data Sharing](/user-guide/data-sharing-intro) and another table or view in the consumer account not associated with the data sharing provider account (that is, their own table or view), Snowflake returns an error message.

    * A data sharing provider account can simulate how a data sharing consumer account views tables or views made available through a share.

To do this, the data sharing provider specifies the consumer account name as the argument. For example:
[code] execute using policy_context(current_account => '<consumer_account_name>') ... ;
          
[/code]

  * The result depends on the following:

    * The masking policy or projection policy that is set on a column, if any.
    * The row access policy, aggregation policy, or join policy that is set on the table or view, if any.
    * The policy definitions.
    * The `_query_` expression.
    * The privileges granted to roles.
    * The roles granted to users (including role hierarchy).
    * The arguments in this function.

Important

If the result from this function is not what you expected:

    * Consult with your internal policy administrator to determine which tables, views, and columns are protected by policies, and to better understand the body definitions of those policies. This administrator might have a custom role like `POLICY_ADMIN`, `MASKING_ADMIN`, or `RAP_ADMIN`.
    * Double-check the:
      * Function string values.
      * `SELECT` `_query_` expression.
      * Privileges [granted to roles](/sql-reference/sql/grant-privilege) (for example, SELECT on table or view, USAGE or any other privilege on parent database and schema) and the corresponding [privilege inheritance](/user-guide/security-access-control-overview#label-role-hierarchy-and-privilege-inheritance).
      * [Role hierarchy](/user-guide/security-access-control-configure#label-security-role-hierarchy), especially if specifying the CURRENT_AVAILABLE_ROLES function and its values as an argument for this function.

Update the SQL statement using this function, as needed, and try again.

  * SYS_CONTEXT list arguments accept either a single string value or a parenthesized tuple of string values. For example, both of the following are valid:
[code] -- Single value (no parentheses needed):
        SNOWFLAKE$SESSION_ACTIVATED_ROLES => 'ANALYST'
        
        -- Multiple values (parenthesized tuple):
        SNOWFLAKE$SESSION_ACTIVATED_ROLES => ('ANALYST', 'PUBLIC')
        
[/code]

  * You can combine context function arguments and SYS_CONTEXT arguments in the same POLICY_CONTEXT call.

  * For more information about the SNOWFLAKE$CURRENT namespace properties that some of these arguments simulate, see [SYS_CONTEXT (SNOWFLAKE$CURRENT namespace)](/sql-reference/functions/sys_context_snowflake_current).




## Examples¶

Simulate the effect of the PUBLIC system role querying the table `empl_info`:

> 
[code]
>     EXECUTE USING POLICY_CONTEXT(CURRENT_ROLE => 'PUBLIC')
>       AS SELECT * FROM empl_info;
>     
[/code]

Simulate a specific set of activated roles in the session:

> 
[code]
>     EXECUTE USING POLICY_CONTEXT(
>       SNOWFLAKE$SESSION_ACTIVATED_ROLES => ('ANALYST', 'PUBLIC')
>     )
>       AS SELECT * FROM empl_info;
>     
[/code]

Combine context function arguments with SYS_CONTEXT arguments:

> 
[code]
>     EXECUTE USING POLICY_CONTEXT(
>       CURRENT_ROLE => 'ANALYST',
>       SNOWFLAKE$SESSION_ACTIVATED_ROLES => ('ANALYST', 'PUBLIC'),
>       SNOWFLAKE$CURRENT_ACTIVATED_ROLES => ('ANALYST')
>     )
>       AS SELECT * FROM empl_info;
>     
[/code]
