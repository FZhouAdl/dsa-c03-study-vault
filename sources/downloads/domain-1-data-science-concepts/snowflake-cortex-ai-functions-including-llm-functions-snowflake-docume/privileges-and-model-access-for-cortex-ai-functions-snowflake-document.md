---
title: "Privileges and model access for Cortex AI Functions | Snowflake Documentation"
source: https://docs.snowflake.com/user-guide/snowflake-cortex/aisql-privileges-and-access
cert_domain: domain-1-data-science-concepts
crawl_depth: 1
crawled: 2026-08-23
---

# Privileges and model access for Cortex AI Functions¶

## Cortex LLM privileges¶

Access to Snowflake Cortex AI Functions is gated by an account-level privilege and one of several database roles. Use the following sections to grant or revoke access at the account, role, or per-function level.

### USE AI FUNCTIONS on the account privilege¶

Important

Your users need both the USE AI FUNCTIONS account-level privilege (or a per-function `USE AI FUNCTION <name>` privilege) and one of the CORTEX_USER or AI_FUNCTIONS_USER database roles to use Snowflake Cortex AI Functions.

The USE AI FUNCTIONS account-level privilege includes the privileges that allow your users to call Snowflake Cortex AI functions. By default, the USE AI FUNCTIONS privilege is granted to the PUBLIC role. The PUBLIC role is automatically granted to all users and roles, allowing all users in your account to use the Snowflake Cortex AI functions. If you don’t want all your users to have this privilege, you can revoke access to the PUBLIC role and grant access to other roles.

If you need finer-grained control over which AI functions individual roles can call, see USE AI FUNCTION <name> — per-function privileges.

To control which roles have the USE AI FUNCTIONS privilege:

  * Revoke it from the PUBLIC role.
  * Grant it to specific roles.



Important

You must use the ACCOUNTADMIN role to manage the USE AI FUNCTIONS account-level privilege.

To revoke the USE AI FUNCTIONS account-level privilege from the PUBLIC role, run the following command:
[code] 
    REVOKE USE AI FUNCTIONS ON ACCOUNT
    FROM ROLE PUBLIC;
    
[/code]

Note

Revoking the USE AI FUNCTIONS account-level privilege prevents your users from accessing most Snowflake Cortex AI Functions. Your users need **both** the USE AI FUNCTIONS account-level privilege and one of the CORTEX_USER or AI_FUNCTIONS_USER database roles to use Snowflake Cortex AI Functions. If a user has the USE AI FUNCTIONS account-level privilege but doesn’t have the CORTEX_USER role, they can still use the AI_AGG and AI_SUMMARIZE_AGG functions.

After you’ve revoked the USE AI FUNCTIONS privilege from the PUBLIC role, you can use the ACCOUNTADMIN role to grant it to other roles in your Snowflake account.

The following example:

  1. Grants the USE AI FUNCTIONS privilege to `cortex_user_role`.
  2. Grants the `cortex_user_role` to `example_user`.


[code] 
    USE ROLE ACCOUNTADMIN;
    
    CREATE ROLE cortex_user_role;
    
    GRANT USE AI FUNCTIONS ON ACCOUNT TO ROLE cortex_user_role;
    
    GRANT ROLE cortex_user_role TO USER example_user;
    
[/code]

You can grant access to Snowflake Cortex AI Functions through roles that are commonly used by specific groups of users. For example, if you’ve created an `analyst` role that is used as a default role by analysts in your organization, you can grant these users access to Snowflake Cortex AI Functions with a single [GRANT <privileges> … TO ROLE](/sql-reference/sql/grant-privilege) statement. For more information about granting privileges to commonly used roles, see [User roles](/user-guide/admin-user-management#label-user-management-user-roles).
[code] 
    GRANT USE AI FUNCTIONS ON ACCOUNT TO ROLE analyst;
    
[/code]

Important

Currently, USE AI FUNCTIONS does not apply to AI Function queries that are run inside Snowflake native applications. A query with AI Function calls runs successfully regardless of whether the role has USE AI FUNCTIONS privilege.

### USE AI FUNCTION <name> — per-function privileges¶

In addition to the blanket USE AI FUNCTIONS privilege, you can grant per-function privileges using `USE AI FUNCTION <name>`. This allows ACCOUNTADMIN to control access at the individual function level instead of granting access to all AI functions at once.

The per-function privilege and the blanket USE AI FUNCTIONS privilege have an **OR** relationship:

  * If a role has USE AI FUNCTIONS, it can call **all** Cortex AI functions, regardless of any per-function grants or revocations.
  * If a role has only `USE AI FUNCTION AI_COMPLETE`, it can call only the AI_COMPLETE function.
  * If a role has both USE AI FUNCTIONS and a per-function grant, revoking the per-function grant does **not** affect access because the blanket privilege still applies.



Important

Per-function privileges require the same CORTEX_USER (or AI_FUNCTIONS_USER) database role as the blanket USE AI FUNCTIONS privilege.

#### Supported per-function privileges¶

The following table lists each Cortex AI function and its corresponding per-function privilege name for use with `GRANT USE AI FUNCTION <name> ON ACCOUNT TO ROLE <role_name>`.

Function| Per-function privilege name  
---|---  
[AI_COMPLETE](/sql-reference/functions/ai_complete)| AI_COMPLETE  
[AI_CLASSIFY](/sql-reference/functions/ai_classify)| AI_CLASSIFY  
[AI_FILTER](/sql-reference/functions/ai_filter)| AI_FILTER  
[AI_AGG](/sql-reference/functions/ai_agg)| AI_AGG  
[AI_EMBED](/sql-reference/functions/ai_embed)| AI_EMBED  
[AI_EXTRACT](/sql-reference/functions/ai_extract)| AI_EXTRACT  
[AI_SENTIMENT](/sql-reference/functions/ai_sentiment)| AI_SENTIMENT  
[AI_SUMMARIZE_AGG](/sql-reference/functions/ai_summarize_agg)| AI_SUMMARIZE_AGG  
[AI_SIMILARITY](/sql-reference/functions/ai_similarity)| AI_SIMILARITY  
[AI_TRANSCRIBE](/sql-reference/functions/ai_transcribe)| AI_TRANSCRIBE  
[AI_PARSE_DOCUMENT](/sql-reference/functions/ai_parse_document)| AI_PARSE_DOCUMENT  
[AI_REDACT](/sql-reference/functions/ai_redact)| AI_REDACT  
[AI_TRANSLATE](/sql-reference/functions/ai_translate)| AI_TRANSLATE  
[AI_COUNT_TOKENS](/sql-reference/functions/ai_count_tokens)| AI_COUNT_TOKENS  
[SNOWFLAKE.CORTEX.COMPLETE](/sql-reference/functions/complete-snowflake-cortex)| COMPLETE  
[SNOWFLAKE.CORTEX.CLASSIFY_TEXT](/sql-reference/functions/classify_text-snowflake-cortex)| CLASSIFY_TEXT  
[SNOWFLAKE.CORTEX.COUNT_TOKENS](/sql-reference/functions/count_tokens-snowflake-cortex)| COUNT_TOKENS  
[SNOWFLAKE.CORTEX.EMBED_TEXT](/sql-reference/functions/embed_text_1024-snowflake-cortex)| EMBED_TEXT  
[SNOWFLAKE.CORTEX.ENTITY_SENTIMENT](/sql-reference/functions/entity_sentiment-snowflake-cortex)| ENTITY_SENTIMENT  
[SNOWFLAKE.CORTEX.EXTRACT_ANSWER](/sql-reference/functions/extract_answer-snowflake-cortex)| EXTRACT_ANSWER  
[SNOWFLAKE.CORTEX.PARSE_DOCUMENT](/sql-reference/functions/parse_document-snowflake-cortex)| PARSE_DOCUMENT  
[SNOWFLAKE.CORTEX.SENTIMENT](/sql-reference/functions/sentiment-snowflake-cortex)| SENTIMENT  
[SNOWFLAKE.CORTEX.SUMMARIZE](/sql-reference/functions/summarize-snowflake-cortex)| SUMMARIZE  
[SNOWFLAKE.CORTEX.SUMMARIZE_AGG](/sql-reference/functions/summarize_agg-snowflake-cortex)| SUMMARIZE_AGG  
[SNOWFLAKE.CORTEX.TRANSLATE](/sql-reference/functions/translate-snowflake-cortex)| TRANSLATE  
[SNOWFLAKE.CORTEX.TRY_COMPLETE](/sql-reference/functions/try_complete-snowflake-cortex)| TRY_COMPLETE  
  
#### Granting per-function privileges¶

Use the ACCOUNTADMIN role to grant a per-function privilege. The syntax is:
[code] 
    GRANT USE AI FUNCTION <function_name> ON ACCOUNT TO ROLE <role_name>;
    
[/code]

The following example revokes the blanket privilege from PUBLIC, then grants only AI_COMPLETE access to a specific role:
[code] 
    USE ROLE ACCOUNTADMIN;
    
    -- Remove blanket access from PUBLIC
    REVOKE USE AI FUNCTIONS ON ACCOUNT FROM ROLE PUBLIC;
    
    -- Create a role with access to only AI_COMPLETE
    CREATE ROLE ai_complete_user_role;
    GRANT USE AI FUNCTION AI_COMPLETE ON ACCOUNT TO ROLE ai_complete_user_role;
    GRANT DATABASE ROLE SNOWFLAKE.CORTEX_USER TO ROLE ai_complete_user_role;
    
    GRANT ROLE ai_complete_user_role TO USER example_user;
    
[/code]

You can grant multiple per-function privileges to the same role to build a custom set of allowed functions:
[code] 
    USE ROLE ACCOUNTADMIN;
    
    CREATE ROLE ai_analyst_role;
    
    GRANT USE AI FUNCTION AI_COMPLETE ON ACCOUNT TO ROLE ai_analyst_role;
    GRANT USE AI FUNCTION AI_CLASSIFY ON ACCOUNT TO ROLE ai_analyst_role;
    GRANT USE AI FUNCTION AI_EXTRACT ON ACCOUNT TO ROLE ai_analyst_role;
    GRANT USE AI FUNCTION AI_TRANSLATE ON ACCOUNT TO ROLE ai_analyst_role;
    
    GRANT DATABASE ROLE SNOWFLAKE.CORTEX_USER TO ROLE ai_analyst_role;
    GRANT ROLE ai_analyst_role TO USER analyst_user;
    
[/code]

#### Revoking per-function privileges¶

To revoke a per-function privilege:
[code] 
    REVOKE USE AI FUNCTION <function_name> ON ACCOUNT FROM ROLE <role_name>;
    
[/code]

For example:
[code] 
    USE ROLE ACCOUNTADMIN;
    
    REVOKE USE AI FUNCTION AI_COMPLETE ON ACCOUNT FROM ROLE ai_analyst_role;
    
[/code]

After revocation, the role can no longer call AI_COMPLETE unless it also has the blanket USE AI FUNCTIONS privilege.

#### Viewing per-function grants¶

You can view per-function privilege grants using SHOW GRANTS:
[code] 
    -- View all grants to a specific role
    SHOW GRANTS TO ROLE ai_analyst_role;
    
    -- View all grants on the account
    SHOW GRANTS ON ACCOUNT;
    
[/code]

Per-function grants appear with the privilege name `USE AI FUNCTION <function_name>` (for example, `USE AI FUNCTION AI_COMPLETE`).

### Using AI Functions with Restricted Caller’s Rights¶

To use AI Functions with Restricted Caller’s Rights, you must grant the USE AI FUNCTIONS privilege (or the appropriate `USE AI FUNCTION <name>` per-function privilege) to both the session role and the service or application owner role.

For example, to use AI Functions inside a Snowpark Container Services (SPCS) service that runs with Restricted Caller’s Rights:

  1. Grant the USE AI FUNCTIONS privilege to the role used in the SPCS session (for example, `CHATBOT_USER_ROLE`):
[code] GRANT USE AI FUNCTIONS ON ACCOUNT TO ROLE CHATBOT_USER_ROLE;
         
[/code]

  2. Grant the caller version of the privilege to the service owner role:
[code] GRANT CALLER USE AI FUNCTIONS ON ACCOUNT TO ROLE <service_owner_role>;
         
[/code]




### CORTEX_USER database role¶

The CORTEX_USER database role in the SNOWFLAKE database includes the privileges that allow users to call Snowflake Cortex AI Functions. By default, the CORTEX_USER role is granted to the PUBLIC role. The PUBLIC role is automatically granted to all users and roles, so this allows all users in your account to use the Snowflake Cortex AI functions.

If you don’t want all users to have this privilege, you can revoke access to the PUBLIC role and grant access to other roles. The SNOWFLAKE.CORTEX_USER database role cannot be granted directly to a user. For more information, see [Using SNOWFLAKE database roles](/sql-reference/snowflake-db-roles#label-using-snowflake-db-roles).

To revoke the CORTEX_USER database role from the PUBLIC role, run the following commands using the ACCOUNTADMIN role:
[code] 
    REVOKE DATABASE ROLE SNOWFLAKE.CORTEX_USER
      FROM ROLE PUBLIC;
    
    REVOKE IMPORTED PRIVILEGES ON DATABASE SNOWFLAKE
      FROM ROLE PUBLIC;
    
[/code]

You can then selectively provide access to specific roles. A user with the ACCOUNTADMIN role can grant this role to a custom role in order to allow users to access Cortex AI functions. In the following example, use the ACCOUNTADMIN role and grant the user `some_user` the CORTEX_USER database role via the account role `cortex_user_role`, which you create for this purpose.
[code] 
    USE ROLE ACCOUNTADMIN;
    
    CREATE ROLE cortex_user_role;
    GRANT DATABASE ROLE SNOWFLAKE.CORTEX_USER TO ROLE cortex_user_role;
    
    GRANT ROLE cortex_user_role TO USER some_user;
    
[/code]

You can also grant access to Snowflake Cortex AI functions through existing roles commonly used by specific groups of users. (See [User roles](/user-guide/admin-user-management#label-user-management-user-roles).) For example, if you have created an `analyst` role that is used as a default role by analysts in your organization, you can easily grant these users access to Snowflake Cortex AI Functions with a single GRANT statement.
[code] 
    GRANT DATABASE ROLE SNOWFLAKE.CORTEX_USER TO ROLE analyst;
    
[/code]

### AI_FUNCTIONS_USER database role¶

The AI_FUNCTIONS_USER database role in the SNOWFLAKE database allows users to call Snowflake Cortex [scalar AI functions](/user-guide/snowflake-cortex/aisql#label-cortex-llm-ai-function) (all Cortex AI functions except the aggregate functions AI_AGG and AI_SUMMARIZE_AGG) without granting access to Cortex services such as Cortex Agent, Cortex Analyst, Cortex Fine-tuning, or Cortex Search.

Important

Your users need both the USE AI FUNCTIONS account-level privilege (or a per-function `USE AI FUNCTION <name>` privilege) plus one of CORTEX_USER and AI_FUNCTIONS_USER database roles to call Snowflake Cortex AI functions.

AI_FUNCTIONS_USER role is not granted to the PUBLIC role by default. ACCOUNTADMIN must explicitly grant this role to roles that require access to AI functions. The AI_FUNCTIONS_USER database role cannot be granted directly to users but must be granted to roles that users can assume. For more information, see [Using SNOWFLAKE database roles](/sql-reference/snowflake-db-roles#label-using-snowflake-db-roles).

The following example creates a custom role, grants the AI_FUNCTIONS_USER database role to it, and assigns the role to a user.
[code] 
    USE ROLE ACCOUNTADMIN;
    
    CREATE ROLE analyst_rl;
    GRANT DATABASE ROLE SNOWFLAKE.AI_FUNCTIONS_USER TO ROLE analyst_rl;
    
    GRANT ROLE analyst_rl TO USER some_user;
    
[/code]

Alternatively, to give all users access to scalar AI function capabilities, grant the AI_FUNCTIONS_USER role to the PUBLIC role.
[code] 
    USE ROLE ACCOUNTADMIN;
    
    GRANT DATABASE ROLE SNOWFLAKE.AI_FUNCTIONS_USER TO ROLE PUBLIC;
    
[/code]

### CORTEX_EMBED_USER database role¶

The CORTEX_EMBED_USER database role in the SNOWFLAKE database includes the privileges that allow users to call the text embedding functions AI_EMBED, EMBED_TEXT_768, and EMBED_TEXT_1024 and to create Cortex Search Services with managed vector embeddings. CORTEX_EMBED_USER allows you to grant embedding privileges separately from other Cortex AI capabilities.

Note

You can create Cortex Search Services with user-provided embeddings without the CORTEX_EMBED_USER role. In that case, you must generate the embeddings yourself, outside of Snowflake, and load them into a table.

Unlike the CORTEX_USER role, the CORTEX_EMBED_USER role is not granted to the PUBLIC role by default. You must explicitly grant this role to roles that require embedding capabilities if you have revoked the CORTEX_USER role. The CORTEX_EMBED_USER database role cannot be granted directly to users but must be granted to roles that users can assume. The following example illustrates this process.
[code] 
    USE ROLE ACCOUNTADMIN;
    
    CREATE ROLE cortex_embed_user_role;
    GRANT DATABASE ROLE SNOWFLAKE.CORTEX_EMBED_USER TO ROLE cortex_embed_user_role;
    
    GRANT ROLE cortex_embed_user_role TO USER some_user;
    
[/code]

Alternatively, to give all users access to embedding capabilities, grant the CORTEX_EMBED_USER role to the PUBLIC role as follows.
[code] 
    USE ROLE ACCOUNTADMIN;
    
    GRANT DATABASE ROLE SNOWFLAKE.CORTEX_EMBED_USER TO ROLE PUBLIC;
    
[/code]

### Using AI Functions in stored procedures with EXECUTE AS RESTRICTED CALLER¶

To use AI Functions inside stored procedures with `EXECUTE AS RESTRICTED CALLER`, grant the following privileges to the role that created the stored procedure:
[code] 
    GRANT INHERITED CALLER USAGE ON ALL SCHEMAS IN DATABASE snowflake TO ROLE <role_that_created_the_stored_procedure>;
    GRANT INHERITED CALLER USAGE ON ALL FUNCTIONS IN DATABASE snowflake TO ROLE <role_that_created_the_stored_procedure>;
    GRANT CALLER USAGE ON DATABASE snowflake TO ROLE <role_that_created_the_stored_procedure>;
    
[/code]

## Control model access¶

Snowflake Cortex provides two mechanisms to enforce access to models:

  * Role-based access control (RBAC) (recommended, fine-grained control)
  * Account-level allowlist parameter (legacy, planned for deprecation)



Use the account-level allowlist for account-wide defaults and RBAC for per-role control. You can also use both mechanisms together: access is granted if either mechanism permits it. Snowflake recommends migrating to RBAC exclusively.

Note

The ACCOUNTADMIN role always has access to all models on the account, regardless of the CORTEX_MODELS_ALLOWLIST parameter or model RBAC grants. Because access granted through a secondary role is also honored, a user who has ACCOUNTADMIN as a secondary role can access all models as well. To verify that the allowlist and RBAC restrictions work as expected, test them with a role other than ACCOUNTADMIN and disable secondary roles with `USE SECONDARY ROLES NONE`.

### Account-level allowlist parameter¶

Warning

`CORTEX_MODELS_ALLOWLIST` is being deprecated. Starting in August 2026, you can no longer change this parameter to a new value — the only permitted change will be to set it to `'None'`. Later in 2026, the parameter will be removed entirely. Snowflake recommends migrating to role-based access control (RBAC) now, which provides finer-grained, per-role control and is the go-forward model access mechanism.

You can control model access across your entire account using the CORTEX_MODELS_ALLOWLIST parameter. Supported features respect this parameter and block allowlist access to models that are not listed, unless the calling role has RBAC access to the model. For how the two mechanisms interact, see How RBAC and the allowlist interact.

The CORTEX_MODELS_ALLOWLIST parameter can be set to `'All'`, `'None'`, or to a comma-separated list of model names. Model names are case-sensitive and must be specified in lowercase (for example, `'mistral-large2'` rather than `'MISTRAL-LARGE2'`). This parameter can only be set at the account level, not at the user or session levels. Only the ACCOUNTADMIN role can set the parameter using the [ALTER ACCOUNT](/sql-reference/sql/alter-account) command.

Examples:

  * To allow access to all models:
[code] ALTER ACCOUNT SET CORTEX_MODELS_ALLOWLIST = 'All';
        
[/code]

  * To allow access to the `mistral-large2` and `llama3.1-70b` models:
[code] ALTER ACCOUNT SET CORTEX_MODELS_ALLOWLIST = 'mistral-large2,llama3.1-70b';
        
[/code]

  * To block allowlist access to all models (roles with RBAC grants can still use those models):
[code] ALTER ACCOUNT SET CORTEX_MODELS_ALLOWLIST = 'None';
        
[/code]




Snowflake recommends migrating to RBAC instead, as described in the following section.

### Role-based access control (RBAC)¶

Although Cortex models are not themselves Snowflake objects, Snowflake lets you create model objects in the SNOWFLAKE.MODELS schema that _represent_ the Cortex models. By applying RBAC to these objects, you can control access to models the same way you would any other Snowflake object. Supported features accept the identifiers of objects in SNOWFLAKE.MODELS wherever a model can be specified.

Tip

To use RBAC exclusively, set CORTEX_MODELS_ALLOWLIST to `'None'`.

#### Refresh model objects and application roles¶

The `SNOWFLAKE.MODELS` schema is automatically populated with objects representing all currently available Cortex models and is refreshed on a daily basis. The procedure also creates corresponding application roles and `CORTEX-MODEL-ROLE-ALL`, a role that covers all models.

If you want to pick up newly available models without waiting for the next daily refresh, an ACCOUNTADMIN can run the `SNOWFLAKE.MODELS.CORTEX_BASE_MODELS_REFRESH` stored procedure on demand:
[code] 
    CALL SNOWFLAKE.MODELS.CORTEX_BASE_MODELS_REFRESH();
    
[/code]

Tip

You can safely call `CORTEX_BASE_MODELS_REFRESH` at any time; it won’t create duplicate objects or roles.

After refreshing the model objects, you can verify that the models appear in the SNOWFLAKE.MODELS schema as follows:
[code] 
    SHOW MODELS IN SNOWFLAKE.MODELS;
    
[/code]

The returned list of models resembles the following:

created_on| name| model_type| database_name| schema_name| owner  
---|---|---|---|---|---  
2025-04-22 09:35:38.558 -0700| CLAUDE-4-5-SONNET| CORTEX_BASE| SNOWFLAKE| MODELS| SNOWFLAKE  
2025-04-22 09:36:16.793 -0700| LLAMA3.1-405B| CORTEX_BASE| SNOWFLAKE| MODELS| SNOWFLAKE  
2025-04-22 09:37:18.692 -0700| OPENAI-GPT-5.2| CORTEX_BASE| SNOWFLAKE| MODELS| SNOWFLAKE  
  
To verify that you can see the application roles associated with these models, use the SHOW APPLICATION ROLES command, as in the following example:
[code] 
    SHOW APPLICATION ROLES LIKE 'CORTEX-MODEL%' IN APPLICATION SNOWFLAKE;
    
[/code]

The list of application roles resembles the following:

created_on| name| owner| comment| owner_role_type  
---|---|---|---|---  
2025-04-22 09:35:38.558 -0700| CORTEX-MODEL-ROLE-ALL| SNOWFLAKE| MODELS| APPLICATION  
2025-04-22 09:36:16.793 -0700| CORTEX-MODEL-ROLE-LLAMA3.1-405B| SNOWFLAKE| MODELS| APPLICATION  
  
#### Grant application roles to user roles¶

You can grant model application roles to specific user roles in your account.

  * To grant a role access to a specific model:
[code] GRANT APPLICATION ROLE SNOWFLAKE."CORTEX-MODEL-ROLE-LLAMA3.1-70B" TO ROLE MY_ROLE;
        
[/code]

  * To grant a role access to all current and future models:
[code] GRANT APPLICATION ROLE SNOWFLAKE."CORTEX-MODEL-ROLE-ALL" TO ROLE MY_ROLE;
        
[/code]




#### Default bootstrap of CORTEX-MODEL-ROLE-ALL¶

As part of the [CORTEX_MODELS_ALLOWLIST deprecation](/release-notes/bcr-bundles/un-bundled/bcr-2378) behavior change, Snowflake grants the `CORTEX-MODEL-ROLE-ALL` application role to the `SNOWFLAKE.PUBLIC` application role. `SNOWFLAKE.PUBLIC` is granted to the account-level PUBLIC role, so users inherit access to all current and future Cortex models by default (subject to Cortex LLM privileges). Accounts that have `CORTEX_MODELS_ALLOWLIST` set to `'All'` (including new accounts, which default to `'All'`) receive this bootstrap grant.

This bootstrap path is separate from grants you make yourself with `GRANT APPLICATION ROLE ... TO ROLE PUBLIC`. Use the stored procedures in this section to manage the bootstrap grant. Use ordinary `GRANT` and `REVOKE APPLICATION ROLE` statements only for grants you manage yourself.

##### Investigate whether the bootstrap grant is present¶

To confirm that `CORTEX-MODEL-ROLE-ALL` is granted through `SNOWFLAKE.PUBLIC`, run the following as ACCOUNTADMIN and look for `CORTEX-MODEL-ROLE-ALL` in the result:
[code] 
    SHOW GRANTS TO APPLICATION ROLE SNOWFLAKE.PUBLIC;
    
[/code]

You can also list model application roles and see which account roles inherit `SNOWFLAKE.PUBLIC`:
[code] 
    SHOW APPLICATION ROLES LIKE 'CORTEX-MODEL%' IN APPLICATION SNOWFLAKE;
    SHOW GRANTS OF APPLICATION ROLE SNOWFLAKE.PUBLIC;
    
[/code]

Tip

`SNOWFLAKE.PUBLIC` is an application role on the SNOWFLAKE database. It is not the same object as the account-level PUBLIC role. Use `SHOW GRANTS TO APPLICATION ROLE SNOWFLAKE.PUBLIC` to audit the bootstrap grant. `SHOW GRANTS TO ROLE PUBLIC` shows account-level PUBLIC grants and might not reflect the bootstrap path.

##### Revoke or restore the bootstrap grant¶

To remove default access to all models through the bootstrap grant, call the following procedure as ACCOUNTADMIN:
[code] 
    CALL SNOWFLAKE.LOCAL.REVOKE_FROM_PUBLIC_APPLICATION_ROLE(
      'APP_ROLE',
      'CORTEX-MODEL-ROLE-ALL'
    );
    
[/code]

After the revoke, re-run `SHOW GRANTS TO APPLICATION ROLE SNOWFLAKE.PUBLIC` and confirm that `CORTEX-MODEL-ROLE-ALL` is no longer listed. Then grant specific model application roles to the roles that need them.

To restore the bootstrap grant after you’ve revoked it:
[code] 
    CALL SNOWFLAKE.LOCAL.GRANT_TO_PUBLIC_APPLICATION_ROLE(
      'APP_ROLE',
      'CORTEX-MODEL-ROLE-ALL'
    );
    
[/code]

Important

A raw `REVOKE APPLICATION ROLE SNOWFLAKE."CORTEX-MODEL-ROLE-ALL" FROM ROLE PUBLIC` statement does **not** remove the bootstrap grant from `SNOWFLAKE.PUBLIC`, and it doesn’t persist that intent across Snowflake upgrades. Always use `SNOWFLAKE.LOCAL.REVOKE_FROM_PUBLIC_APPLICATION_ROLE` to revoke the bootstrap grant.

If you previously ran `GRANT APPLICATION ROLE SNOWFLAKE."CORTEX-MODEL-ROLE-ALL" TO ROLE PUBLIC` yourself, you can still revoke that customer-managed grant with `REVOKE APPLICATION ROLE SNOWFLAKE."CORTEX-MODEL-ROLE-ALL" FROM ROLE PUBLIC`. That revoke doesn’t replace the stored-procedure revoke for the `SNOWFLAKE.PUBLIC` bootstrap path.

#### Use model objects with supported features¶

To use model objects with supported Cortex features, specify the identifier of the model object in SNOWFLAKE.MODELS as the model argument. You can use a fully-qualified identifier, a partial identifier, or a simple model name that will be automatically resolved to SNOWFLAKE.MODELS.

  * Using a fully-qualified identifier:
[code] SELECT AI_COMPLETE('SNOWFLAKE.MODELS."LLAMA3.1-70B"', 'Hello');
        
[/code]

  * Using a partial identifier:
[code] USE DATABASE SNOWFLAKE;
        USE SCHEMA MODELS;
        SELECT AI_COMPLETE('LLAMA3.1-70B', 'Hello');
        
[/code]

  * Using automatic lookup with a simple model name:
[code] -- Automatically resolves to SNOWFLAKE.MODELS."LLAMA3.1-70B"
        SELECT AI_COMPLETE('llama3.1-70b', 'Hello');
        
[/code]




#### How RBAC and the allowlist interact¶

A number of Cortex features accept a model name as a string argument, for example `AI_COMPLETE('_model_ ', '_prompt_ ')`. When you provide a model name:

  1. Cortex first attempts to locate a matching model object in `SNOWFLAKE.MODELS`. If you provide an unqualified name like `'x'`, it automatically looks for `SNOWFLAKE.MODELS."X"`.
  2. Access is granted if **either** of the following is true:
     * The calling role has USAGE on the model object (via an application role granted through RBAC), **or**
     * The model name matches an entry in `CORTEX_MODELS_ALLOWLIST` (or the allowlist is set to `'All'`).
  3. Access is denied only if **both** checks fail.



The following example shows that either mechanism can grant access. The allowlist allows `mistral-large2`; the role has RBAC access to `LLAMA3.1-70B` but not to `claude-sonnet-4-6`. The last query fails because neither mechanism permits that model.
[code] 
    -- set up access
    USE SECONDARY ROLES NONE;
    USE ROLE ACCOUNTADMIN;
    ALTER ACCOUNT SET CORTEX_MODELS_ALLOWLIST = 'MISTRAL-LARGE2';
    CALL SNOWFLAKE.MODELS.CORTEX_BASE_MODELS_REFRESH();
    GRANT APPLICATION ROLE SNOWFLAKE."CORTEX-MODEL-ROLE-LLAMA3.1-70B" TO ROLE PUBLIC;
    
    -- test access
    USE ROLE PUBLIC;
    
    -- this succeeds because mistral-large2 is in the allowlist
    SELECT AI_COMPLETE('MISTRAL-LARGE2', 'Hello');
    
    -- this succeeds because the role has access to the model object
    SELECT AI_COMPLETE('SNOWFLAKE.MODELS."LLAMA3.1-70B"', 'Hello');
    
    -- this fails because the first argument is
    -- neither an identifier for an accessible model object
    -- nor is it a model name in the allowlist
    SELECT AI_COMPLETE('claude-sonnet-4-6', 'Hello');
    
[/code]

### Common pitfalls¶

  * Access to a model (whether by allowlist or RBAC) does not always mean that it can be used. It may still be subject to cross-region, deprecation, or other availability constraints. These restrictions can result in error messages that seem similar to model access errors. To check which Cortex Base Models are currently available in your account and their lifecycle status, use [SHOW CORTEX BASE MODELS](/sql-reference/sql/show-cortex-base-models).
  * Model access controls only govern the use of a model and not the use of a feature itself. A feature can have its own access controls. For example, access to `AI_COMPLETE` is governed by the `CORTEX_USER` or `AI_FUNCTIONS_USER` database role and the USE AI FUNCTIONS account-level privilege. For more information, see Cortex LLM privileges.
  * Not all features support model access controls. For more information about what a feature supports, see the supported features table.
  * Secondary roles can obscure permissions. For example, if a user has ACCOUNTADMIN as a secondary role, all model objects may appear accessible. Disable secondary roles temporarily when verifying permissions.
  * Revoking `CORTEX-MODEL-ROLE-ALL` from the account-level PUBLIC role does not remove the bootstrap grant on `SNOWFLAKE.PUBLIC`. Use `SHOW GRANTS TO APPLICATION ROLE SNOWFLAKE.PUBLIC` to check, and `SNOWFLAKE.LOCAL.REVOKE_FROM_PUBLIC_APPLICATION_ROLE` to revoke.
  * Qualified model object identifiers are quoted and therefore case-sensitive. For more information, see [QUOTED_IDENTIFIERS_IGNORE_CASE](/sql-reference/parameters#label-quoted-identifiers-ignore-case).



### Migrate from the allowlist to RBAC¶

Because `CORTEX_MODELS_ALLOWLIST` is planned for deprecation, Snowflake recommends migrating to model RBAC. The following steps walk through a complete migration.

#### Step 1: Check your current allowlist¶

As ACCOUNTADMIN, check what models are currently allowlisted:
[code] 
    SHOW PARAMETERS LIKE 'CORTEX_MODELS_ALLOWLIST' IN ACCOUNT;
    
[/code]

Note the value. For example:

  * `'All'`: all models are accessible. Rely on the CORTEX-MODEL-ROLE-ALL bootstrap, or grant `CORTEX-MODEL-ROLE-ALL` yourself to replace this.
  * `'model-a,model-b'`: only specific models are accessible. Grant per-model roles to replace this.
  * `'None'`: no models are accessible. You can set RBAC grants selectively.



#### Step 2: Grant equivalent model application roles¶

Because `CORTEX_MODELS_ALLOWLIST` applies account-wide, the direct RBAC equivalent is granting model application roles to the PUBLIC role, which is automatically granted to all users. Start by replicating the allowlist behavior exactly, then tighten access to specific roles as needed.

Warning

Granting to PUBLIC is not sufficient in all execution contexts. In the following scenarios, PUBLIC’s grants may not be active, and you must grant model application roles directly to the specific role in use:

  * **Secondary roles disabled** : When a session runs `USE SECONDARY ROLES NONE`, only the active primary role’s grants apply. If the active role doesn’t inherit PUBLIC’s model grants through its own role chain, the call will fail.
  * **Native Apps** : Native app execution contexts use the app’s own role hierarchy. Account-level PUBLIC grants don’t automatically carry through.
  * **Restricted Caller’s Rights (RCR) stored procedures** : The procedure runs under the caller’s role, but inherited grants from PUBLIC may not be available depending on how the procedure is defined.



Test all your workloads — including stored procedures, native apps, and any session that sets secondary roles — before disabling the allowlist.

The `SNOWFLAKE.MODELS` schema is refreshed daily and already contains objects for all available models. To see which model roles are available:
[code] 
    SHOW APPLICATION ROLES LIKE 'CORTEX-MODEL%' IN APPLICATION SNOWFLAKE;
    
[/code]

To replicate an allowlist that includes specific models (equivalent to `CORTEX_MODELS_ALLOWLIST = 'model-a,model-b'`), grant those roles to PUBLIC:
[code] 
    USE ROLE ACCOUNTADMIN;
    
    GRANT APPLICATION ROLE SNOWFLAKE."CORTEX-MODEL-ROLE-MODEL-A" TO ROLE PUBLIC;
    GRANT APPLICATION ROLE SNOWFLAKE."CORTEX-MODEL-ROLE-MODEL-B" TO ROLE PUBLIC;
    
[/code]

To replicate an allowlist that allows all models (equivalent to `CORTEX_MODELS_ALLOWLIST = 'All'`), either rely on the automatic CORTEX-MODEL-ROLE-ALL bootstrap through `SNOWFLAKE.PUBLIC`, or grant the role yourself to PUBLIC:
[code] 
    USE ROLE ACCOUNTADMIN;
    
    GRANT APPLICATION ROLE SNOWFLAKE."CORTEX-MODEL-ROLE-ALL" TO ROLE PUBLIC;
    
[/code]

Once the allowlist is disabled, you can take advantage of RBAC’s finer-grained control by removing broad all-models access and granting specific model roles only to the roles that need them. If all-models access came from the `SNOWFLAKE.PUBLIC` bootstrap, revoke it with the stored procedure. If you granted `CORTEX-MODEL-ROLE-ALL` to PUBLIC yourself, use `REVOKE APPLICATION ROLE`:
[code] 
    USE ROLE ACCOUNTADMIN;
    
    -- If the bootstrap grant is present (preferred check: SHOW GRANTS TO APPLICATION ROLE SNOWFLAKE.PUBLIC)
    CALL SNOWFLAKE.LOCAL.REVOKE_FROM_PUBLIC_APPLICATION_ROLE(
      'APP_ROLE',
      'CORTEX-MODEL-ROLE-ALL'
    );
    
    -- Only if you previously granted CORTEX-MODEL-ROLE-ALL to PUBLIC yourself
    REVOKE APPLICATION ROLE SNOWFLAKE."CORTEX-MODEL-ROLE-ALL" FROM ROLE PUBLIC;
    
    -- Grant specific models only to the roles that need them
    GRANT APPLICATION ROLE SNOWFLAKE."CORTEX-MODEL-ROLE-MODEL-A" TO ROLE analyst_role;
    GRANT APPLICATION ROLE SNOWFLAKE."CORTEX-MODEL-ROLE-MODEL-B" TO ROLE data_science_role;
    
[/code]

If you want to restrict model access to a specific group of users rather than all users in the account, grant the model role to a custom role and make sure broad all-models access isn’t still available through PUBLIC or the bootstrap path:
[code] 
    USE ROLE ACCOUNTADMIN;
    
    -- Create a role for users who need model access
    CREATE ROLE IF NOT EXISTS cortex_ai_role;
    
    -- Grant the desired model roles to that custom role
    GRANT APPLICATION ROLE SNOWFLAKE."CORTEX-MODEL-ROLE-MODEL-A" TO ROLE cortex_ai_role;
    GRANT APPLICATION ROLE SNOWFLAKE."CORTEX-MODEL-ROLE-MODEL-B" TO ROLE cortex_ai_role;
    
    -- Remove bootstrap all-models access if present
    CALL SNOWFLAKE.LOCAL.REVOKE_FROM_PUBLIC_APPLICATION_ROLE(
      'APP_ROLE',
      'CORTEX-MODEL-ROLE-ALL'
    );
    
    -- Only if you previously granted CORTEX-MODEL-ROLE-ALL to PUBLIC yourself
    REVOKE APPLICATION ROLE SNOWFLAKE."CORTEX-MODEL-ROLE-ALL" FROM ROLE PUBLIC;
    
    -- Assign the custom role to the users who need access
    GRANT ROLE cortex_ai_role TO USER alice;
    GRANT ROLE cortex_ai_role TO USER bob;
    
[/code]

#### Step 3: Verify access with RBAC¶

Before disabling the allowlist, verify that the RBAC grants work as expected. Switch to a non-ACCOUNTADMIN role that has the model role grant and run a test call:
[code] 
    USE ROLE PUBLIC;
    
    -- Should succeed if CORTEX-MODEL-ROLE-MODEL-A was granted to PUBLIC
    SELECT AI_COMPLETE('SNOWFLAKE.MODELS."MODEL-A"', 'Hello');
    
[/code]

To see which models the current role has access to:
[code] 
    SHOW CORTEX BASE MODELS IN SCHEMA SNOWFLAKE.MODELS;
    
[/code]

#### Step 4: Disable the allowlist¶

Once you’ve confirmed that RBAC grants are working, disable the allowlist so that RBAC is the only access mechanism:
[code] 
    USE ROLE ACCOUNTADMIN;
    
    ALTER ACCOUNT SET CORTEX_MODELS_ALLOWLIST = 'None';
    
[/code]

Setting the allowlist to `'None'` disables the allowlist fallback entirely, so only RBAC grants determine access.

### Supported features¶

Model access controls are supported by the following features:

Feature| Account-level allowlist| Role-based access control| Notes  
---|---|---|---  
[AI_COMPLETE](/sql-reference/functions/ai_complete)| ✔| ✔|   
[AI_CLASSIFY](/sql-reference/functions/ai_classify)| ✔| ✔| If the model powering this function is not allowed, the error message contains information about how to modify the allowlist or model RBAC grants.  
[AI_FILTER](/sql-reference/functions/ai_filter)| ✔| ✔| If the model powering this function is not allowed, the error message contains information about how to modify the allowlist or model RBAC grants.  
[AI_AGG](/sql-reference/functions/ai_agg)| ✔| ✔| If the model powering this function is not allowed, the error message contains information about how to modify the allowlist or model RBAC grants.  
[AI_SUMMARIZE_AGG](/sql-reference/functions/ai_summarize_agg)| ✔| ✔| If the model powering this function is not allowed, the error message contains information about how to modify the allowlist or model RBAC grants.  
[AI_TRANSCRIBE](/sql-reference/functions/ai_transcribe)| ✔| ✔|   
[AI_EXTRACT](/sql-reference/functions/ai_extract)| ✔| ✔|   
[AI_SENTIMENT](/sql-reference/functions/ai_sentiment)| ✔| ✔|   
[AI_TRANSLATE](/sql-reference/functions/ai_translate)| ✔| ✔|   
[AI_PARSE_DOCUMENT](/sql-reference/functions/ai_parse_document)| ✔| ✔|   
[AI_REDACT](/sql-reference/functions/ai_redact)| ✔| ✔|   
[Cortex REST API](/user-guide/snowflake-cortex/cortex-rest-api)| ✔| ✔|   
[Cortex Playground](/user-guide/snowflake-cortex/cortex-playground)| ✔| ✔|
