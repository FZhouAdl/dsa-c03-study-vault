---
title: "ALTER MODEL ... MODIFY VERSION | Snowflake Documentation"
source: https://docs.snowflake.com/sql-reference/sql/alter-model-modify-version
cert_domain: domain-4-model-deployment
crawl_depth: 1
crawled: 2026-08-23
---

# ALTER MODEL … MODIFY VERSION¶

Modifies a version of a model, changing the version’s comment or metadata.

See also:
    

[ALTER MODEL … ADD VERSION](/sql-reference/sql/alter-model-add-version), [ALTER MODEL … DROP VERSION](/sql-reference/sql/alter-model-drop-version)

## Syntax¶
[code] 
    ALTER MODEL [ IF EXISTS ] <name> MODIFY VERSION <version_or_alias_name> SET
      [ COMMENT = '<string_literal>' ]
      [ METADATA = '<json_metadata>']
    
[/code]

## Parameters¶

`_name_`
    

Specifies the identifier of the model.

If the identifier contains spaces or special characters, the entire string must be enclosed in double quotes. Identifiers enclosed in double quotes are also case-sensitive.

For more information, see [Identifier requirements](/sql-reference/identifiers-syntax).

`_version_or_alias_name_`
    

Specifies the identifier of the version, either its version name or its alias. Version names that contain spaces or that are case sensitive must be enclosed in double quotes. For information on identifier syntax, see [Identifier requirements](/sql-reference/identifiers-syntax).

Aliases must be valid identifiers without double quotes.

See Usage Notes for more information on aliases.

`SET ...`
    

Specifies one or more model version properties to be set.

`COMMENT = '_string_literal_ '`
    

Sets the comment of the version.

`METADATA = '_json_metadata_ '`
    

Sets the metadata of the version. Metadata is a JSON object that stores key-value pairs of your choosing.

## Usage notes¶

Aliases are alternative names for model versions. In addition to aliases you create, the following three system aliases are available.

  * `DEFAULT` refers to the default version of the model.
  * `FIRST` refers to the oldest version of the model by creation time.
  * `LAST` refers to the newest version of the model by creation time.
