---
title: "DESCRIBE MASKING POLICY | Snowflake Documentation"
source: https://docs.snowflake.com/sql-reference/sql/desc-masking-policy
cert_domain: domain-3-model-development
crawl_depth: 1
crawled: 2026-08-23
---

# DESCRIBE MASKING POLICY¶

Describes the details about a masking policy, including the creation date, name, data type, and SQL expression.

DESCRIBE can be abbreviated to DESC.

See also:
    

[Masking policy DDL](/user-guide/security-column-intro#label-security-column-intro-ddl)

## Syntax¶
[code] 
    DESC[RIBE] MASKING POLICY <name>
    
[/code]

## Parameters¶

`_name_`
    

Identifier for the masking policy; must be unique for your account.

The identifier value must start with an alphabetic character and cannot contain spaces or special characters unless the entire identifier string is enclosed in double quotes (e.g. `"My object"`). Identifiers enclosed in double quotes are also case-sensitive.

For more details, see [Identifier requirements](/sql-reference/identifiers-syntax).

## Access control requirements¶

A [role](/user-guide/security-access-control-overview#label-access-control-overview-roles) used to execute this SQL command must have at least one of the following [privileges](/user-guide/security-access-control-overview#label-access-control-overview-privileges) at a minimum:

Privilege| Object| Notes  
---|---|---  
APPLY MASKING POLICY| Account|   
APPLY| Masking policy|   
OWNERSHIP| Masking policy| OWNERSHIP is a special privilege on an object that is automatically granted to the role that created the object, but can also be transferred using the [GRANT OWNERSHIP](/sql-reference/sql/grant-ownership) command to a different role by the owning role (or any role with the MANAGE GRANTS privilege).  
  
Operating on an object in a schema requires at least one privilege on the parent database and at least one privilege on the parent schema.

For instructions on creating a custom role with a specified set of privileges, see [Creating custom roles](/user-guide/security-access-control-configure#label-security-custom-role).

For general information about roles and privilege grants for performing SQL actions on [securable objects](/user-guide/security-access-control-overview#label-access-control-securable-objects), see [Overview of Access Control](/user-guide/security-access-control-overview).

For additional details on masking policy DDL and privileges, see [Managing Column-level Security](/user-guide/security-column-intro#label-security-column-manage).

## Usage notes¶

  * To post-process the output of this command, you can use the [pipe operator](/sql-reference/operators-flow) (`->>`) or the [RESULT_SCAN](/sql-reference/functions/result_scan) function. Both constructs treat the output as a result set that you can query.

For example, you can use the pipe operator or RESULT_SCAN function to select specific columns from the SHOW command output or filter the rows.

When you refer to the output columns, use [double-quoted identifiers](/sql-reference/identifiers-syntax#label-delimited-identifier) for the column names. For example, to select the output column `type`, specify `SELECT "type"`.

You must use double-quoted identifiers because the output column names for SHOW commands are in lowercase. The double quotes ensure that the column names in the SELECT list or WHERE clause match the column names in the SHOW command output that was scanned.




## Example¶
[code] 
    DESC MASKING POLICY ssn_mask;
    
[/code]
[code] 
    +-----+------------+---------------+-------------------+-----------------------------------------------------------------------+
    | Row | name       | signature     | return_type       | body                                                                  |
    +-----+------------+---------------+-------------------+-----------------------------------------------------------------------+
    | 1   | SSN_MASK   | (VAL VARCHAR) | VARCHAR(16777216) | case when current_role() in ('ANALYST') then val else '*********' end |
    +-----+------------+---------------+-------------------+-----------------------------------------------------------------------+
    
[/code]
