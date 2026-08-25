---
title: "Context functions | Snowflake Documentation"
source: https://docs.snowflake.com/sql-reference/functions-context
cert_domain: domain-2-data-preparation-and-feature-engineering
crawl_depth: 1
crawled: 2026-08-23
---

# Context functions¶

This family of functions allows for the gathering of information about the context in which the statement is executed. These functions are evaluated at most once per statement.

## List of functions¶

Sub-category| Function| Notes  
---|---|---  
General context| [CURRENT_CLIENT](/sql-reference/functions/current_client)|   
| [CURRENT_DATE](/sql-reference/functions/current_date)|   
| [CURRENT_IP_ADDRESS](/sql-reference/functions/current_ip_address)|   
| [CURRENT_REGION](/sql-reference/functions/current_region)|   
| [CURRENT_TIME](/sql-reference/functions/current_time)|   
| [CURRENT_TIMESTAMP](/sql-reference/functions/current_timestamp)|   
| [CURRENT_VERSION](/sql-reference/functions/current_version)|   
| [GETDATE](/sql-reference/functions/getdate)| Alias for CURRENT_TIMESTAMP.  
| [LOCALTIME](/sql-reference/functions/localtime)| Alias for CURRENT_TIME.  
| [LOCALTIMESTAMP](/sql-reference/functions/localtimestamp)| Alias for CURRENT_TIMESTAMP.  
| [SYSDATE](/sql-reference/functions/sysdate)|   
| [SYSTIMESTAMP](/sql-reference/functions/systimestamp)|   
| [SYS_CONTEXT](/sql-reference/functions/sys_context)|   
Session context| [ALL_USER_NAMES](/sql-reference/functions/all_user_names)|   
| [CURRENT_ACCOUNT](/sql-reference/functions/current_account)| Returns account locator.  
| [CURRENT_ACCOUNT_NAME](/sql-reference/functions/current_account_name)| Returns account name.  
| [CURRENT_ORGANIZATION_NAME](/sql-reference/functions/current_organization_name)|   
| [CURRENT_ORGANIZATION_USER](/sql-reference/functions/current_organization_user)|   
| [CURRENT_ROLE](/sql-reference/functions/current_role)|   
| [CURRENT_AVAILABLE_ROLES](/sql-reference/functions/current_available_roles)|   
| [CURRENT_SECONDARY_ROLES](/sql-reference/functions/current_secondary_roles)|   
| [CURRENT_SESSION](/sql-reference/functions/current_session)|   
| [CURRENT_STATEMENT](/sql-reference/functions/current_statement)|   
| [CURRENT_TRANSACTION](/sql-reference/functions/current_transaction)|   
| [CURRENT_USER](/sql-reference/functions/current_user)|   
| [GETVARIABLE](/sql-reference/functions/getvariable)|   
| [SET_SYS_CONTEXT](/sql-reference/functions/set_sys_context)|   
| [LAST_QUERY_ID](/sql-reference/functions/last_query_id)|   
| [LAST_TRANSACTION](/sql-reference/functions/last_transaction)|   
Session object context| [CURRENT_DATABASE](/sql-reference/functions/current_database)|   
| [CURRENT_ROLE_TYPE](/sql-reference/functions/current_role_type)|   
| [CURRENT_SCHEMA](/sql-reference/functions/current_schema)|   
| [CURRENT_SCHEMAS](/sql-reference/functions/current_schemas)|   
| [CURRENT_WAREHOUSE](/sql-reference/functions/current_warehouse)|   
| [INVOKER_ROLE](/sql-reference/functions/invoker_role)|   
| [INVOKER_SHARE](/sql-reference/functions/invoker_share)|   
| [IS_AGENT_ACTIVATED (SYS_CONTEXT function)](/sql-reference/functions/is_agent_activated)|   
| [IS_DATABASE_ROLE_ACTIVATED (SYS_CONTEXT function)](/sql-reference/functions/is_database_role_activated)|   
| [IS_APPLICATION_ROLE_ACTIVATED (SYS_CONTEXT function)](/sql-reference/functions/is_application_role_activated)|   
| [IS_APPLICATION_ROLE_IN_SESSION](/sql-reference/functions/is_application_role_in_session)|   
| [IS_DATABASE_ROLE_IN_SESSION](/sql-reference/functions/is_database_role_in_session)|   
| [IS_GRANTED_TO_INVOKER_ROLE](/sql-reference/functions/is_granted_to_invoker_role)|   
| [IS_INSTANCE_ROLE_IN_SESSION](/sql-reference/functions/is_instance_role_in_session)|   
| [IS_ROLE_ACTIVATED (SYS_CONTEXT function)](/sql-reference/functions/is_role_activated)|   
| [IS_ROLE_IN_SESSION](/sql-reference/functions/is_role_in_session)|   
| [POLICY_CONTEXT](/sql-reference/functions/policy_context)|   
Alert context| [GET_CONDITION_QUERY_UUID](/sql-reference/functions/get_condition_query_uuid)|   
Organization context| [IS_GROUP_ACTIVATED (SYS_CONTEXT function)](/sql-reference/functions/is_group_activated)|   
| [IS_GROUP_IMPORTED (SYS_CONTEXT function)](/sql-reference/functions/is_group_imported)|   
| [IS_USER_IMPORTED (SYS_CONTEXT function)](/sql-reference/functions/is_user_imported)|   
  
## Usage notes¶

  * Context functions generally do not require arguments (except for [SYS_CONTEXT](/sql-reference/functions/sys_context)).

  * To comply with the ANSI standard, the following context functions can be called without parentheses in SQL statements:

    * CURRENT_DATE
    * CURRENT_TIME
    * CURRENT_TIMESTAMP
    * CURRENT_USER
    * LOCALTIME
    * LOCALTIMESTAMP

Note

If you are setting a [Snowflake Scripting variable](/developer-guide/snowflake-scripting/variables) to an expression that calls one of these functions (for example, `my_var := <function_name>();`), you must include the parentheses.




## Examples¶

Display the current warehouse, database, and schema for the session:
[code] 
    SELECT CURRENT_WAREHOUSE(), CURRENT_DATABASE(), CURRENT_SCHEMA();
    
[/code]
[code] 
    +---------------------+--------------------+------------------+
    | CURRENT_WAREHOUSE() | CURRENT_DATABASE() | CURRENT_SCHEMA() |
    |---------------------+--------------------+------------------+
    | MY_WAREHOUSE        | MY_DB              | PUBLIC           |
    |---------------------+--------------------+------------------+
    
[/code]

Display the current date, time, and timestamp (note that parentheses are not required to call these functions):
[code] 
    SELECT CURRENT_DATE, CURRENT_TIME, CURRENT_TIMESTAMP;
    
[/code]
[code] 
    +--------------+--------------+-------------------------------+
    | CURRENT_DATE | CURRENT_TIME | CURRENT_TIMESTAMP             |
    |--------------+--------------+-------------------------------|
    | 2024-06-07   | 10:45:15     | 2024-06-07 10:45:15.064 -0700 |
    +--------------+--------------+-------------------------------+
    
[/code]

In a Snowflake Scripting block, call the CURRENT_DATE function without parentheses to set a variable in a SQL statement:
[code] 
    EXECUTE IMMEDIATE
    $$
    DECLARE
      currdate DATE;
    BEGIN
      SELECT CURRENT_DATE INTO currdate;
      RETURN currdate;
    END;
    $$
    ;
    
[/code]
[code] 
    +-----------------+
    | anonymous block |
    |-----------------|
    | 2024-06-07      |
    +-----------------+
    
[/code]

In a Snowflake Scripting block, attempting to set a variable to an expression that calls the CURRENT_DATE function without parentheses results in an error:
[code] 
    EXECUTE IMMEDIATE
    $$
    DECLARE
      today DATE;
    BEGIN
      today := CURRENT_DATE;
      RETURN today;
    END;
    $$
    ;
    
[/code]
[code] 
    000904 (42000): SQL compilation error: error line 5 at position 11
    invalid identifier 'CURRENT_DATE'
    
[/code]

The same block returns the current date when the function is called with the parentheses:
[code] 
    EXECUTE IMMEDIATE
    $$
    DECLARE
      today DATE;
    BEGIN
      today := CURRENT_DATE();
      RETURN today;
    END;
    $$
    ;
    
[/code]
[code] 
    +-----------------+
    | anonymous block |
    |-----------------|
    | 2024-06-07      |
    +-----------------+
    
[/code]
