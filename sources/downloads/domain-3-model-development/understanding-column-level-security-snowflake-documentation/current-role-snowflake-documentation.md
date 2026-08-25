---
title: "CURRENT_ROLE | Snowflake Documentation"
source: https://docs.snowflake.com/sql-reference/functions/current_role
cert_domain: domain-3-model-development
crawl_depth: 1
crawled: 2026-08-23
---

Categories:
    

[Context functions](/sql-reference/functions-context) (Session Object)

# CURRENT_ROLE¶

Returns the name of the [primary role](/user-guide/security-access-control-overview#label-access-control-role-enforcement) in use for the current session when the primary role is an account-level role or NULL if the role in use for the current session is a database role.

To specify a different role for the session, execute the [USE ROLE](/sql-reference/sql/use-role) command.

## Syntax¶
[code] 
    CURRENT_ROLE()
    
[/code]

## Arguments¶

None.

## Usage notes¶

  * Granting access on a [secure UDF](/developer-guide/secure-udf-procedure#label-secure-udf-data-sharing) or [secure view](/user-guide/views-secure#label-secure-view-data-sharing) that contains this function to a share is allowed. When the secure UDF or secure view is accessed from the data sharing consumer account, this function always returns a NULL value.
  * Snowflake returns a NULL value if this function is used in a [masking policy](/user-guide/security-column-intro#label-security-column-intro-data-sharing) or [row access policy](/user-guide/security-row-intro#label-security-row-intro-data-sharing) that is assigned to a shared table or view.



## Examples¶

This demonstrates `CURRENT_ROLE()`:
[code] 
    SELECT CURRENT_ROLE();
    
[/code]

Output:

> 
[code]
>     +----------------+
>     | CURRENT_ROLE() |
>     |----------------|
>     | SYSADMIN       |
>     +----------------+
>     
[/code]
