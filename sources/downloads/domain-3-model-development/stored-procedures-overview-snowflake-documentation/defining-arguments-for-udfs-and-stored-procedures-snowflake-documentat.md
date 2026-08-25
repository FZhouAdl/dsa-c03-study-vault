---
title: "Defining arguments for UDFs and stored procedures | Snowflake Documentation"
source: https://docs.snowflake.com/developer-guide/udf-stored-procedure-arguments
cert_domain: domain-3-model-development
crawl_depth: 1
crawled: 2026-08-23
---

# Defining arguments for UDFs and stored procedures¶

In the [CREATE FUNCTION](/sql-reference/sql/create-function) or [CREATE PROCEDURE](/sql-reference/sql/create-procedure) that you execute to define a function or procedure, you specify arguments that can be passed in. For example:
[code] 
    CREATE FUNCTION my_function(integer_argument INT, varchar_argument VARCHAR)
      ...
    
[/code]
[code] 
    CREATE PROCEDURE my_procedure(boolean_argument BOOLEAN, date_argument DATE)
      ...
    
[/code]

When you call a function or procedure, the argument values are bound to the handler’s arguments. They may be bound based on matching names or by argument position, depending on the language you’re using for the handler.

This topic provides guidelines on specifying the arguments for a function or procedure.

## Limits on the number of input arguments¶

Scalar functions (UDFs) have a limit of 500 input arguments.

## Specify the data types for the arguments¶

Choose the SQL data type that corresponds to the data type of the argument that you are using in the handler code.

For information about how Snowflake maps SQL data types to handler data types, see [Data Type Mappings Between SQL and Handler Languages](/developer-guide/udf-stored-procedure-data-type-mapping).

## Omit the `Session` argument for Java, Python, and Scala procedures¶

In the [CREATE PROCEDURE](/sql-reference/sql/create-procedure) statement for a procedure written in Java, Python, or Scala, do not define the argument for the Snowpark `Session` object.

For example, suppose that your handler code passes in a `Session` object and a `String` object:
[code] 
    public String queryTable(Session session, String tableName) { ... }
    
[/code]

In the CREATE PROCEDURE statement, do not define an argument for the `Session` object. Instead, just define an argument for the input string:
[code] 
    CREATE OR REPLACE PROCEDURE query_table(table_name VARCHAR)
      ...
    
[/code]

`Session` is an implicit argument that you do not specify when calling the procedure. At runtime, when you call your stored procedure, Snowflake creates a `Session` object and passes it to your stored procedure.

## Specify optional arguments¶

You can specify that an argument is optional. For details, see the next sections:

  * Designating an argument as optional
  * Overloading functions and procedures with optional arguments
  * Calling functions and procedures that have optional arguments



### Designating an argument as optional¶

If you want an argument to be optional, use the DEFAULT keyword to specify the default value for the argument. For example:
[code] 
    CREATE OR REPLACE FUNCTION build_string_udf(
        word VARCHAR,
        prefix VARCHAR DEFAULT 'pre-',
        suffix VARCHAR DEFAULT '-post'
      )
      ...
    
[/code]
[code] 
    CREATE OR REPLACE PROCEDURE build_string_proc(
        word VARCHAR,
        prefix VARCHAR DEFAULT 'pre-',
        suffix VARCHAR DEFAULT '-post'
      )
      ...
    
[/code]

For the default value of the argument, you can use an expression. For example:
[code] 
    CREATE OR REPLACE FUNCTION my_date_udf(optional_date_arg DATE DEFAULT CURRENT_DATE())
      ...
    
[/code]

You must specify optional arguments after the required arguments (if any). You cannot specify an optional argument before a required argument.
[code] 
    -- This is not allowed.
    CREATE FUNCTION wrong_order(optional_argument INTEGER DEFAULT 0, required_argument INTEGER)
      ...
    
[/code]

### Overloading functions and procedures with optional arguments¶

If you are [overloading](/developer-guide/udf-stored-procedure-naming-conventions#label-procedure-function-name-overloading) a function or procedure, you cannot use an optional argument to distinguish between different signatures. For example, suppose that you create the following UDF that passes in no arguments:
[code] 
    CREATE FUNCTION my_udf_a()
      ...
    
[/code]

If you attempt to create a UDF with the same name that passes in an optional argument, the CREATE FUNCTION statement fails:
[code] 
    CREATE FUNCTION my_udf_a(optional_arg INTEGER DEFAULT 0)
      ...
    
[/code]
[code] 
    000949 (42723): SQL compilation error:
      Cannot overload FUNCTION 'MY_UDF_A' as it would cause ambiguous FUNCTION overloading.
    
[/code]

As another example, suppose that you create a UDF that passes in a required INTEGER argument:
[code] 
    CREATE FUNCTION my_udf_b(required_arg INTEGER)
      ...
    
[/code]

If you attempt to create a UDF with the same name that passes in a required INTEGER argument and an optional argument, the CREATE FUNCTION statement fails:
[code] 
    CREATE FUNCTION my_udf_b(required_arg INTEGER, optional_arg INTEGER DEFAULT 0)
      ...
    
[/code]
[code] 
    000949 (42723): SQL compilation error:
      Cannot overload FUNCTION 'MY_UDF_B' as it would cause ambiguous FUNCTION overloading.
    
[/code]

This also affects cases in which you use [ALTER FUNCTION … RENAME](/sql-reference/sql/alter-function) or [ALTER PROCEDURE … RENAME](/sql-reference/sql/alter-procedure) to rename a function or procedure. If you want to rename a function or procedure, there cannot be an existing function with the same name and signature. Optional arguments do not distinguish one signature from another.

For example, suppose that you create a UDF named `abc_udf` that passes in a required INTEGER argument:
[code] 
    CREATE FUNCTION abc_udf(required_arg INTEGER)
      ...
    
[/code]

Suppose that you create a UDF with a different name (`def_udf`) that passes in a required INTEGER argument and an optional argument:
[code] 
    CREATE FUNCTION def_udf(required_arg INTEGER, optional_arg INTEGER DEFAULT 0)
      ...
    
[/code]

If you attempt to change the name of `def_udf` to `abc_udf`, an error occurs because there is already a UDF that has the same name and the same types of required arguments:
[code] 
    000949 (42723): SQL compilation error:
      Cannot overload FUNCTION 'ABC_UDF' as it would cause ambiguous FUNCTION overloading.
    
[/code]

### Calling functions and procedures that have optional arguments¶

To call functions and procedures that have optional arguments, see:

  * [Calling a UDF that has optional arguments](/developer-guide/udf/udf-calling-sql#label-call-udf-calling-udf-optional)
  * [Specifying optional arguments](/developer-guide/stored-procedure/stored-procedures-calling#label-call-procedure-optional)
