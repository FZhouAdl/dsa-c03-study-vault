---
title: "LIST | Snowflake Documentation"
source: https://docs.snowflake.com/sql-reference/sql/list
cert_domain: domain-4-model-deployment
crawl_depth: 1
crawled: 2026-08-23
---

# LIST¶

Returns a list of files from one of the following Snowflake storage features:

  * Stage

    * Named internal
    * Named external
    * For a specified table
    * For the current user
  * [Git repository clone in Snowflake](/developer-guide/git/git-overview)




LIST can be abbreviated to LS.

See also:
    

[REMOVE](/sql-reference/sql/remove), [PUT](/sql-reference/sql/put), [COPY INTO <table>](/sql-reference/sql/copy-into-table), [COPY INTO <location>](/sql-reference/sql/copy-into-location), [GET](/sql-reference/sql/get)

## Syntax¶

The syntax differs depending on whether you’re listing files in a stage or a Git repository clone.

### For a stage¶
[code] 
    LIST { internalStage | externalStage } [ PATTERN = '<regex_pattern>' ]
    
[/code]

Where:

> 
[code]
>     internalStage ::=
>         @[<namespace>.]<int_stage_name>[/<path>]
>       | @[<namespace>.]%<table_name>[/<path>]
>       | @~[/<path>]
>     
[/code]
[code]
>     externalStage ::=
>       @[<namespace>.]<ext_stage_name>[/<path>]
>     
[/code]

### For a Git repository clone¶
[code] 
    LIST repositoryClone [ PATTERN = '<regex_pattern>' ]
    
[/code]

Where:

> 
[code]
>     repositoryClone ::=
>       @[<namespace>.]<repository_clone>/<path>
>     
[/code]

## Required parameters¶

### For a stage¶

`internalStage | externalStage`
    

Specifies the location where the data files are staged:

> `@[_namespace_.]_int_stage_name_[/_path_]`| Files are in the specified named internal stage.  
> ---|---  
> `@[_namespace_.]_ext_stage_name_[/_path_]`| Files are in the specified named external stage.  
> `@[_namespace_.]%_table_name_[/_path_]`| Files are in the stage for the specified table.  
> `@~[/_path_]`| Files are in the stage for the current user.  
>   
> Where:
> 
>   * `<namespace>` is the database and/or schema in which the named stage or table resides. It is **optional** if a database and schema are currently in use within the session; otherwise, it is required.
> 
>   * `<path>` is an optional case-sensitive path for files in the cloud storage location (i.e. files have names that begin with a common string) that limits access to a set of files. Paths are alternatively called _prefixes_ or _folders_ by different cloud storage services.
> 
> 

> 
> Note
> 
> If the stage name or path includes spaces or special characters, it must be enclosed in single quotes (e.g. `'@"my stage"'` for a stage named `"my stage"`).
> 
> Specifying a path provides a scope for the LIST command, potentially reducing the amount of time required to run the command.

### For a Git repository clone¶

`repositoryClone`
    

Specifies the name of the [repository clone](/sql-reference/sql/create-git-repository) and the branch, tag, or commit for which to list files.

`@[_namespace_.]_repository_clone_ /_path_`

When listing files from a Git repository clone, the `path` is required and must begin with one of the following:

> `branches/_branch_name_`|  List files from the specified branch.  
> ---|---  
> `tags/_tag_name_`|  List files from the specified tag.  
> `commits/_commit_hash_`|  List files from the commit specified by the commit hash.  
>   
> If the repository clone name or path includes spaces or special characters, it must be enclosed in single quotes (for example, `'@"my repository"'` for a repository named `"my repository"`).

## Optional parameters¶

`PATTERN = '_regex_pattern_ '`
    

Specifies a regular expression pattern for filtering files from the output. The command lists all files in the specified `_path_` and applies the regular expression pattern on each of the files found.

## Usage notes¶

  * To run this command with an external stage that uses a storage integration, you must use a role that has or inherits the USAGE privilege on the storage integration.

For more information, see [Stage privileges](/user-guide/security-access-control-privileges#label-access-control-privileges-stage).

  * In contrast to named stages, table and user stages are not first-class database objects; rather, they are implicit stages associated with the table/user. As such, they have no grantable privileges of their own:
    * You can always list files in your user stage (i.e. no privileges are required).
    * To list files in a table stage, you must use a role that has the OWNERSHIP privilege on the table.
    * PATTERN supports the [Java Pattern class](https://docs.oracle.com/javase/8/docs/api/java/util/regex/Pattern.html) syntax.



## Output¶

The command returns columns in the following tables. Column values differ depending on whether you’re using LIST with a stage or Git repository clone.

### For a stage¶

Column| Data type| Description  
---|---|---  
name| VARCHAR| Name of the staged file  
size| NUMBER| Size of the file compressed (in bytes)  
md5| VARCHAR| The MD5 column stores an MD5 hash of the contents of the staged data file.For internal stages with default encryption (SNOWFLAKE_FULL), during upload the source file is encrypted with a random key, and its resulting MD5 digest will always differ from the original local file.Amazon S3 stages report the value via the S3 eTag field, which might not be an MD5 hash of the file contents.For Google Cloud stages that use a customer-managed encryption key (CMEK), md5 is expected to be NULL.For more information, see [Customer-managed encryption keys](https://cloud.google.com/storage/docs/encryption/customer-managed-keys).  
sha1| VARCHAR| Not used  
last_modified| VARCHAR| Timestamp when the file was last updated in the stage  
  
### For a Git repository clone¶

Column| Data type| Description  
---|---|---  
name| VARCHAR| Full file path with extension  
size| NUMBER| Size of the file compressed (in bytes)  
md5| VARCHAR| Not used  
sha1| VARCHAR| A unique identifier generated by applying the SHA-1 hashing algorithm to the file’s contents. It is used by Git to track and reference the exact version of a file in the repository, and can be used to detect changes in the file’s content.  
last_modified| VARCHAR| Timestamp of the commit associated with the listed files. This does not necessarily indicate when the file content was last changed.  
  
## Examples¶

### For a stage¶

List all the files in the stage for the `mytable` table:
[code] 
    LIST @%mytable;
    
[/code]

List all the files in the `path1` path of the `mystage` named stage:
[code] 
    LIST @mystage/path1;
    
[/code]

List the files that match a regular expression (i.e. all file names containing the string `data_0`) in the stage for the `mytable` table:
[code] 
    LIST @%mytable PATTERN='.*data_0.*';
    
[/code]

List the files in the `/analysis/` path of the `my_csv_stage` named stage that match a regular expression (i.e. all file names containing the string `data_0`):
[code] 
    LIST @my_csv_stage/analysis/ PATTERN='.*data_0.*';
    
[/code]

Use the abbreviated form of the command to list all the files in the stage for the current user:
[code] 
    LS @~;
    
[/code]

### For a Git repository clone¶

For examples, see [View a list of repository files](/developer-guide/git/git-operations#label-git-integration-viewing-repository-files).
