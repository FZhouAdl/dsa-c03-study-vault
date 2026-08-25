---
title: "Secure objects: Redaction of information in error messages | Snowflake Documentation"
source: https://docs.snowflake.com/release-notes/bcr-bundles/un-bundled/bcr-1858
cert_domain: domain-3-model-development
crawl_depth: 1
crawled: 2026-08-23
---

# Secure objects: Redaction of information in error messages¶

Error messages related to secure objects behave as follows:

Before the change:
    

Error messages related to secure objects show the full message.

After the change:
    

Error messages related to secure objects might be redacted.

The change applies to error messages related to the following types of objects:

  * [Secure views](/user-guide/views-secure)
  * [Secure functions](/developer-guide/secure-udf-procedure) (including secure table functions)
  * [Masking policies](/user-guide/security-column-intro)
  * [Row access policy bodies](/user-guide/security-row-intro)



For more information about secure objects, see [Use secure objects to control data access](/user-guide/data-sharing-secure-views).

When an error is detected during the expansion or evaluation of a secure object, the error message is considered for redaction. When an error message is redacted, the error code remains unchanged.

Two types of changes to error messages are possible: redaction during execution and redaction in metadata after execution. These types of changes are described in the following sections.

## Redaction during execution¶

A whole error message or a part of an error message can be redacted when the error is returned during an operation. Generally, this type of error message redaction occurs when a user tries to use a secure object without having the [OWNERSHIP](/user-guide/security-access-control-privileges) privilege on the secure object.

## Redaction in metadata after execution¶

Users can view metadata about errors after they occur, including the error messages. For example, users can view this metadata in the **Query History** page in Snowsight, or by querying views and calling functions in the [Snowflake Information Schema](/sql-reference/info-schema). When an error message is redacted during execution, the error message is always redacted in the metadata after execution for all users.

When an error message isn’t redacted during execution, the message appears unchanged in the metadata for some users and is redacted for other users. The error message is unchanged in the metadata in either of the following cases:

  * The user viewing the metadata has the [AUDIT](/user-guide/security-access-control-privileges) privilege.
  * The user viewing the metadata has the [ENABLE_UNREDACTED_SECURE_OBJECT_ERROR](/sql-reference/parameters#label-enable-unredacted-secure-object-error) user parameter set to `TRUE`. A user with the AUDIT privilege can set this parameter for a user.
  * The user viewing the metadata executed the statement that caused the error.



In all other cases, the error message is redacted in the metadata. Redacted error messages include the text: `Error in secure object`.

## Examples of error message redaction¶

The following examples show error messages that are redacted. The redaction can occur during execution or in metadata after execution.

### Example 1: Querying a secure view¶

In the following example, a user with the SELECT privilege on a secure view executes a query on the view that returns an error.

Create the secure view:
[code] 
    CREATE SECURE VIEW myview
      AS SELECT a FROM mytable;
    
[/code]

Drop the table used in the view query:
[code] 
    DROP TABLE mytable;
    
[/code]

Execute a query on the view:
[code] 
    SELECT * FROM myview;
    
[/code]

#### Error message displayed to all users before the change¶
[code] 
    002037 (42601): SQL compilation error:
    Failure during expansion of view 'MYVIEW': SQL compilation error:
    Object 'DB.SC.MYTABLE' does not exist or not authorized.
    
[/code]

#### Redacted error message displayed to some users after the change¶
[code] 
    002037 (42601): SQL compilation error:
    Failure during expansion of view 'MYVIEW': Error in secure object
    
[/code]

### Example 2: Running a query that calls a secure function¶

In the following examples, a user with the USAGE privilege on a secure function executes a query that calls the secure function, but the secure function returns an error.

#### Example 2a: The function arguments result in an error¶

Create the secure function:
[code] 
    CREATE SECURE FUNCTION myfunction1(x FLOAT, y FLOAT)
      RETURNS FLOAT
      LANGUAGE SQL
    AS
      'SELECT x / y';
    
[/code]

Execute a query that calls the secure function:
[code] 
    SELECT myfunction1(1, 0);
    
[/code]

##### Error message displayed to all users before the change¶
[code] 
    100051 (22012): Division by zero
    
[/code]

##### Redacted error message displayed to some users after the change¶
[code] 
    100051 (22012): Error in secure object
    
[/code]

#### Example 2b: An object the function depends on is deleted¶

Create the secure function:
[code] 
    CREATE SECURE FUNCTION myfunction2()
      RETURNS TABLE (a NUMBER)
      LANGUAGE SQL
    AS
      'SELECT * FROM mytable';
    
[/code]

Drop the table used in the function:
[code] 
    DROP TABLE mytable;
    
[/code]

Execute a query that calls the secure function:
[code] 
    SELECT * FROM TABLE(myfunction2());
    
[/code]

##### Error message displayed to all users before the change¶
[code] 
    002003 (42S02): SQL compilation error:
    Object 'DB.SC.MYTABLE' does not exist or not authorized
    
[/code]

##### Redacted error message displayed to some users after the change¶
[code] 
    002003 (42S02): Error in secure object
    
[/code]

### Example 3: A masking policy returns an error¶

In the following example, a user runs a query on a view with a masking policy that encounters an error.

Create a masking policy:
[code] 
    CREATE MASKING POLICY allowed_role_names_mp as (val NUMBER) RETURNS NUMBER ->
      CASE
        WHEN EXISTS
          (SELECT role FROM allowed_roles WHERE role = CURRENT_ROLE()) THEN val
        ELSE '********'
      END;
    
[/code]

Create a view and set the masking policy on a column in the view:
[code] 
    CREATE TABLE test_masking_policy(x NUMBER) AS
      SELECT * FROM VALUES (1), (2), (3);
    
    CREATE VIEW myview_mp
      AS SELECT * FROM test_masking_policy;
    
    ALTER VIEW myview_mp
      MODIFY COLUMN x SET MASKING POLICY allowed_role_names_mp;
    
[/code]

Drop the table used in the masking policy:
[code] 
    DROP TABLE allowed_roles;
    
[/code]

Execute a query on the view as a user that doesn’t have ownership privileges on the masking policy:
[code] 
    SELECT * FROM myview_mp;
    
[/code]

#### Error message displayed to all users before the change¶
[code] 
    002003 (42S02): SQL compilation error:
    Object 'DB.SC.ALLOWED_ROLES' does not exist or not authorized.
    
[/code]

#### Redacted error message displayed to some users after the change¶
[code] 
    002003 (42S02): Error in secure object
    
[/code]

### Example 4: A row access policy returns an error¶

In the following example, a user runs a query on a view with a row access policy and encounters an error.

Create a row access policy:
[code] 
    CREATE OR REPLACE ROW ACCESS POLICY myrap AS (role NUMBER) RETURNS BOOLEAN ->
      EXISTS (
        SELECT 1 FROM allowed_roles
          WHERE role::STRING = CURRENT_ROLE());
    
[/code]

Create a view and add the row access policy on the view:
[code] 
    CREATE TABLE test_row_access_policy(x NUMBER) AS
      SELECT * FROM VALUES (1), (2), (3);
    
    CREATE VIEW myview_rap
      AS SELECT * FROM test_row_access_policy;
    
    ALTER VIEW myview_rap
      ADD ROW ACCESS POLICY myrap ON (x);
    
[/code]

Drop the table used in the row access policy:
[code] 
    DROP TABLE allowed_roles;
    
[/code]

Query the view as a user that doesn’t have OWNERSHIP privileges on the row access policy:
[code] 
    SELECT * FROM myview_rap;
    
[/code]

#### Error message displayed to all users before the change¶
[code] 
    002003 (42S02): SQL compilation error:
    Object 'DB.SC.ALLOWED_ROLES' does not exist or not authorized.
    
[/code]

#### Redacted error message displayed to some users after the change¶
[code] 
    002003 (42S02): Error in secure object
    
[/code]

Ref: 1858
