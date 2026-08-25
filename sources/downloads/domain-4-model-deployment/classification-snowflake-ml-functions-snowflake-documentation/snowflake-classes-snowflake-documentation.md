---
title: "Snowflake classes | Snowflake Documentation"
source: https://docs.snowflake.com/sql-reference/snowflake-db-classes
cert_domain: domain-4-model-deployment
crawl_depth: 1
crawled: 2026-08-23
---

# Snowflake classes¶

The SNOWFLAKE database also includes Classes provided by Snowflake.

## Concepts¶

A _Class_ is similar to a class in object oriented programming and serves as a blueprint for creating instances. An _Instance_ is an object created from a Class. Classes and instances are schema-level objects in Snowflake. You can think of a class as an extensible Snowflake object type and an instance as a Snowflake object.

A class provides a public API through stored procedures and functions. Collectively they are referred to as _class methods_. A class also provides _class roles_ that enable fine-grained privileges on class methods. In addition to its public API, a class includes private state and private procedures and functions, similar to private properties and methods in object oriented programming. The implementation of a class can evolve over time through new _class versions_. Instances are upgraded to the latest class version automatically by Snowflake.

For example, Snowflake provides the [ANOMALY_DETECTION class](/sql-reference/classes/anomaly_detection) in the SNOWFLAKE.ML schema. You can create an instance of a class using a CREATE command just as you would create an object of a specific object type.

The example below creates an instance of a class and calls an instance method.

  1. Update your search path to include `SNOWFLAKE.ML`:
[code] ALTER SESSION SET SEARCH_PATH = '$current, $public, snowflake.ml';
         
[/code]

  2. Create an instance of ANOMALY_DETECTION class:
[code] CREATE ANOMALY_DETECTION mydatabase.myschema.my_anomaly_detector(...);
         
[/code]

  3. After you create an instance of the ANOMALY_DETECTION class, you can call instance methods:
[code] mydatabase.myschema.my_anomaly_detector!DETECT_ANOMALIES(...);
         
[/code]




Note

Currently, classes are only provided by Snowflake and cannot be created by users.

## List available classes¶

You can find available classes and learn more about each class using SHOW commands. These commands allow you to:

  * Find all available classes in the SNOWFLAKE database.
  * List class methods.
  * List class roles.



### Find all classes¶

List all the available Snowflake classes by executing the [SHOW CLASSES](/sql-reference/sql/show-classes) command:
[code] 
    SHOW CLASSES IN DATABASE SNOWFLAKE;
    
[/code]

The results of this statement include the database and schema name for each class.

### Update your search path¶

Classes are objects in a schema in the SNOWFLAKE database. You must use the fully qualified class name (for example, SNOWFLAKE.ML.ANOMALY_DETECTION) to execute the SQL commands that follow in this topic. Alternatively, you can update the [search path](/sql-reference/name-resolution#label-object-name-resolution-search-path) to include the database and schema for a class, then refer to the class by its unqualified name (for example, ANOMALY_DETECTION).

Note

If you update the search path for a particular class, functions that have the same name but that are part of a different class will no longer be accessible. For example, if you add `SNOWFLAKE.CORTEX` to your search path, the string function [TRANSLATE](/sql-reference/functions/translate) won’t be accessible since the [SNOWFLAKE.CORTEX.TRANSLATE](/sql-reference/functions/translate-snowflake-cortex) function exists.

You can modify the search path using ALTER SESSION, ALTER USER, or ALTER ACCOUNT.

Command| Notes  
---|---  
[ALTER SESSION](/sql-reference/sql/alter-session)| Modifies the search path for the current session only. You can modify your own search path at the session level. A session-level change overrides the account-level or user-level setting.  
[ALTER USER](/sql-reference/sql/alter-user)| Modifies the search path persistently for the current or specified user. You can modify your own search path at the user level. An administrator can modify another user’s search path. A user-level change overrides the account-level or session-level setting.  
[ALTER ACCOUNT](/sql-reference/sql/alter-account)| Modifies the search path persistently for all users in the account. An administrator must modify the search path at the account level.  
  
  1. Execute the following statement and copy your current search path from the `value` column:
[code] SHOW PARAMETERS LIKE 'search_path';
         
[/code]

  2. Update your search path.

Note

The examples below use the default search path, `$current, $public`. If your search path in the `value` column from the previous step does not match the default value, edit the example statements below to include your actual search path.

For example, to add SNOWFLAKE.ML to your search path for your current session, execute the following statement:
[code] ALTER SESSION SET SEARCH_PATH = '$current, $public, SNOWFLAKE.ML';
         
[/code]

To add SNOWFLAKE.ML to your own search path at the user level, execute the following statement:
[code] ALTER USER SET SEARCH_PATH = '$current, $public, SNOWFLAKE.ML';
         
[/code]

A user with the ACCOUNTADMIN role can update the search path for the account by executing the following statement:
[code] ALTER ACCOUNT SET SEARCH_PATH = '$current, $public, SNOWFLAKE.ML';
         
[/code]




For more information on how Snowflake resolves names, see [Object name resolution](/sql-reference/name-resolution).

### Class methods¶

A class provides a public API through stored procedures and functions. Collectively they are referred to as class _methods_. To list all the methods for a class, including the arguments required for each method, execute the [SHOW FUNCTIONS IN CLASS](/sql-reference/sql/show-functions) and [SHOW PROCEDURES IN CLASS](/sql-reference/sql/show-procedures) commands. A class might include multiple methods with the same name but different signatures (that is to say, a different number of arguments or argument data types).

Note

The example statements in this topic use the non-qualified class name ANOMALY_DETECTION. If you have not updated your search path to include SNOWFLAKE.ML, use the fully qualified name for the SNOWFLAKE.ML.ANOMALY_DETECTION class.

For example, to list all the functions available in the SNOWFLAKE.ML.ANOMALY_DETECTION class, execute the following statement:
[code] 
    SHOW FUNCTIONS IN CLASS ANOMALY_DETECTION;
    
[/code]
[code] 
    +-----------------------+-------------------+-------------------+--------------------------------------------------------------------------+--------------+----------+
    | name                  | min_num_arguments | max_num_arguments | arguments                                                                | descriptions | language |
    |-----------------------+-------------------+-------------------+--------------------------------------------------------------------------+--------------+----------|
    | _DETECT_ANOMALIES_1_1 |                 5 |                 5 | (MODEL BINARY, TS TIMESTAMP_NTZ, Y FLOAT, FEATURES ARRAY, CONFIG OBJECT) | NULL         | Python   |
    | _FIT                  |                 3 |                 3 | (TS TIMESTAMP_NTZ, Y FLOAT, FEATURES ARRAY)                              | NULL         | Python   |
    | _FIT                  |                 4 |                 4 | (TS TIMESTAMP_NTZ, Y FLOAT, LABEL BOOLEAN, FEATURES ARRAY)               | NULL         | Python   |
    +-----------------------+-------------------+-------------------+--------------------------------------------------------------------------+--------------+----------+
    
[/code]

To list all the stored procedures in the SNOWFLAKE.ML.ANOMALY_DETECTION class, execute the following statement:
[code] 
    SHOW PROCEDURES IN CLASS ANOMALY_DETECTION;
    
[/code]

The results below include the stored procedures in the class for which the current role in the session has been granted access privileges:
[code] 
    +---------------------------------+-------------------+-------------------+------------------------------------------------------------------------------------------------------------------------------------------+--------------+------------+
    | name                            | min_num_arguments | max_num_arguments | arguments                                                                                                                                | descriptions | language   |
    |---------------------------------+-------------------+-------------------+------------------------------------------------------------------------------------------------------------------------------------------+--------------+------------|
    | __CONSTRUCT                     |                 4 |                 4 | (INPUT_DATA VARCHAR, TIMESTAMP_COLNAME VARCHAR, TARGET_COLNAME VARCHAR, LABEL_COLNAME VARCHAR)                                           | NULL         | Javascript |
    | __CONSTRUCT                     |                 5 |                 5 | (INPUT_DATA VARCHAR, SERIES_COLNAME VARCHAR, TIMESTAMP_COLNAME VARCHAR, TARGET_COLNAME VARCHAR, LABEL_COLNAME VARCHAR)                   | NULL         | Javascript |
    | DETECT_ANOMALIES                |                 4 |                 4 | (INPUT_DATA VARCHAR, SERIES_COLNAME VARCHAR, TIMESTAMP_COLNAME VARCHAR, TARGET_COLNAME VARCHAR)                                          | NULL         | SQL        |
    | DETECT_ANOMALIES                |                 5 |                 5 | (INPUT_DATA VARCHAR, SERIES_COLNAME VARCHAR, TIMESTAMP_COLNAME VARCHAR, TARGET_COLNAME VARCHAR, CONFIG_OBJECT OBJECT)                    | NULL         | SQL        |
    | DETECT_ANOMALIES                |                 3 |                 3 | (INPUT_DATA VARCHAR, TIMESTAMP_COLNAME VARCHAR, TARGET_COLNAME VARCHAR)                                                                  | NULL         | SQL        |
    | DETECT_ANOMALIES                |                 4 |                 4 | (INPUT_DATA VARCHAR, TIMESTAMP_COLNAME VARCHAR, TARGET_COLNAME VARCHAR, CONFIG_OBJECT OBJECT)                                            | NULL         | SQL        |
    | EXPLAIN_FEATURE_IMPORTANCE      |                 0 |                 0 | ()                                                                                                                                       | NULL         | SQL        |
    | _CONSTRUCTFEATUREINPUT          |                 6 |                 6 | (INPUT_REF VARCHAR, SERIES_COLNAME VARCHAR, TIMESTAMP_COLNAME VARCHAR, TARGET_COLNAME VARCHAR, LABEL_COLNAME VARCHAR, REF_ALIAS VARCHAR) | NULL         | Javascript |
    | _CONSTRUCTINFERENCEFUNCTIONNAME |                 0 |                 0 | ()                                                                                                                                       | NULL         | SQL        |
    | _CONSTRUCTINFERENCERESULTAPI    |                 0 |                 0 | ()                                                                                                                                       | NULL         | SQL        |
    | _SETTRAININGINFO                |                 0 |                 0 | ()                                                                                                                                       | NULL         | SQL        |
    +---------------------------------+-------------------+-------------------+------------------------------------------------------------------------------------------------------------------------------------------+--------------+------------+
    
[/code]

### Class roles¶

A class might have one or more roles that are granted the USAGE privilege on some or all class methods. You can list the available roles in a class using the [SHOW ROLES IN CLASS](/sql-reference/sql/show-roles) command.

List all the roles in the SNOWFLAKE.ML.ANOMALY_DETECTION class:
[code] 
    SHOW ROLES IN CLASS ANOMALY_DETECTION;
    
[/code]
[code] 
    +-------------------------------+------+---------+
    | created_on                    | name | comment |
    |-------------------------------+------+---------|
    | 2023-06-06 01:06:42.808 +0000 | USER | NULL    |
    +-------------------------------+------+---------+
    
[/code]

#### Instance roles¶

Roles are defined in the class and instantiated in the instance as an _instance role_. An instance role can be granted to a role in your account to enable access to instance methods.

For example, if you have an ANOMALY_DETECTION instance `my_anomaly_detector` in schema `my_db.my_schema`, you can view the privileges granted to the instance role USER using the following statement:
[code] 
    SHOW GRANTS TO SNOWFLAKE.ML.ANOMALY_DETECTION ROLE my_db.my_schema.my_anomaly_detector!USER;
    
[/code]

To grant the instance role to role `my_role` in your account, execute the following statement:
[code] 
    GRANT SNOWFLAKE.ML.ANOMALY_DETECTION ROLE my_db.my_schema.my_anomaly_detector!USER
      TO ROLE my_role;
    
[/code]

The above statement enables the role `my_role` to execute methods of the ANOMALY_DETECTOR instance `my_anomaly_detector`.

Note

The role `my_role` must also have the USAGE privilege on database `my_db` and schema `my_schema`. Role `my_role` must also have the appropriate privileges on objects passed to instance methods.

## Grant the privilege to create class instances¶

In order to create an instance of a class, a role must be granted the CREATE _< class_name>_ privilege.

For example, to enable the `ml_admin` role to create SNOWFLAKE.ML.ANOMALY_DETECTION instances in the `mydb.myschema` schema, execute the following statement:
[code] 
    GRANT CREATE ANOMALY_DETECTION ON SCHEMA mydb.myschema TO ROLE ml_admin;
    
[/code]

## Create an instance¶

You can create an instance of a class using the CREATE _< object>_ command and the class constructor method.

Note

Instance names in a schema must be unique irrespective of the class they were created from. For example, if you have an instance of the [BUDGET (SNOWFLAKE.CORE)](/sql-reference/classes/budget) class named `foo`, you can’t create an instance of the [ANOMALY_DETECTION (SNOWFLAKE.ML)](/sql-reference/classes/anomaly_detection) class named `foo` in the same schema.

For example, to create an anomaly detector `my_anomaly_detector` instance, execute the following statement:
[code] 
    CREATE ANOMALY_DETECTION my_anomaly_detector(
      INPUT_DATA => SYSTEM$REFERENCE('VIEW', 'my_view'),
      TIMESTAMP_COLUMN => 'my_timestamp_column'
      TARGET_COLNAME => 'my_target_column',
      LABEL_COLNAME => ''
    );
    
[/code]

## Use an instance¶

After you create an instance of a class, you can call the instance methods that the class provides. Calling a method requires the exclamation point (`!`) character. The `!` character is used to dereference the instance.

For example, to call the DETECT_ANOMALIES method of the anomaly detector `my_anomaly_detector`, execute the following statement:
[code] 
    CALL my_anomaly_detector!DETECT_ANOMALIES(
      INPUT_DATA => SYSTEM$REFERENCE('VIEW', 'my_view'),
      TIMESTAMP_COLNAME =>'my_timestamp_column',
      TARGET_COLNAME => 'my_target_column'
    );
    
[/code]

## Selecting columns from SQL class instance methods that return tabular data¶

Some methods return tabular data (for example, methods in the [ANOMALY_DETECTION](/sql-reference/classes/anomaly_detection) and [FORECAST](/sql-reference/classes/forecast) classes). To select and manipulate this tabular data, you can call these methods in the [FROM](/sql-reference/constructs/from) clause of a SELECT statement.

When calling the method, omit the [CALL](/sql-reference/sql/call) command. Instead, put the call in parentheses, preceded by the TABLE keyword:
[code] 
    SELECT ... FROM TABLE( <method_name>( <arg> [ , ... <arg> ] ) );
    
[/code]

For example, to select the `ts`, `forecast`, and `is_anomaly` columns from the tabular data returned by the [DETECT_ANOMALIES](/sql-reference/classes/anomaly-detection/methods/detect_anomalies) method of the anomaly detector `my_anomaly_detector`:
[code] 
    SELECT ts, forecast, is_anomaly FROM TABLE(
      my_anomaly_detector!DETECT_ANOMALIES(
        INPUT_DATA => TABLE('my_view'),
        TIMESTAMP_COLNAME =>'my_timestamp_column',
        TARGET_COLNAME => 'my_target_column'
      )
    );
    
[/code]

If you pass in a reference to a query, the query cannot refer to any [common table expressions](/user-guide/queries-cte) defined outside of the reference. For example, executing the following statement results in an error because the query reference refers to `my_data`, which is defined in the outer [WITH](/sql-reference/constructs/with) clause:
[code] 
    WITH my_data AS (
      SELECT * FROM my_view
    )
    SELECT ts, forecast FROM TABLE(
      my_anomaly_detector!DETECT_ANOMALIES(
        INPUT_DATA => TABLE('SELECT * FROM my_data'),
        TIMESTAMP_COLNAME =>'my_timestamp_column',
        TARGET_COLNAME => 'my_target_column'
      )
    );
    
[/code]

To work around this limitation, move the WITH clause inside the query reference:
[code] 
    SELECT ts, forecast FROM TABLE(
      my_anomaly_detector!DETECT_ANOMALIES(
        INPUT_DATA => TABLE('
          WITH my_data AS (
            SELECT * FROM my_view
          )
          SELECT * FROM my_data
        '),
        TIMESTAMP_COLNAME =>'my_timestamp_column',
        TARGET_COLNAME => 'my_target_column'
      )
    );
    
[/code]

## Available classes¶

For a list of available Snowflake classes, see [SQL class reference](/sql-reference-classes).

## Limitations¶

[Replication](/user-guide/account-replication-intro) is supported only for instances of the [CUSTOM_CLASSIFIER](/sql-reference/classes/custom_classifier) class.
