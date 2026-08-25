---
title: "Table functions | Snowflake Documentation"
source: https://docs.snowflake.com/sql-reference/functions-table
cert_domain: domain-2-data-preparation-and-feature-engineering
crawl_depth: 1
crawled: 2026-08-23
---

# Table functions¶

A table function returns a set of rows for each input row. The returned set can contain zero, one, or more rows. Each row can contain one or more columns.

Table functions are sometimes called “tabular functions”.

## What are table functions?¶

Table functions are typically used when a function returns multiple rows for each individual input.

Each time that a table function is called, it can return a different number of rows. For example, a function `record_high_temperatures_for_date()`, which returns a list of record high temperatures for a specified date, might return 0 rows on April 10, 1 row on June 10, and 40 rows on August 20.

### Simple examples of table functions¶

The following are appropriate as table functions:

  * A function that accepts an account number and a date, and returns all charges billed to that account on that date. (More than one charge might have been billed on a particular date.)
  * A function that accepts a user ID and returns the database roles assigned to that user. (A user might have multiple roles, including “sysadmin” and “useradmin”.)



### Functions in which each output row depends upon multiple input rows¶

Table functions can be grouped into two categories based on the number of input rows that affect each output row:

  * 1-to-N
  * M-to-N



The functions described earlier are 1-to-N table functions: each output row depends upon only one input row. For example, a function `record_high_temperatures_for_date()` might produce multiple output rows (one for each city that hit a record on that date). Each output row for a specific input date depends only on that date; each output row is independent of the rows for every other date.

Snowflake also supports M-to-N table functions: each output row can depend upon multiple input rows. For example, if a function generates a moving average of stock prices, that function uses stock prices from multiple input rows (multiple dates) to generate each output row.

More generally, in an M-to-N function, a group of M input rows produces a group of N output rows. M can be one or more rows. N can be zero, one, or more rows.

For example, in a 10-day moving average, M is 10. N is 1 because each group of 10 input rows produces one average price.

### Built-in table functions vs user-defined table functions¶

Snowflake provides hundreds of built-in functions, many of which are table functions. Built-in table functions are listed in System-Defined Table Functions.

Users can also write their own functions, called user-defined functions or “UDFs”. Some UDFs are scalar; some are tabular. User-defined table functions are called “UDTFs”. For information about UDFs (including UDTFs), see [User-defined functions overview](/developer-guide/udf/udf-overview).

Built-in table functions and user-defined table functions generally follow the same rules; for example, they are called the same way from SQL statements.

## Using a table function¶

### Using a table function in the FROM clause¶

A table contains a set of rows. Similarly, a table function returns a set of rows. Both tables and table functions are used in contexts that expect a set of rows. Specifically, table functions are used in the [FROM](/sql-reference/constructs/from) clause of a SQL statement.

To help the SQL compiler recognize a table function as a source of rows, Snowflake requires that the table function call be wrapped by the `TABLE()` keyword.

For example, the following statement calls a table function named `record_high_temperatures_for_date()`, which takes a DATE value as an argument:

> 
[code]
>     SELECT city_name, temperature
>         FROM TABLE(record_high_temperatures_for_date('2021-06-27'::DATE))
>         ORDER BY city_name;
>     
[/code]

For more information about the syntax of `TABLE()`, see [Table literals](/sql-reference/literals-table).

Table functions, like functions in general, can accept zero, one, or multiple input arguments in each invocation. Each argument must be a scalar expression.

For more details about the syntax of table function calls, see Syntax (in this topic).

### Using a table as input to a table function¶

The argument to a table function can be a literal or an expression, such as a column of a table. For example, the SELECT statement below passes values from a table as arguments to a table function:
[code] 
    CREATE OR REPLACE table dates_of_interest (event_date DATE);
    INSERT INTO dates_of_interest (event_date) VALUES
        ('2021-06-21'::DATE),
        ('2022-06-21'::DATE);
    
    CREATE OR REPLACE FUNCTION record_high_temperatures_for_date(d DATE)
        RETURNS TABLE (event_date DATE, city VARCHAR, temperature NUMBER)
        as
        $$
        SELECT d, 'New York', 65.0
        UNION ALL
        SELECT d, 'Los Angeles', 69.0
        $$;
    
[/code]
[code] 
    SELECT
            doi.event_date as "Date", 
            record_temperatures.city,
            record_temperatures.temperature
        FROM dates_of_interest AS doi,
             TABLE(record_high_temperatures_for_date(doi.event_date)) AS record_temperatures
          ORDER BY doi.event_date, city;
    +------------+-------------+-------------+
    | Date       | CITY        | TEMPERATURE |
    |------------+-------------+-------------|
    | 2021-06-21 | Los Angeles |          69 |
    | 2021-06-21 | New York    |          65 |
    | 2022-06-21 | Los Angeles |          69 |
    | 2022-06-21 | New York    |          65 |
    +------------+-------------+-------------+
    
[/code]

The arguments to a table function can come from other table-like sources, including views and other table functions.

## List of system-defined table functions¶

Snowflake provides the following system-defined (i.e. built-in) table functions:

Sub-category| Function| Notes  
---|---|---  
Data Loading| [INFER_SCHEMA](/sql-reference/functions/infer_schema)| For more information, see [Load data into Snowflake](/guides-overview-loading-data).  
| [VALIDATE](/sql-reference/functions/validate)|   
Data Generation| [GENERATOR](/sql-reference/functions/generator)|   
Data Conversion| [SPLIT_TO_TABLE](/sql-reference/functions/split_to_table)|   
| [STRTOK_SPLIT_TO_TABLE](/sql-reference/functions/strtok_split_to_table)|   
Differential Privacy| [CUMULATIVE_PRIVACY_LOSSES](/sql-reference/functions/cumulative_privacy_losses)|   
Object Modeling| [GET_OBJECT_REFERENCES](/sql-reference/functions/get_object_references)|   
Parameterized Queries| [TO_QUERY](/sql-reference/functions/to_query)|   
Semi-structured Queries| [FLATTEN](/sql-reference/functions/flatten)| For more information, see [Querying Semi-structured Data](/user-guide/querying-semistructured).  
Query Results| [RESULT_SCAN](/sql-reference/functions/result_scan)| Can be used to perform SQL operations on the output from another SQL operation (e.g. SHOW).  
Query Profile| [GET_QUERY_OPERATOR_STATS](/sql-reference/functions/get_query_operator_stats)|   
Historical & Usage Information| | Includes:

  * [Snowflake Information Schema](/sql-reference/info-schema)
  * [Account Usage](/sql-reference/account-usage)
  * [LOCAL schema](/sql-reference/local)

  
User Login| [LOGIN_HISTORY , LOGIN_HISTORY_BY_USER](/sql-reference/functions/login_history)|   
Queries| [QUERY_HISTORY , QUERY_HISTORY_BY_*](/sql-reference/functions/query_history)|   
| [QUERY_ACCELERATION_HISTORY](/sql-reference/functions/query_acceleration_history)| TO BE DEPRECATED - Refer to [QUERY_ACCELERATION_HISTORY view](/sql-reference/account-usage/query_acceleration_history). For more information, see [Using the Query Acceleration Service (QAS)](/user-guide/query-acceleration-service).  
Warehouse & Storage Usage| [DATABASE_STORAGE_USAGE_HISTORY](/sql-reference/functions/database_storage_usage_history)| TO BE DEPRECATED - Refer to [DATABASE_STORAGE_USAGE_HISTORY view](/sql-reference/account-usage/database_storage_usage_history).  
| [WAREHOUSE_LOAD_HISTORY](/sql-reference/functions/warehouse_load_history)|   
| [WAREHOUSE_METERING_HISTORY](/sql-reference/functions/warehouse_metering_history)| TO BE DEPRECATED - Refer to [WAREHOUSE_METERING_HISTORY view](/sql-reference/account-usage/warehouse_metering_history).  
| [STAGE_STORAGE_USAGE_HISTORY](/sql-reference/functions/stage_storage_usage_history)| TO BE DEPRECATED - Refer to [STAGE_STORAGE_USAGE_HISTORY view](/sql-reference/account-usage/stage_storage_usage_history).  
| [ESTIMATE_HYBRID_TABLE_STORAGE_USAGE](/sql-reference/functions/estimate_hybrid_table_storage_usage)| Returns a near-real-time estimate of the total storage used by a hybrid table.  
Storage Lifecycle Policies| [STORAGE_LIFECYCLE_POLICY_HISTORY](/sql-reference/functions/storage_lifecycle_policy_history)| Information Schema table function. For more information, see [Storage lifecycle policies](/user-guide/storage-management/storage-lifecycle-policies).  
Column-level & Row-level Security| [POLICY_REFERENCES](/sql-reference/functions/policy_references)|   
Object Tagging| [TAG_REFERENCES](/sql-reference/functions/tag_references)| Information Schema table function.  
| [TAG_REFERENCES_ALL_COLUMNS](/sql-reference/functions/tag_references_all_columns)| Information Schema table function.  
| [TAG_REFERENCES_WITH_LINEAGE](/sql-reference/functions/tag_references_with_lineage)| Account Usage table function.  
Account Replication| [REPLICATION_GROUP_DANGLING_REFERENCES](/sql-reference/functions/replication_group_dangling_references)| For more information, see [Introduction to replication and failover across multiple accounts](/user-guide/account-replication-intro)  
| [REPLICATION_GROUP_REFRESH_HISTORY, REPLICATION_GROUP_REFRESH_HISTORY_ALL](/sql-reference/functions/replication_group_refresh_history)|   
| [REPLICATION_GROUP_REFRESH_PROGRESS, REPLICATION_GROUP_REFRESH_PROGRESS_BY_JOB, REPLICATION_GROUP_REFRESH_PROGRESS_ALL](/sql-reference/functions/replication_group_refresh_progress)|   
| [REPLICATION_GROUP_USAGE_HISTORY](/sql-reference/functions/replication_group_usage_history)| TO BE DEPRECATED - Refer to [REPLICATION_GROUP_USAGE_HISTORY view](/sql-reference/account-usage/replication_group_usage_history).  
Alerts| [ALERT_HISTORY](/sql-reference/functions/alert_history)| For more information, see [Setting up alerts based on data in Snowflake](/user-guide/alerts).  
| [SERVERLESS_ALERT_HISTORY](/sql-reference/functions/serverless_alert_history)| TO BE DEPRECATED - Refer to [SERVERLESS_ALERT_HISTORY view](/sql-reference/account-usage/serverless_alert_history).  
Bind variables| [BIND_VALUES](/sql-reference/functions/bind_values)| For more information, see [Retrieve bind variable values](/sql-reference/bind-variables#label-bind-variables-retrieving-values).  
Database Replication| [DATABASE_REFRESH_HISTORY](/sql-reference/functions/database_refresh_history)| For more information, see [Replicating databases across multiple accounts](/user-guide/db-replication-config).  
| [DATABASE_REFRESH_PROGRESS , DATABASE_REFRESH_PROGRESS_BY_JOB](/sql-reference/functions/database_refresh_progress)|   
| [DATABASE_REPLICATION_USAGE_HISTORY](/sql-reference/functions/database_replication_usage_history)| TO BE DEPRECATED - Refer to [DATABASE_REPLICATION_USAGE_HISTORY view](/sql-reference/account-usage/database_replication_usage_history).  
Data Loading & Transfer| [COPY_HISTORY](/sql-reference/functions/copy_history)|   
| [DATA_TRANSFER_HISTORY](/sql-reference/functions/data_transfer_history)| TO BE DEPRECATED - Refer to [DATA_TRANSFER_HISTORY view](/sql-reference/account-usage/data_transfer_history).  
| [PIPE_USAGE_HISTORY](/sql-reference/functions/pipe_usage_history)| TO BE DEPRECATED - Refer to [PIPE_USAGE_HISTORY view](/sql-reference/account-usage/pipe_usage_history).  
| [STAGE_DIRECTORY_FILE_REGISTRATION_HISTORY](/sql-reference/functions/stage_directory_file_registration_history)|   
| [VALIDATE_PIPE_LOAD](/sql-reference/functions/validate_pipe_load)|   
Data Clustering (within Tables)| [AUTOMATIC_CLUSTERING_HISTORY](/sql-reference/functions/automatic_clustering_history)| TO BE DEPRECATED - Refer to [AUTOMATIC_CLUSTERING_HISTORY view](/sql-reference/account-usage/automatic_clustering_history). For more information, see [Automatic Clustering](/user-guide/tables-auto-reclustering).  
dbt Projects on Snowflake| [DBT_PROJECT_EXECUTION_HISTORY](/sql-reference/functions/dbt_project_execution_history)| For more information, see [dbt Projects on Snowflake](/user-guide/data-engineering/dbt-projects-on-snowflake).  
Dynamic Tables| [DYNAMIC_TABLES](/sql-reference/functions/dynamic_tables)| For more information, see [Create a dynamic table](/user-guide/dynamic-tables/create).  
| [DYNAMIC_TABLE_GRAPH_HISTORY](/sql-reference/functions/dynamic_table_graph_history)|   
| [DYNAMIC_TABLE_REFRESH_HISTORY](/sql-reference/functions/dynamic_table_refresh_history)|   
External Functions| [EXTERNAL_FUNCTIONS_HISTORY](/sql-reference/functions/external_functions_history)| For more information, see [Writing external functions](/sql-reference/external-functions).  
External Tables| [AUTO_REFRESH_REGISTRATION_HISTORY](/sql-reference/functions/auto_refresh_registration_history)| TO BE DEPRECATED - Refer to [CATALOG_LINKED_DATABASE_USAGE_HISTORY view](/sql-reference/account-usage/catalog_linked_database_usage_history). For more information, see [Introduction to external tables](/user-guide/tables-external-intro).  
| [EXTERNAL_TABLE_FILES](/sql-reference/functions/external_table_files)|   
| [EXTERNAL_TABLE_FILE_REGISTRATION_HISTORY](/sql-reference/functions/external_table_registration_history)|   
Iceberg Tables| [ICEBERG_TABLE_FILES](/sql-reference/functions/iceberg_table_files)| Information Schema table function.  
| [ICEBERG_TABLE_SNAPSHOT_REFRESH_HISTORY](/sql-reference/functions/iceberg_table_snapshot_refresh_history)| Information Schema table function.  
Listings| [AVAILABLE_LISTINGS](/sql-reference/functions/available_listings)|   
| [AVAILABLE_LISTING_REFRESH_HISTORY](/sql-reference/functions/available_listing_refresh_history)|   
| [LISTING_REFRESH_HISTORY](/sql-reference/functions/listing_refresh_history)|   
Materialized Views Maintenance| [MATERIALIZED_VIEW_REFRESH_HISTORY](/sql-reference/functions/materialized_view_refresh_history)| TO BE DEPRECATED - Refer to [MATERIALIZED_VIEW_REFRESH_HISTORY view](/sql-reference/account-usage/materialized_view_refresh_history). For more information, see [Working with Materialized Views](/user-guide/views-materialized).  
Machine learning| [ONLINE_FEATURE_TABLE_REFRESH_HISTORY](/sql-reference/functions/online-feature-table-refresh-history)| For more information, see [Feature store commands](/sql-reference/commands-feature-store).  
Notifications| [NOTIFICATION_HISTORY](/sql-reference/functions/notification_history)| For more information, see [Using SYSTEM$SEND_EMAIL to send email notifications](/user-guide/notifications/email-stored-procedures).  
SCIM Maintenance| [REST_EVENT_HISTORY](/sql-reference/functions/rest_event_history)| For more information, see [Auditing SCIM API requests](/user-guide/scim-api-references#label-scim-auditing-rest-api-requests)  
Search Optimization Maintenance| [SEARCH_OPTIMIZATION_HISTORY](/sql-reference/functions/search_optimization_history)| TO BE DEPRECATED - Refer to [SEARCH_OPTIMIZATION_HISTORY view](/sql-reference/account-usage/search_optimization_history). For more information, see [Search optimization service](/user-guide/search-optimization-service).  
Streams| [SYSTEM$STREAM_BACKLOG](/sql-reference/functions/system_stream_backlog)| For more information, see [Introduction to streams](/user-guide/streams-intro).  
Tasks| [COMPLETE_TASK_GRAPHS](/sql-reference/functions/complete_task_graphs)| For more information, see [Introduction to tasks](/user-guide/tasks-intro).  
| [CURRENT_TASK_GRAPHS](/sql-reference/functions/current_task_graphs)|   
| [SERVERLESS_TASK_HISTORY](/sql-reference/functions/serverless_task_history)| TO BE DEPRECATED - Refer to [SERVERLESS_TASK_HISTORY view](/sql-reference/account-usage/serverless_task_history).  
| [TASK_DEPENDENTS](/sql-reference/functions/task_dependents)|   
| [TASK_HISTORY](/sql-reference/functions/task_history)|   
Network rules| [NETWORK_RULE_REFERENCES](/sql-reference/functions/network_rule_references)| Information Schema table function. For details, see [Network rules](/user-guide/network-rules).  
Data Quality| [DATA_METRIC_FUNCTION_EXPECTATIONS](/sql-reference/functions/data_metric_function_expectations)|   
| [DATA_METRIC_FUNCTION_REFERENCES](/sql-reference/functions/data_metric_function_references)|   
| [DATA_QUALITY_MONITORING_EXPECTATION_STATUS](/sql-reference/functions/data_quality_monitoring_expectation_status)|   
| [DATA_QUALITY_MONITORING_RESULTS](/sql-reference/functions/data_quality_monitoring_results)|   
| [SYSTEM$DATA_METRIC_SCAN](/sql-reference/functions/system_data_metric_scan)|   
| [SYSTEM$EVALUATE_DATA_QUALITY_EXPECTATIONS](/sql-reference/functions/system_evaluate_data_quality_expectations)|   
| [SYSTEM$EVALUATE_DATA_QUALITY_EXPECTATIONS_PERSIST_RESULT](/sql-reference/functions/system_evaluate_data_quality_expectations_persist_result)|   
Data Lineage| [GET_LINEAGE (SNOWFLAKE.CORE)](/sql-reference/functions/get_lineage-snowflake-core)| For more information, see [Data Lineage](/user-guide/ui-snowsight-lineage).  
Cortex Search| [CORTEX_SEARCH_DATA_SCAN](/sql-reference/functions/cortex_search_data_scan)| For more information, see [Cortex Search](/user-guide/snowflake-cortex/cortex-search/cortex-search-overview).  
| [CORTEX_SEARCH_REFRESH_HISTORY](/sql-reference/functions/cortex_search_refresh_history)|   
Contacts| [GET_CONTACTS](/sql-reference/functions/get_contacts)|   
Snowpark Container Services| [GET_JOB_HISTORY](/sql-reference/functions/get_job_history)| For more information, see [Snowpark Container Services: Monitoring Services](/developer-guide/snowpark-container-services/monitoring-services).  
| [<service_name>!SPCS_GET_EVENTS](/sql-reference/functions/spcs_get_events)|   
| [<service_name>!SPCS_GET_LOGS](/sql-reference/functions/spcs_get_logs)|   
| [<service_name>!SPCS_GET_METRICS](/sql-reference/functions/spcs_get_metrics)|   
Snowflake Native Apps| [APPLICATION_CALLBACK_HISTORY](/sql-reference/functions/application_callback_history)| For more information, see [Callbacks](/developer-guide/native-apps/callbacks).  
| [APPLICATION_SPECIFICATION_STATUS_HISTORY](/sql-reference/functions/application_specification_status_history)| For more information, see [Use app specifications to request controlled access](/developer-guide/native-apps/requesting-app-specs).  
| [APPLICATION_CONFIGURATION_VALUE_HISTORY](/sql-reference/functions/application_configuration_value_history)| For more information, see [Application configuration](/developer-guide/native-apps/app-configuration).  
Cortex Agents| [GET_AI_RECORD_TRACE (SNOWFLAKE.LOCAL)](/sql-reference/functions/get_ai_record_trace-snowflake-local)| For more information, see [Cortex Agent evaluations](/user-guide/snowflake-cortex/cortex-agents-evaluations) and [AI Observability data](/sql-reference/local/ai_observability_events). Supports Cortex Agent and External Agent; `agent_type` is `CORTEX AGENT` or `EXTERNAL AGENT`.  
| [GET_AI_OBSERVABILITY_LOGS (SNOWFLAKE.LOCAL)](/sql-reference/functions/get_ai_observability_logs-snowflake-local)| For more information, see [Cortex Agent evaluations](/user-guide/snowflake-cortex/cortex-agents-evaluations) and [AI Observability data](/sql-reference/local/ai_observability_events). Supports Cortex Agent and External Agent; `agent_type` is `CORTEX AGENT` or `EXTERNAL AGENT`.  
| [GET_AI_OBSERVABILITY_EVENTS (SNOWFLAKE.LOCAL)](/sql-reference/functions/get_ai_observability_events-snowflake-local)| For more information, see [Monitor Cortex Agent requests](/user-guide/snowflake-cortex/cortex-agents-monitor), [Monitor Cortex Search requests](/user-guide/snowflake-cortex/cortex-search/cortex-search-monitor), and [AI Observability data](/sql-reference/local/ai_observability_events). `agent_type` is `CORTEX AGENT`, `EXTERNAL AGENT`, or `CORTEX SEARCH SERVICE`.  
| [GET_AI_EVALUATION_DATA (SNOWFLAKE.LOCAL)](/sql-reference/functions/get_ai_evaluation_data-snowflake-local)| For more information, see [Cortex Agent evaluations](/user-guide/snowflake-cortex/cortex-agents-evaluations) and [AI Observability data](/sql-reference/local/ai_observability_events). Supports Cortex Agent and External Agent; `agent_type` is `CORTEX AGENT` or `EXTERNAL AGENT`.  
| [SYSTEM$CREATE_EVALUATION_DATASET](/sql-reference/functions/system_create_evaluation_dataset)| For more information, see [Cortex Agent evaluations](/user-guide/snowflake-cortex/cortex-agents-evaluations).  
  
## Syntax¶
[code] 
    SELECT ...
      FROM [ <input_table> [ [AS] <alias_1> ] ,
             [ LATERAL ]
           ]
           TABLE( <table_function>( [ <arg_1> [, ... ] ] ) ) [ [ AS ] <alias_2> ];
    
[/code]

For function-specific syntax, see the documentation for the individual system-defined table functions.

## Usage notes¶

  * Table functions can also be applied to a set of rows using the LATERAL construct.
  * To enable using table expressions, Snowflake supports ANSI/ISO standard syntax for table expressions in the [FROM](/sql-reference/constructs/from) clause of queries and subqueries. This syntax is used to indicate that an expression returns a collection of rows instead of a single row.
  * This ANSI/ISO syntax is valid only in the [FROM](/sql-reference/constructs/from) clause of the [SELECT](/sql-reference/sql/select) list. You cannot omit these keywords and parentheses from a collection subquery specification in any other context.
