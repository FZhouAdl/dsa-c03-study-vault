---
title: "IS_AGENT_ACTIVATED (SYS_CONTEXT function) | Snowflake Documentation"
source: https://docs.snowflake.com/sql-reference/functions/is_agent_activated
cert_domain: domain-3-model-development
crawl_depth: 1
crawled: 2026-08-23
---

Categories:
    

[Context functions](/sql-reference/functions-context) (General)

# IS_AGENT_ACTIVATED (SYS_CONTEXT function)¶

Returns the VARCHAR value `'TRUE'` if an agent is active in the current execution context, or `'FALSE'` if no agent is active.

See also:
    

[SYS_CONTEXT (SNOWFLAKE$CURRENT namespace)](/sql-reference/functions/sys_context_snowflake_current)

## Syntax¶
[code] 
    SYS_CONTEXT(
      'SNOWFLAKE$CURRENT' ,
      'IS_AGENT_ACTIVATED'
    )
    
[/code]

## Arguments¶

`'SNOWFLAKE$CURRENT'`
    

Specifies that you want to call a function to return context information about the current execution context.

`'IS_AGENT_ACTIVATED'`
    

Calls the IS_AGENT_ACTIVATED function. This function takes no additional arguments and returns whether any agent is active in the current execution context.

## Returns¶

The function returns one of the following VARCHAR values:

  * `'TRUE'` if any agent is active in the current execution context.
  * `'FALSE'` if no agent is active.



The function returns `'TRUE'` in the following scenarios:

  * A Snowflake first-party agent is executing the query, including:
    * A named [Cortex Agent](/user-guide/snowflake-cortex/cortex-agents) (data agent)
    * A stateless lite agent invoked through a Snowflake CoCo client (CLI, Desktop, or web) or the [REST API](/developer-guide/snowflake-rest-api/cortex-lite-agent/cortex-lite-agent-introduction)
    * A query initiated through Snowflake CoWork
  * An external agent is executing the query, including:
    * An agent that connects to Snowflake through a managed MCP server
    * A session opened by a [SERVICE_AGENT](/user-guide/admin-user-management#label-user-management-types) user, which is automatically agent-active
    * A session using a [custom OAuth integration](/user-guide/oauth-custom#configuring-agent-sessions) configured with `IS_AGENTIC = TRUE`



For an overview of how Snowflake identifies, audits, and governs agent sessions, see [Agent identity](/user-guide/agent-identity).

To compare this return value against the BOOLEAN value TRUE or FALSE, [cast](/sql-reference/data-type-conversion#label-data-type-explicit-casting) the return value to BOOLEAN. For example:
[code] 
    SELECT SYS_CONTEXT('SNOWFLAKE$CURRENT', 'IS_AGENT_ACTIVATED')::BOOLEAN = TRUE;
    
[/code]

2026_06 behavior change bundle

When the [2026_06 behavior change bundle](/release-notes/bcr-bundles/2026_06_bundle) is [enabled in your account](/release-notes/bcr-bundles/managing-behavior-change-releases#label-manage-bcr-check-status) and you call this function through `SYS_CONTEXT`, it returns `BOOLEAN` instead of the `VARCHAR` string `'TRUE'` or `'FALSE'`.

Existing casts, such as `::BOOLEAN` or `::NUMBER`, continue to work unchanged.

For the return type of every property and function by namespace, see [SYS_CONTEXT return types](/sql-reference/functions/sys_context#label-sys-context-returns).

## Examples¶

The following example checks whether any agent is active in the current execution context:
[code] 
    SELECT SYS_CONTEXT('SNOWFLAKE$CURRENT', 'IS_AGENT_ACTIVATED');
    
[/code]

### Masking policy to restrict agent access¶

You can use IS_AGENT_ACTIVATED in a [masking policy](/sql-reference/sql/create-masking-policy) to mask sensitive data when an agent runs a query:
[code] 
    CREATE OR REPLACE MASKING POLICY mask_ssn_from_agents
      AS (val STRING) RETURNS STRING ->
      CASE
        WHEN SYS_CONTEXT('SNOWFLAKE$CURRENT', 'IS_AGENT_ACTIVATED') = 'TRUE'
          THEN '***-**-' || RIGHT(val, 4)
        ELSE val
      END;
    
    ALTER TABLE hr.employees
      MODIFY COLUMN ssn
      SET MASKING POLICY mask_ssn_from_agents;
    
[/code]

When an agent-initiated query accesses the `ssn` column, only the last four digits are visible. Direct user queries see the full value.
