---
title: "Account Usage | Snowflake Documentation"
source: https://docs.snowflake.com/sql-reference/account-usage
cert_domain: domain-3-model-development
crawl_depth: 1
crawled: 2026-08-23
---

# Account Usage¶

In the SNOWFLAKE database, the ACCOUNT_USAGE and READER_ACCOUNT_USAGE schemas enable querying object metadata, as well as historical usage data, for your account and all reader accounts (if any) associated with the account.

## Overview of Account Usage schemas¶

ACCOUNT_USAGE:
    

Views that display object metadata and usage metrics for your account.

In general, these views mirror the corresponding views and table functions in the Snowflake [Snowflake Information Schema](/sql-reference/info-schema), but with the following differences:

  * Records for dropped objects are included in each view.
  * Longer retention time for historical usage data.
  * Data latency.



For more details, see Differences Between Account Usage and Information Schema (in this topic). For more details about each view, see ACCOUNT_USAGE Views (in this topic).

READER_ACCOUNT_USAGE:
    

Views that display object metadata and usage metrics for all the reader accounts that have been created for your account (as a [Secure Data Sharing](/guides-overview-sharing) provider).

These views are a small subset of the ACCOUNT_USAGE views that apply to reader accounts. Also, each view in this schema contains an additional `READER_ACCOUNT_NAME` column for filtering results by reader account.

For more details about each view, see READER_ACCOUNT_USAGE Views (in this topic).

Note that these views are empty if no reader accounts have been created for your account.

## Differences between Account Usage and Information Schema¶

The Account Usage views and the corresponding views (or table functions) in the [Snowflake Information Schema](/sql-reference/info-schema) utilize identical structures and naming conventions, but with some key differences, as described in this section:

Difference| Account Usage| Information Schema  
---|---|---  
Includes dropped objects| Yes| No  
Latency of data| From 45 minutes to 3 hours (varies by view)| None  
Retention of historical data| 1 year| From 7 days to 6 months (varies by view/table function)  
  
For more details, see the following sections.

### Dropped object records¶

Account usage views include records for all objects that have been dropped. Many of the views for object types contain an additional `DELETED` column that displays the timestamp when the object was dropped.

In addition, because objects can be dropped and recreated with the same name, to differentiate between object records that have the same name, the account usage views include ID columns, where appropriate, that display the internal IDs generated and assigned to each record by the system.

If a column for an object name (for example, the `TABLE_NAME` column) is NULL, that object has been dropped. In this case, the columns for the names and IDs of the parent objects (for example, the `DATABASE_NAME` and `SCHEMA_NAME` columns) are also NULL.

Note that in some views, the column for the object name might still contain the name of the object, even if the object has been dropped.

### Data latency¶

Due to the process of extracting the data from Snowflake’s internal metadata store, the account usage views have some natural latency:

  * For most of the views, the latency is 2 hours (120 minutes).
  * For the remaining views, the latency varies between 45 minutes and 3 hours.



For details, see the list of views for each schema (in this topic). Also, note that these are all maximum time lengths; the actual latency for a given view when the view is queried may be less.

In contrast, views/table functions in the [Snowflake Information Schema](/sql-reference/info-schema) do not have any latency.

### Historical data retention¶

Certain account usage views provide historical usage metrics. The retention period for these views is 1 year (365 days).

In contrast, the corresponding views and table functions in the [Snowflake Information Schema](/sql-reference/info-schema) have much shorter retention periods, ranging from 7 days to 6 months, depending on the view.

## ACCOUNT_USAGE views¶

The ACCOUNT_USAGE schema contains the following views:

View| Type| Latency [1]| Edition [3]| Notes  
---|---|---|---|---  
[ACCESS_HISTORY](/sql-reference/account-usage/access_history)| Historical| 3 hours| Enterprise Edition (or higher)| Data retained for 1 year.  
[AGGREGATE_ACCESS_HISTORY](/sql-reference/account-usage/aggregate_access_history)| Historical| 3 hours| Enterprise Edition (or higher)| Data retained for 1 year.  
[AGGREGATE_QUERY_HISTORY](/sql-reference/account-usage/aggregate_query_history)| Historical| 3 hours| |   
[AGGREGATION_POLICIES](/sql-reference/account-usage/aggregation_policies)| Object| 2 hours| |   
[ALERT_HISTORY](/sql-reference/account-usage/alert_history)| Historical| 3 hours| | Data retained for 1 year.  
[ANOMALIES_DAILY](/sql-reference/account-usage/anomalies_daily)| Historical| 3 hours| | Data retained for 1 year.  
[APPLICATION_CALLBACK_HISTORY](/sql-reference/account-usage/application_callback_history)| Historical| 3 hours| | Data retained for 1 year.  
[APPLICATION_CONFIGURATIONS](/sql-reference/account-usage/application_configurations)| Object| 3 hours| | Data retained for 1 year.  
[APPLICATION_CONFIGURATION_VALUE_HISTORY](/sql-reference/account-usage/application_configuration_value_history)| Historical| 3 hours| | Data retained for 1 year.  
[APPLICATION_DAILY_USAGE_HISTORY](/sql-reference/account-usage/application_daily_usage_history)| Historical| 24 hours| | Data retained for 1 year.  
[APPLICATION_SPECIFICATION_STATUS_HISTORY](/sql-reference/account-usage/application_specification_status_history)| Historical| 1 hour| | Data retained for 1 year.  
[APPLICATION_SPECIFICATIONS](/sql-reference/account-usage/application_specifications)| Historical| 1 hour| | Data for deleted app specifications is retained for 1 year.  
[ARCHIVE_STORAGE_DATA_RETRIEVAL_USAGE_HISTORY](/sql-reference/account-usage/archive_storage_data_retrieval_usage_history)| Historical| 1 hour| | Data retained for 1 year.  
[AUTOMATIC_CLUSTERING_HISTORY](/sql-reference/account-usage/automatic_clustering_history)| Historical| 3 hours| | Data retained for 1 year.  
[BACKUP_OPERATION_HISTORY](/sql-reference/account-usage/backup_operation_history)| Historical| 6 hours| | Data retained for 1 year.  
[BACKUP_POLICIES](/sql-reference/account-usage/backup_policies)| Object| 6 hours| |   
[BACKUP_SETS](/sql-reference/account-usage/backup_sets)| Object| 6 hours| |   
[BACKUP_STORAGE_USAGE](/sql-reference/account-usage/backup_storage_usage)| Historical| 6 hours| | Data retained for 1 year.  
[BACKUPS](/sql-reference/account-usage/backups)| Object| 6 hours| |   
[BLOCK_STORAGE_HISTORY](/sql-reference/account-usage/block_storage_history)| Historical| 3 hours| | Data retained for 1 year.  
[BLOCK_STORAGE_SNAPSHOTS](/sql-reference/account-usage/block_storage_snapshots)| Object| 3 hours| |   
[CATALOG_LINKED_DATABASE_USAGE_HISTORY](/sql-reference/account-usage/catalog_linked_database_usage_history)| Historical| 3 hours| | Data retained for 1 year.  
[CLASS_INSTANCES](/sql-reference/account-usage/class_instances)| Object| 3 hours| | Data retained for 1 year.  
[CLASSES](/sql-reference/account-usage/classes)| Object| 3 hours| | Data retained for 1 year.  
[COLUMN_QUERY_PRUNING_HISTORY](/sql-reference/account-usage/column_query_pruning_history)| Historical| 4 hours| | Data retained for 1 year.  
[COLUMNS](/sql-reference/account-usage/columns)| Object| 90 minutes| |   
[COMPLETE_TASK_GRAPHS](/sql-reference/account-usage/complete_task_graphs)| Historical| 45 minutes| | Data retained for 1 year.  
[COMPUTE_POOLS](/sql-reference/account-usage/compute_pools)| Historical| 3 hours| | Data retained for 1 year.  
[CONTACT_REFERENCES](/sql-reference/account-usage/contact_references)| Object| 3 hours| |   
[CONTACTS](/sql-reference/account-usage/contacts)| Object| 3 hours| |   
[COPY_FILES_HISTORY](/sql-reference/account-usage/copy_files_history)| Historical| | | Data retained for 1 year.  
[COPY_HISTORY](/sql-reference/account-usage/copy_history)| Historical| 2 hours [2]| | Data retained for 1 year.  
[CORTEX_AGENT_USAGE_HISTORY](/sql-reference/account-usage/cortex_agent_usage_history)| Historical| 1 hour| | Data retained for 1 year.  
[CORTEX_AI_FUNCTIONS_USAGE_HISTORY](/sql-reference/account-usage/cortex_ai_functions_usage_history)| Historical| | | Data retained for 1 year.  
[CORTEX_AI_GUARDRAILS_USAGE_HISTORY](/sql-reference/account-usage/cortex_ai_guardrails_usage_history)| Historical| | | Data retained for 1 year.  
[CORTEX_AISQL_USAGE_HISTORY](/sql-reference/account-usage/cortex_aisql_usage_history)| Historical| | | Data retained for 1 year.  
[CORTEX_CODE_CLI_USAGE_HISTORY](/sql-reference/account-usage/cortex_code_cli_usage_history)| Historical| 1 hour| | Data retained for 1 year.  
[CORTEX_CODE_SNOWSIGHT_USAGE_HISTORY](/sql-reference/account-usage/cortex_code_snowsight_usage_history)| Historical| 1 hour| | Data retained for 1 year.  
[CORTEX_ANALYST_USAGE_HISTORY](/sql-reference/account-usage/cortex_analyst_usage_history)| Historical| 1 hour| | Data retained for 1 year.  
[CORTEX_DOCUMENT_PROCESSING_USAGE_HISTORY](/sql-reference/account-usage/cortex_document_processing_usage_history)| Historical| 1 hour| | Data retained for 1 year.  
[CORTEX_FINE_TUNING_USAGE_HISTORY](/sql-reference/account-usage/cortex_fine_tuning_usage_history)| Historical| 1 hour| | Data retained for 1 year.  
[CORTEX_FUNCTIONS_QUERY_USAGE_HISTORY](/sql-reference/account-usage/cortex_functions_query_usage_history)| Historical| | | Data retained for 1 year.  
[CORTEX_FUNCTIONS_USAGE_HISTORY](/sql-reference/account-usage/cortex_functions_usage_history)| Historical| | | Data retained for 1 year.  
[CORTEX_PROVISIONED_THROUGHPUT_USAGE_HISTORY](/sql-reference/account-usage/cortex_provisioned_throughput_usage_history)| Historical| | | Data retained for 1 year.  
[CORTEX_REST_API_RATE_LIMIT_POLICIES](/sql-reference/account-usage/cortex_rest_api_rate_limit_policies)| Object| 6 hours| |   
[CORTEX_REST_API_USAGE_HISTORY](/sql-reference/account-usage/cortex_rest_api_usage_history)| Historical| | | Data retained for 1 year.  
[CORTEX_SEARCH_BATCH_QUERY_USAGE_HISTORY](/sql-reference/account-usage/cortex_search_batch_query_usage_history)| Historical| 1 hour| | Data retained for 1 year.  
[CORTEX_SEARCH_DAILY_USAGE_HISTORY](/sql-reference/account-usage/cortex_search_daily_usage_history)| Historical| 3 hours| | Data retained for 1 year.  
[CORTEX_SEARCH_SERVING_USAGE_HISTORY](/sql-reference/account-usage/cortex_search_serving_usage_history)| Historical| 1 hour| | Data retained for 1 year.  
[CREDENTIALS](/sql-reference/account-usage/credentials)| Object| 2 hours| |   
[DATA_CLASSIFICATION_HISTORY](/sql-reference/account-usage/data_classification_history)| Historical| 3 hours| Enterprise Edition (or higher)| Data retained for 1 year.  
[DATA_CLASSIFICATION_LATEST](/sql-reference/account-usage/data_classification_latest)| Object| 3 hours| Enterprise Edition (or higher)| Data retained for as long as the table exists.  
[DATA_METRIC_FUNCTION_EXPECTATIONS](/sql-reference/account-usage/data_metric_function_expectations)| Object| 30 minutes| Enterprise Edition (or higher)|   
[DATA_METRIC_FUNCTION_REFERENCES](/sql-reference/account-usage/data_metric_function_references)| Object| 3 hours| Enterprise Edition (or higher)|   
[DATA_MOVEMENT_POLICIES](/sql-reference/account-usage/data_movement_policies)| Object| 2 hours| |   
[DATA_MOVEMENT_POLICY_RULES](/sql-reference/account-usage/data_movement_policy_rules)| Object| 2 hours| |   
[DATA_MOVEMENT_RULE_REFERENCES](/sql-reference/account-usage/data_movement_rule_references)| Historical| 3 hours| |   
[DATA_MOVEMENT_VIOLATIONS](/sql-reference/account-usage/data_movement_violations)| Historical| 3 hours| | Data is retained for one year.  
[DATA_QUALITY_MONITORING_USAGE_HISTORY](/sql-reference/account-usage/data_quality_monitoring_usage_history)| Historical| 3 hours| Enterprise Edition (or higher)| Data retained for 1 year.  
[DATABASES](/sql-reference/account-usage/databases)| Object| 3 hours| |   
[DATABASE_REPLICATION_USAGE_HISTORY](/sql-reference/account-usage/database_replication_usage_history)| Historical| 3 hours| | Data retained for 1 year.  
[DATABASE_STORAGE_USAGE_HISTORY](/sql-reference/account-usage/database_storage_usage_history)| Historical| 3 hours| | Data retained for 1 year.  
[DATA_TRANSFER_HISTORY](/sql-reference/account-usage/data_transfer_history)| Historical| 2 hours| | Data retained for 1 year.  
[DBT_PROJECT_EXECUTION_HISTORY](/sql-reference/account-usage/dbt_project_execution_history)| Historical| 2 hours| | Data retained for 1 year.  
[DOCUMENT_AI_USAGE_HISTORY](/sql-reference/account-usage/document_ai_usage_history)| Historical| | | Data retained for 1 year.  
[DYNAMIC_TABLE_REFRESH_HISTORY](/sql-reference/account-usage/dynamic_table_refresh_history)| Historical| 3 hours| | Data retained for 1 year.  
[ELEMENT_TYPES](/sql-reference/account-usage/element_types)| Object| 90 minutes| |   
[EVENT_ROUTING_TABLES](/sql-reference/account-usage/event_routing_tables)| Object| 2 hours| |   
[EVENT_USAGE_HISTORY](/sql-reference/account-usage/event_usage_history)| Historical| 3 hours| | Data retained for 1 year.  
[EXTERNAL_ACCESS_HISTORY](/sql-reference/account-usage/external_access_history)| Historical| 2 hours| | Data retained for 1 year.  
[FIELDS](/sql-reference/account-usage/fields)| Object| 90 minutes| |   
[FILE_FORMATS](/sql-reference/account-usage/file_formats)| Object| 2 hours| |   
[FUNCTIONS](/sql-reference/account-usage/functions)| Object| 2 hours| |   
[GRANTS_TO_ROLES](/sql-reference/account-usage/grants_to_roles)| Object| 2 hours| |   
[GRANTS_TO_SHARES](/sql-reference/account-usage/grants_to_shares)| Object| 3 hours| |   
[GRANTS_TO_USERS](/sql-reference/account-usage/grants_to_users)| Object| 2 hours| |   
[HYBRID_TABLES](/sql-reference/account-usage/hybrid_tables)| Object| 3 hours| |   
[HYBRID_TABLE_USAGE_HISTORY](/sql-reference/account-usage/hybrid_table_usage_history)| Historical| 3 hours| | Data retained for 1 year. (As of March 1, 2026, hybrid table requests are no longer billed, and metering was disabled soon after this pricing change took effect.)  
[ICEBERG_STORAGE_OPTIMIZATION_HISTORY](/sql-reference/account-usage/iceberg_storage_optimization_history)| Historical| 2 hours| | Data retained for 1 year.  
[INDEX_COLUMNS](/sql-reference/account-usage/index_columns)| Object| 3 hours| |   
[INDEXES](/sql-reference/account-usage/indexes)| Object| 3 hours| |   
[INGRESS_NETWORK_ACCESS_HISTORY](/sql-reference/account-usage/ingress_network_access_history)| Historical| 4 hours| | Data retained for 1 year.  
[INTERNAL_DATA_TRANSFER_HISTORY](/sql-reference/account-usage/internal_data_transfer_history)| Historical| 3 hours| |   
[INTERNAL_STAGE_NETWORK_ACCESS_HISTORY](/sql-reference/account-usage/internal_stage_network_access_history)| Historical| 6 hours| | Data retained for 1 year.  
[JOIN_POLICIES](/sql-reference/account-usage/join_policies)| Object| 2 hours| |   
[LISTINGS](/sql-reference/account-usage/listings)| Object| 3 hours| |   
[LOAD_HISTORY](/sql-reference/account-usage/load_history)| Historical| 90 minutes [2]| | Data retained for 1 year.  
[LOCK_WAIT_HISTORY](/sql-reference/account-usage/lock_wait_history)| Historical| 3 hours| | Data retained for 1 year.  
[LOGIN_HISTORY](/sql-reference/account-usage/login_history)| Historical| 2 hours| | Data retained for 1 year.  
[MASKING_POLICIES](/sql-reference/account-usage/masking_policies)| Object| 2 hours| |   
[MATERIALIZED_VIEW_REFRESH_HISTORY](/sql-reference/account-usage/materialized_view_refresh_history)| Historical| 3 hours| Enterprise Edition (or higher)| Data retained for 1 year.  
[METERING_DAILY_HISTORY](/sql-reference/account-usage/metering_daily_history)| Historical| 3 hours| | Data retained for 1 year.  
[METERING_HISTORY](/sql-reference/account-usage/metering_history)| Historical| 3 hours| | Data retained for 1 year.  
[NETWORK_POLICIES](/sql-reference/account-usage/network_policies)| Object| 2 hours| |   
[NETWORK_RULE_REFERENCES](/sql-reference/account-usage/network_rule_references)| Object| 2 hours| |   
[NETWORK_RULES](/sql-reference/account-usage/network_rules)| Object| 2 hours| |   
[NOTEBOOKS_CONTAINER_RUNTIME_HISTORY](/sql-reference/account-usage/notebooks_container_runtime_history)| Historical| 3 hours| |   
[OBJECT_ACCESS_REQUEST_HISTORY](/sql-reference/account-usage/object_access_request_history)| Historical| 3 hours| |   
[OBJECT_DEPENDENCIES](/sql-reference/account-usage/object_dependencies)| Historical| 3 hours| |   
[ONLINE_FEATURE_TABLE_REFRESH_HISTORY](/sql-reference/account-usage/online_feature_table_refresh_history)| Historical| 3 hours| |   
[OPENFLOW_USAGE_HISTORY](/sql-reference/account-usage/openflow_usage_history)| Historical| 3 hours| |   
[OUTBOUND_PRIVATELINK_ENDPOINTS](/sql-reference/account-usage/outbound_privatelink_endpoints)| Object| 2 hours| Business Critical (or higher)| Data for deleted endpoints is retained for 1 year.  
[PASSWORD_POLICIES](/sql-reference/account-usage/password_policies)| Object| 2 hours| |   
[PIPES](/sql-reference/account-usage/pipes)| Object| 2 hours| |   
[PIPE_USAGE_HISTORY](/sql-reference/account-usage/pipe_usage_history)| Historical| 3 hours| | Data retained for 1 year.  
[POLICY_REFERENCES](/sql-reference/account-usage/policy_references)| Object| 2 hours| |   
[POSTGRES_COMPUTE_USAGE_HISTORY](/sql-reference/account-usage/postgres_compute_usage_history)| Historical| 3 hours| | Data retained for 1 year.  
[POSTGRES_STORAGE_USAGE_HISTORY](/sql-reference/account-usage/postgres_storage_usage_history)| Historical| 3 hours| | Data retained for 1 year.  
[PRIVACY_BUDGETS](/sql-reference/account-usage/privacy_budgets)| Object| 24 hours| Enterprise Edition (or higher)|   
[PRIVACY_POLICIES](/sql-reference/account-usage/privacy_policies)| Object| 2 hours| Enterprise Edition (or higher)|   
[PROCEDURES](/sql-reference/account-usage/procedures)| Object| 2 hours| |   
[PROJECTION_POLICIES](/sql-reference/account-usage/projection_policies)| Object| 2 hours| |   
[QUERY_ACCELERATION_ELIGIBLE](/sql-reference/account-usage/query_acceleration_eligible)| Historical| 3 hours| | Data retained for 1 year.  
[QUERY_ACCELERATION_HISTORY](/sql-reference/account-usage/query_acceleration_history)| Historical| 3 hours| Enterprise Edition (or higher)| Data retained for 1 year.  
[QUERY_ATTRIBUTION_HISTORY](/sql-reference/account-usage/query_attribution_history)| Historical| 8 hours| | Data retained for 1 year.  
[QUERY_HISTORY](/sql-reference/account-usage/query_history)| Historical| 45 minutes| | Data retained for 1 year.  
[QUERY_INSIGHTS](/sql-reference/account-usage/query_insights)| Historical| | | Data retained for 1 year.  
[QUERY_METERING_HISTORY](/sql-reference/account-usage/query_metering_history)| Historical| 1 hour| | Data retained for 1 year.  
[REFERENTIAL_CONSTRAINTS](/sql-reference/account-usage/referential_constraints)| Object| 2 hours| |   
[REPLICATION_GROUP_REFRESH_HISTORY](/sql-reference/account-usage/replication_group_refresh_history)| Historical| 3 hours| | Data retained for 1 year.  
[REPLICATION_GROUP_USAGE_HISTORY](/sql-reference/account-usage/replication_group_usage_history)| Historical| 3 hours| | Data retained for 1 year.  
[REPLICATION_GROUPS](/sql-reference/account-usage/replication_groups)| Object| 2 hours| |   
[REPLICATION_USAGE_HISTORY](/sql-reference/account-usage/replication_usage_history)| Historical| 3 hours| | Data retained for 1 year.  
[RESOURCE_MONITORS](/sql-reference/account-usage/resource_monitors)| Object| 2 hours| |   
[ROLES](/sql-reference/account-usage/roles)| Object| 2 hours| |   
[ROW_ACCESS_POLICIES](/sql-reference/account-usage/row_access_policies)| Object| 2 hours| |   
[SCHEMATA](/sql-reference/account-usage/schemata)| Object| 2 hours| |   
[SEARCH_OPTIMIZATION_BENEFITS](/sql-reference/account-usage/search_optimization_benefits)| Historical| 6 hours| Enterprise Edition (or higher)| Data retained for 1 year.  
[SEARCH_OPTIMIZATION_HISTORY](/sql-reference/account-usage/search_optimization_history)| Historical| 3 hours| Enterprise Edition (or higher)| Data retained for 1 year.  
[SECRETS](/sql-reference/account-usage/secrets)| Object| 2 hours| |   
[SEMANTIC_DIMENSIONS](/sql-reference/account-usage/semantic_dimensions)| Object| 2 hours| |   
[SEMANTIC_FACTS](/sql-reference/account-usage/semantic_facts)| Object| 2 hours| |   
[SEMANTIC_METRICS](/sql-reference/account-usage/semantic_metrics)| Object| 2 hours| |   
[SEMANTIC_RELATIONSHIPS](/sql-reference/account-usage/semantic_relationships)| Object| 2 hours| |   
[SEMANTIC_TABLES](/sql-reference/account-usage/semantic_tables)| Object| 2 hours| |   
[SEMANTIC_VIEWS](/sql-reference/account-usage/semantic_views)| Object| 2 hours| |   
[SEQUENCES](/sql-reference/account-usage/sequences)| Object| 2 hours| |   
[SERVERLESS_ALERT_HISTORY](/sql-reference/account-usage/serverless_alert_history)| Historical| 3 hours| | Data retained for 1 year.  
[SERVERLESS_TASK_HISTORY](/sql-reference/account-usage/serverless_task_history)| Historical| 3 hours| | Data retained for 1 year.  
[SERVICES](/sql-reference/account-usage/services)| Object| 3 hours| |   
[SESSION_POLICIES](/sql-reference/account-usage/session_policies)| Object| 2 hours| |   
[SESSIONS](/sql-reference/account-usage/sessions)| Historical| 3 hours| | Data retained for 1 year.  
[SHARES](/sql-reference/account-usage/shares)| Object| 3 hours| |   
[SNAPSHOT_OPERATION_HISTORY](/sql-reference/account-usage/snapshot_operation_history)| Historical| 6 hours| | Data retained for 1 year. This view is deprecated. Use the [BACKUP_OPERATION_HISTORY](/sql-reference/account-usage/backup_operation_history) view instead.  
[SNAPSHOT_POLICIES](/sql-reference/account-usage/snapshot_policies)| Object| 6 hours| | This view is deprecated. Use the [BACKUP_POLICIES](/sql-reference/account-usage/backup_policies) view instead.  
[SNAPSHOT_SETS](/sql-reference/account-usage/snapshot_sets)| Object| 6 hours| | This view is deprecated. Use the [BACKUP_SETS](/sql-reference/account-usage/backup_sets) view instead.  
[SNAPSHOT_STORAGE_USAGE](/sql-reference/account-usage/snapshot_storage_usage)| Historical| 6 hours| | Data retained for 1 year. This view is deprecated. Use the [BACKUP_STORAGE_USAGE](/sql-reference/account-usage/backup_storage_usage) view instead.  
[SNAPSHOTS](/sql-reference/account-usage/snapshots)| Object| 6 hours| | This view is deprecated. Use the [BACKUPS](/sql-reference/account-usage/backups) view instead.  
[SNOWFLAKE_APP_RUNTIME_COMPUTE_HISTORY](/sql-reference/account-usage/snowflake_app_runtime_compute_history)| Historical| 3 hours| | Data retained for 1 year.  
[SNOWFLAKE_COCO_USAGE_HISTORY](/sql-reference/account-usage/snowflake_coco_usage_history)| Historical| 1 hour| | Data retained for 1 year.  
[SNOWFLAKE_COWORK_USAGE_HISTORY](/sql-reference/account-usage/snowflake_cowork_usage_history_view)| Historical| 1 hour| | Data retained for 1 year.  
[SNOWPARK_CONTAINER_SERVICES_HISTORY](/sql-reference/account-usage/snowpark_container_services_history)| Historical| 3 hours| | Data retained for 1 year.  
[SNOWPIPE_STREAMING_CHANNEL_HISTORY](/sql-reference/account-usage/snowpipe_streaming_channel_history)| Historical| | |   
[SNOWPIPE_STREAMING_CLIENT_HISTORY](/sql-reference/account-usage/snowpipe_streaming_client_history)| Historical| 2 hours| | Data retained for 1 year.  
[SNOWPIPE_STREAMING_FILE_MIGRATION_HISTORY](/sql-reference/account-usage/snowpipe_streaming_file_migration_history)| Historical| 12 hours| | Data retained for 1 year.  
[STAGES](/sql-reference/account-usage/stages)| Object| 2 hours| |   
[STAGE_STORAGE_USAGE_HISTORY](/sql-reference/account-usage/stage_storage_usage_history)| Historical| 2 hours| | Data retained for 1 year.  
[STORAGE_LIFECYCLE_POLICIES](/sql-reference/account-usage/storage_lifecycle_policies)| Object| 2 hours| |   
[STORAGE_LIFECYCLE_POLICY_HISTORY](/sql-reference/account-usage/storage_lifecycle_policy_history)| Historical| 2 hours| | Data retained for 1 year.  
[STORAGE_REQUEST_HISTORY](/sql-reference/account-usage/storage_request_history)| Historical| 6 hours| | Data retained for 1 year.  
[STORAGE_USAGE](/sql-reference/account-usage/storage_usage)| Historical| 2 hours| | Combined usage across all database tables and internal stages. Data retained for 1 year.  
[TABLES](/sql-reference/account-usage/tables)| Object| 90 minutes| |   
[TABLE_CONSTRAINTS](/sql-reference/account-usage/table_constraints)| Object| 2 hours| |   
[TABLE_DML_HISTORY](/sql-reference/account-usage/table_dml_history)| Historical| 6 hours| | Data retained for 1 year.  
[TABLE_PRUNING_HISTORY](/sql-reference/account-usage/table_pruning_history)| Historical| 6 hours| | Data retained for 1 year.  
[TABLE_QUERY_PRUNING_HISTORY](/sql-reference/account-usage/table_query_pruning_history)| Historical| 4 hours| | Data retained for 1 year.  
[TABLE_STORAGE_METRICS](/sql-reference/account-usage/table_storage_metrics)| Object| 90 minutes| |   
[TAG_REFERENCES](/sql-reference/account-usage/tag_references)| Object| 2 hours| |   
[TAGS](/sql-reference/account-usage/tags)| Object| 2 hours| |   
[TASK_HISTORY](/sql-reference/account-usage/task_history)| Historical| 45 minutes| |   
[TASK_VERSIONS](/sql-reference/account-usage/task_versions)| Object| 3 hours| |   
[TRI_SECRET_SECURE_HISTORY](/sql-reference/account-usage/tri-secret-secure-history)| Historical| 2 hours| |   
[TRUST_CENTER_FINDINGS](/sql-reference/account-usage/trust_center_findings)| Historical| 1 hour| |   
[TYPES](/sql-reference/account-usage/types)| Object| 2 hours| |   
[USERS](/sql-reference/account-usage/users)| Object| 2 hours| |   
[VIEWS](/sql-reference/account-usage/views)| Object| 90 minutes| |   
[WAREHOUSE_EVENTS_HISTORY](/sql-reference/account-usage/warehouse_events_history)| Historical| 3 hours| | Data retained for 1 year.  
[WAREHOUSE_LOAD_HISTORY](/sql-reference/account-usage/warehouse_load_history)| Historical| 3 hours| | Data retained for 1 year.  
[WAREHOUSE_METERING_HISTORY](/sql-reference/account-usage/warehouse_metering_history)| Historical| 3 hours| | Data retained for 1 year.  
  
[1] All latency times are approximate; in some instances, the actual latency may be lower.

[2] The latency of the views for a given table may be up to 2 days if both of the following conditions are true: 1. Fewer than 32 DML statements have been added to the given table since it was last updated in LOAD_HISTORY or COPY_HISTORY. 2. Fewer than 100 rows have been added to the given table since it was last updated in LOAD_HISTORY or COPY_HISTORY.

[3] Unless otherwise noted, the Account Usage view is available to all accounts.

### Account Usage table functions¶

Currently, Snowflake supports one ACCOUNT_USAGE table function:

Table Function| Data Retention| Notes  
---|---|---  
[TAG_REFERENCES_WITH_LINEAGE](/sql-reference/functions/tag_references_with_lineage)| N/A| Results are only returned for the role that has access to the specified object.  
  
Note

Similar to the Account Usage views, please account for latency when calling this table function. The expected latency for this table function is similar to the latency for the TAG_REFERENCES view.

## READER_ACCOUNT_USAGE views¶

The READER_ACCOUNT_USAGE schema contains the following views:

View| Type| Latency [1]| Notes  
---|---|---|---  
[LOGIN_HISTORY](/sql-reference/account-usage/login_history)| Historical| 2 hours| Data retained for 1 year.  
[QUERY_HISTORY](/sql-reference/account-usage/query_history)| Historical| 45 minutes| Data retained for 1 year.  
[RESOURCE_MONITORS](/sql-reference/account-usage/resource_monitors)| Object| 2 hours|   
[STORAGE_USAGE](/sql-reference/account-usage/storage_usage)| Historical| 24 hours| Combined usage across all database tables and internal stages. Data retained for 1 year.  
[WAREHOUSE_METERING_HISTORY](/sql-reference/account-usage/warehouse_metering_history)| Historical| 24 hours| Data retained for 1 year.  
  
[1] All latency times are approximate; in some instances, the actual latency may be lower.

## Enabling other roles to use schemas in the SNOWFLAKE database¶

By default, the SNOWFLAKE database is visible to all users; however, access to schemas in this database can be granted by a user with the ACCOUNTADMIN role using either of the following approaches:

  * Grant IMPORTED PRIVILEGES on the SNOWFLAKE database.
  * Grant a SNOWFLAKE database role to an account role.



Important

To avoid unintentionally granting access to organization-level data, consider using SNOWFLAKE database roles to grant access to views in the ACCOUNT_USAGE schema.

For more information, refer to [GRANT DATABASE ROLE](/sql-reference/sql/grant-database-role).

For example, to grant IMPORTED PRIVILEGES on the SNOWFLAKE database to two additional roles:

> 
[code]
>     USE ROLE ACCOUNTADMIN;
>     
>     GRANT IMPORTED PRIVILEGES ON DATABASE SNOWFLAKE TO ROLE SYSADMIN;
>     GRANT IMPORTED PRIVILEGES ON DATABASE SNOWFLAKE TO ROLE customrole1;
>     
[/code]

A user who is granted the `customrole1` role can query a view as follows:

> 
[code]
>     USE ROLE customrole1;
>     
>     SELECT database_name, database_owner FROM SNOWFLAKE.ACCOUNT_USAGE.DATABASES;
>     
[/code]

For additional examples, see Querying the Account Usage views.

### ACCOUNT_USAGE schema SNOWFLAKE database roles¶

In addition, you can grant finer control to accounts using SNOWFLAKE database roles. For more information on database roles, see [database roles](/user-guide/security-access-control-considerations#label-access-control-considerations-database-roles).

[ACCOUNT_USAGE](/sql-reference/account-usage) schemas have four defined SNOWFLAKE database roles, each granted the SELECT privilege on specific views.

Role| Purpose and Description  
---|---  
OBJECT_VIEWER| The OBJECT_VIEWER role provides visibility into object metadata.  
USAGE_VIEWER| The USAGE_VIEWER role provides visibility into historical usage information.  
GOVERNANCE_VIEWER| The GOVERNANCE_VIEWER role provides visibility into data-governance-related information.  
SECURITY_VIEWER| The SECURITY_VIEWER role provides visibility into security-based information.  
  
# Database role required to access ACCOUNT_USAGE views¶

The OBJECT_VIEWER, USAGE_VIEWER, GOVERNANCE_VIEWER, and SECURITY_VIEWER roles have the SELECT privilege to query Account Usage views in the shared SNOWFLAKE database. Use the following table to determine which database role has access to a view.

View| Database Role  
---|---  
[ACCESS_HISTORY view](/sql-reference/account-usage/access_history)| GOVERNANCE_VIEWER  
[AGGREGATE_ACCESS_HISTORY view](/sql-reference/account-usage/aggregate_access_history)| GOVERNANCE_VIEWER  
[AGGREGATE_QUERY_HISTORY view](/sql-reference/account-usage/aggregate_query_history)| GOVERNANCE_VIEWER  
[AGGREGATION_POLICIES view](/sql-reference/account-usage/aggregation_policies)| GOVERNANCE_VIEWER  
[ANOMALIES_DAILY view](/sql-reference/account-usage/anomalies_daily)| USAGE_VIEWER  
[APPLICATION_CALLBACK_HISTORY view](/sql-reference/account-usage/application_callback_history)| SECURITY_VIEWER  
[APPLICATION_CONFIGURATIONS view](/sql-reference/account-usage/application_configurations)| SECURITY_VIEWER  
[APPLICATION_CONFIGURATION_VALUE_HISTORY view](/sql-reference/account-usage/application_configuration_value_history)| SECURITY_VIEWER  
[APPLICATION_DAILY_USAGE_HISTORY view](/sql-reference/account-usage/application_daily_usage_history)| USAGE_VIEWER  
[APPLICATION_SPECIFICATION_STATUS_HISTORY view](/sql-reference/account-usage/application_specification_status_history)| SECURITY_VIEWER  
[APPLICATION_SPECIFICATIONS view](/sql-reference/account-usage/application_specifications)| SECURITY_VIEWER  
[ARCHIVE_STORAGE_DATA_RETRIEVAL_USAGE_HISTORY view](/sql-reference/account-usage/archive_storage_data_retrieval_usage_history)| USAGE_VIEWER  
[AUTOMATIC_CLUSTERING_HISTORY view](/sql-reference/account-usage/automatic_clustering_history)| USAGE_VIEWER  
[BLOCK_STORAGE_HISTORY view](/sql-reference/account-usage/block_storage_history)| USAGE_VIEWER  
[BLOCK_STORAGE_SNAPSHOTS view](/sql-reference/account-usage/block_storage_snapshots)| OBJECT_VIEWER  
[CATALOG_LINKED_DATABASE_USAGE_HISTORY view](/sql-reference/account-usage/catalog_linked_database_usage_history)| USAGE_VIEWER  
[CLASS_INSTANCES view](/sql-reference/account-usage/class_instances)| USAGE_VIEWER  
[CLASSES view](/sql-reference/account-usage/classes)| USAGE_VIEWER  
[COLUMN_QUERY_PRUNING_HISTORY view](/sql-reference/account-usage/column_query_pruning_history)| USAGE_VIEWER  
[COLUMNS view](/sql-reference/account-usage/columns)| OBJECT_VIEWER  
[COMPLETE_TASK_GRAPHS view](/sql-reference/account-usage/complete_task_graphs)| OBJECT_VIEWER  
[CONTACT_REFERENCES view](/sql-reference/account-usage/contact_references)| GOVERNANCE_VIEWER  
[CONTACTS view](/sql-reference/account-usage/contacts)| GOVERNANCE_VIEWER  
[COPY_FILES_HISTORY view](/sql-reference/account-usage/copy_files_history)| USAGE_VIEWER  
[COPY_HISTORY view](/sql-reference/account-usage/copy_history)| USAGE_VIEWER  
[CORTEX_AGENT_USAGE_HISTORY view](/sql-reference/account-usage/cortex_agent_usage_history)| USAGE_VIEWER  
[CORTEX_AI_FUNCTIONS_USAGE_HISTORY view](/sql-reference/account-usage/cortex_ai_functions_usage_history)| USAGE_VIEWER  
[CORTEX_AI_GUARDRAILS_USAGE_HISTORY view](/sql-reference/account-usage/cortex_ai_guardrails_usage_history)| USAGE_VIEWER  
[CORTEX_AISQL_USAGE_HISTORY view](/sql-reference/account-usage/cortex_aisql_usage_history)| USAGE_VIEWER  
[CORTEX_ANALYST_USAGE_HISTORY view](/sql-reference/account-usage/cortex_analyst_usage_history)| USAGE_VIEWER  
[CORTEX_DOCUMENT_PROCESSING_USAGE_HISTORY view](/sql-reference/account-usage/cortex_document_processing_usage_history)| USAGE_VIEWER  
[CORTEX_FINE_TUNING_USAGE_HISTORY view](/sql-reference/account-usage/cortex_fine_tuning_usage_history)| USAGE_VIEWER  
[CORTEX_FUNCTIONS_QUERY_USAGE_HISTORY view](/sql-reference/account-usage/cortex_functions_query_usage_history)| USAGE_VIEWER  
[CORTEX_FUNCTIONS_USAGE_HISTORY view](/sql-reference/account-usage/cortex_functions_usage_history)| USAGE_VIEWER  
[CORTEX_PROVISIONED_THROUGHPUT_USAGE_HISTORY view](/sql-reference/account-usage/cortex_provisioned_throughput_usage_history)| USAGE_VIEWER  
[CORTEX_REST_API_USAGE_HISTORY view](/sql-reference/account-usage/cortex_rest_api_usage_history)| USAGE_VIEWER  
[CORTEX_SEARCH_BATCH_QUERY_USAGE_HISTORY view](/sql-reference/account-usage/cortex_search_batch_query_usage_history)| USAGE_VIEWER  
[CORTEX_SEARCH_DAILY_USAGE_HISTORY view](/sql-reference/account-usage/cortex_search_daily_usage_history)| USAGE_VIEWER  
[CORTEX_SEARCH_SERVING_USAGE_HISTORY view](/sql-reference/account-usage/cortex_search_serving_usage_history)| USAGE_VIEWER  
[CREDENTIALS view](/sql-reference/account-usage/credentials)| SECURITY_VIEWER  
[DATA_CLASSIFICATION_HISTORY view](/sql-reference/account-usage/data_classification_history)| GOVERNANCE_VIEWER  
[DATA_CLASSIFICATION_LATEST view](/sql-reference/account-usage/data_classification_latest)| GOVERNANCE_VIEWER  
[DATA_METRIC_FUNCTION_EXPECTATIONS view](/sql-reference/account-usage/data_metric_function_expectations)| USAGE_VIEWER or GOVERNANCE_VIEWER  
[DATA_METRIC_FUNCTION_REFERENCES view](/sql-reference/account-usage/data_metric_function_references)| USAGE_VIEWER or GOVERNANCE_VIEWER  
[DATA_QUALITY_MONITORING_USAGE_HISTORY view](/sql-reference/account-usage/data_quality_monitoring_usage_history)| USAGE_VIEWER  
[DATA_TRANSFER_HISTORY view](/sql-reference/account-usage/data_transfer_history)| USAGE_VIEWER  
[DATABASE_STORAGE_USAGE_HISTORY view](/sql-reference/account-usage/database_storage_usage_history)| USAGE_VIEWER  
[DATABASES view](/sql-reference/account-usage/databases)| OBJECT_VIEWER  
[DOCUMENT_AI_USAGE_HISTORY view](/sql-reference/account-usage/document_ai_usage_history)| USAGE_VIEWER  
[DYNAMIC_TABLE_REFRESH_HISTORY view](/sql-reference/account-usage/dynamic_table_refresh_history)| USAGE_VIEWER  
[ELEMENT_TYPES view](/sql-reference/account-usage/element_types)| OBJECT_VIEWER  
[EVENT_USAGE_HISTORY view](/sql-reference/account-usage/event_usage_history)| USAGE_VIEWER  
[EXTERNAL_ACCESS_HISTORY view](/sql-reference/account-usage/external_access_history)| USAGE_VIEWER  
[FIELDS view](/sql-reference/account-usage/fields)| OBJECT_VIEWER  
[FILE_FORMATS view](/sql-reference/account-usage/file_formats)| OBJECT_VIEWER  
[FUNCTIONS view](/sql-reference/account-usage/functions)| OBJECT_VIEWER  
[GRANTS_TO_ROLES view](/sql-reference/account-usage/grants_to_roles)| SECURITY_VIEWER  
[GRANTS_TO_SHARES view](/sql-reference/account-usage/grants_to_shares)| SECURITY_VIEWER  
[GRANTS_TO_USERS view](/sql-reference/account-usage/grants_to_users)| SECURITY_VIEWER  
[HYBRID_TABLE_USAGE_HISTORY view](/sql-reference/account-usage/hybrid_table_usage_history)| USAGE_VIEWER  
[HYBRID_TABLES view](/sql-reference/account-usage/hybrid_tables)| OBJECT_VIEWER  
[ICEBERG_STORAGE_OPTIMIZATION_HISTORY view](/sql-reference/account-usage/iceberg_storage_optimization_history)| USAGE_VIEWER  
[INDEX_COLUMNS view](/sql-reference/account-usage/index_columns)| OBJECT_VIEWER  
[INDEXES view](/sql-reference/account-usage/indexes)| OBJECT_VIEWER  
[INGRESS_NETWORK_ACCESS_HISTORY view](/sql-reference/account-usage/ingress_network_access_history)| SECURITY_VIEWER  
[INTERNAL_DATA_TRANSFER_HISTORY view](/sql-reference/account-usage/internal_data_transfer_history)| USAGE_VIEWER  
[INTERNAL_STAGE_NETWORK_ACCESS_HISTORY view](/sql-reference/account-usage/internal_stage_network_access_history)| SECURITY_VIEWER  
[JOIN_POLICIES view](/sql-reference/account-usage/join_policies)| GOVERNANCE_VIEWER  
[LISTINGS view](/sql-reference/account-usage/listings)| SECURITY_VIEWER  
[LOAD_HISTORY view](/sql-reference/account-usage/load_history)| USAGE_VIEWER  
[LOGIN_HISTORY view](/sql-reference/account-usage/login_history)| SECURITY_VIEWER  
[MASKING_POLICIES view](/sql-reference/account-usage/masking_policies)| GOVERNANCE_VIEWER  
[MATERIALIZED_VIEW_REFRESH_HISTORY view](/sql-reference/account-usage/materialized_view_refresh_history)| USAGE_VIEWER  
[METERING_DAILY_HISTORY view](/sql-reference/account-usage/metering_daily_history)| USAGE_VIEWER  
[METERING_HISTORY view](/sql-reference/account-usage/metering_history)| USAGE_VIEWER  
[NETWORK_POLICIES view](/sql-reference/account-usage/network_policies)| SECURITY_VIEWER  
[NETWORK_RULE_REFERENCES view](/sql-reference/account-usage/network_rule_references)| SECURITY_VIEWER  
[NETWORK_RULES view](/sql-reference/account-usage/network_rules)| SECURITY_VIEWER  
[NOTEBOOKS_CONTAINER_RUNTIME_HISTORY view](/sql-reference/account-usage/notebooks_container_runtime_history)| USAGE_VIEWER  
[OBJECT_ACCESS_REQUEST_HISTORY view](/sql-reference/account-usage/object_access_request_history)| OBJECT_VIEWER  
[OBJECT_DEPENDENCIES view](/sql-reference/account-usage/object_dependencies)| OBJECT_VIEWER  
[ONLINE_FEATURE_TABLE_REFRESH_HISTORY view](/sql-reference/account-usage/online_feature_table_refresh_history)| USAGE_VIEWER  
[OPENFLOW_USAGE_HISTORY view](/sql-reference/account-usage/openflow_usage_history)| USAGE_VIEWER  
[OUTBOUND_PRIVATELINK_ENDPOINTS view](/sql-reference/account-usage/outbound_privatelink_endpoints)| SECURITY_VIEWER  
[PASSWORD_POLICIES view](/sql-reference/account-usage/password_policies)| SECURITY_VIEWER  
[PIPE_USAGE_HISTORY view](/sql-reference/account-usage/pipe_usage_history)| USAGE_VIEWER  
[PIPES view](/sql-reference/account-usage/pipes)| OBJECT_VIEWER  
[POLICY_REFERENCES view](/sql-reference/account-usage/policy_references)| GOVERNANCE_VIEWER, SECURITY_VIEWER  
[POSTGRES_COMPUTE_USAGE_HISTORY view](/sql-reference/account-usage/postgres_compute_usage_history)| USAGE_VIEWER  
[POSTGRES_STORAGE_USAGE_HISTORY view](/sql-reference/account-usage/postgres_storage_usage_history)| USAGE_VIEWER  
[PRIVACY_BUDGETS view](/sql-reference/account-usage/privacy_budgets)| GOVERNANCE_VIEWER  
[PRIVACY_POLICIES view](/sql-reference/account-usage/privacy_policies)| GOVERNANCE_VIEWER  
[PROCEDURES view](/sql-reference/account-usage/procedures)| OBJECT_VIEWER  
[PROJECTION_POLICIES view](/sql-reference/account-usage/projection_policies)| GOVERNANCE_VIEWER  
[QUERY_ACCELERATION_ELIGIBLE view](/sql-reference/account-usage/query_acceleration_eligible)| GOVERNANCE_VIEWER  
[QUERY_ATTRIBUTION_HISTORY view](/sql-reference/account-usage/query_attribution_history)| USAGE_VIEWER, GOVERNANCE_VIEWER  
[QUERY_HISTORY view](/sql-reference/account-usage/query_history)| GOVERNANCE_VIEWER  
[QUERY_INSIGHTS view](/sql-reference/account-usage/query_insights)| GOVERNANCE_VIEWER  
[QUERY_METERING_HISTORY view](/sql-reference/account-usage/query_metering_history)| USAGE_VIEWER, GOVERNANCE_VIEWER  
[REFERENTIAL_CONSTRAINTS view](/sql-reference/account-usage/referential_constraints)| OBJECT_VIEWER  
[REPLICATION_GROUP_REFRESH_HISTORY view](/sql-reference/account-usage/replication_group_refresh_history)| USAGE_VIEWER  
[REPLICATION_GROUP_USAGE_HISTORY view](/sql-reference/account-usage/replication_group_usage_history)| USAGE_VIEWER  
[REPLICATION_GROUPS view](/sql-reference/account-usage/replication_groups)| OBJECT_VIEWER  
[REPLICATION_USAGE_HISTORY view](/sql-reference/account-usage/replication_usage_history)| USAGE_VIEWER  
[RESOURCE_MONITORS view](/sql-reference/account-usage/resource_monitors)| OBJECT_VIEWER  
[ROLES view](/sql-reference/account-usage/roles)| SECURITY_VIEWER  
[ROW_ACCESS_POLICIES view](/sql-reference/account-usage/row_access_policies)| GOVERNANCE_VIEWER  
[SCHEMATA view](/sql-reference/account-usage/schemata)| OBJECT_VIEWER  
[SEARCH_OPTIMIZATION_BENEFITS view](/sql-reference/account-usage/search_optimization_benefits)| USAGE_VIEWER  
[SEARCH_OPTIMIZATION_HISTORY view](/sql-reference/account-usage/search_optimization_history)| USAGE_VIEWER  
[SECRETS view](/sql-reference/account-usage/secrets)| SECURITY_VIEWER  
[SEMANTIC_DIMENSIONS view](/sql-reference/account-usage/semantic_dimensions)| OBJECT_VIEWER  
[SEMANTIC_FACTS view](/sql-reference/account-usage/semantic_facts)| OBJECT_VIEWER  
[SEMANTIC_METRICS view](/sql-reference/account-usage/semantic_metrics)| OBJECT_VIEWER  
[SEMANTIC_RELATIONSHIPS view](/sql-reference/account-usage/semantic_relationships)| OBJECT_VIEWER  
[SEMANTIC_TABLES view](/sql-reference/account-usage/semantic_tables)| OBJECT_VIEWER  
[SEMANTIC_VIEWS view](/sql-reference/account-usage/semantic_views)| OBJECT_VIEWER  
[SEQUENCES view](/sql-reference/account-usage/sequences)| OBJECT_VIEWER  
[SERVERLESS_ALERT_HISTORY view](/sql-reference/account-usage/serverless_alert_history)| USAGE_VIEWER  
[SERVERLESS_TASK_HISTORY view](/sql-reference/account-usage/serverless_task_history)| USAGE_VIEWER  
[SERVICES view](/sql-reference/account-usage/services)| OBJECT_VIEWER  
[SESSION_POLICIES view](/sql-reference/account-usage/session_policies)| SECURITY_VIEWER  
[SESSIONS view](/sql-reference/account-usage/sessions)| SECURITY_VIEWER  
[SHARES view](/sql-reference/account-usage/shares)| SECURITY_VIEWER  
[SNAPSHOT_OPERATION_HISTORY view --- Deprecated](/sql-reference/account-usage/snapshot_operation_history)| OBJECT_VIEWER  
[SNAPSHOT_POLICIES view --- Deprecated](/sql-reference/account-usage/snapshot_policies)| OBJECT_VIEWER  
[SNAPSHOT_SETS view --- Deprecated](/sql-reference/account-usage/snapshot_sets)| OBJECT_VIEWER  
[SNAPSHOT_STORAGE_USAGE view --- Deprecated](/sql-reference/account-usage/snapshot_storage_usage)| OBJECT_VIEWER  
[SNAPSHOTS view — Deprecated](/sql-reference/account-usage/snapshots)| OBJECT_VIEWER  
[SNOWFLAKE_COCO_USAGE_HISTORY view](/sql-reference/account-usage/snowflake_coco_usage_history)| USAGE_VIEWER  
[SNOWFLAKE_COWORK_USAGE_HISTORY view](/sql-reference/account-usage/snowflake_cowork_usage_history)| USAGE_VIEWER  
[SNOWPARK_CONTAINER_SERVICES_HISTORY view](/sql-reference/account-usage/snowpark_container_services_history)| USAGE_VIEWER  
[SNOWPIPE_STREAMING_CHANNEL_HISTORY view](/sql-reference/account-usage/snowpipe_streaming_channel_history)| USAGE_VIEWER  
[STAGE_STORAGE_USAGE_HISTORY view](/sql-reference/account-usage/stage_storage_usage_history)| USAGE_VIEWER  
[STAGES view](/sql-reference/account-usage/stages)| OBJECT_VIEWER  
[STORAGE_LIFECYCLE_POLICIES view](/sql-reference/account-usage/storage_lifecycle_policies)| GOVERNANCE_VIEWER  
[STORAGE_LIFECYCLE_POLICY_HISTORY view](/sql-reference/account-usage/storage_lifecycle_policy_history)| GOVERNANCE_VIEWER  
[STORAGE_REQUEST_HISTORY view](/sql-reference/account-usage/storage_request_history)| USAGE_VIEWER  
[STORAGE_USAGE view](/sql-reference/account-usage/storage_usage)| USAGE_VIEWER  
[TABLE_CONSTRAINTS view](/sql-reference/account-usage/table_constraints)| OBJECT_VIEWER  
[TABLE_DML_HISTORY view](/sql-reference/account-usage/table_dml_history)| USAGE_VIEWER  
[TABLE_PRUNING_HISTORY view](/sql-reference/account-usage/table_pruning_history)| USAGE_VIEWER  
[TABLE_QUERY_PRUNING_HISTORY view](/sql-reference/account-usage/table_query_pruning_history)| USAGE_VIEWER  
[TABLE_STORAGE_METRICS view](/sql-reference/account-usage/table_storage_metrics)| USAGE_VIEWER  
[TABLES view](/sql-reference/account-usage/tables)| OBJECT_VIEWER  
[TAG_REFERENCES view](/sql-reference/account-usage/tag_references)| GOVERNANCE_VIEWER  
[TAGS view](/sql-reference/account-usage/tags)| OBJECT_VIEWER or GOVERNANCE_VIEWER  
[TASK_HISTORY view](/sql-reference/account-usage/task_history)| USAGE_VIEWER  
[TASKS view](/sql-reference/account-usage/tasks)| OBJECT_VIEWER  
[TRUST_CENTER_FINDINGS view](/sql-reference/account-usage/trust_center_findings)| SECURITY_VIEWER  
[USERS view](/sql-reference/account-usage/users)| SECURITY_VIEWER  
[VIEWS view](/sql-reference/account-usage/views)| OBJECT_VIEWER  
[WAREHOUSE_EVENTS_HISTORY view](/sql-reference/account-usage/warehouse_events_history)| USAGE_VIEWER  
[WAREHOUSE_LOAD_HISTORY view](/sql-reference/account-usage/warehouse_load_history)| USAGE_VIEWER  
[WAREHOUSE_METERING_HISTORY view](/sql-reference/account-usage/warehouse_metering_history)| USAGE_VIEWER  
  
### READER_ACCOUNT_USAGE schema SNOWFLAKE database roles¶

The READER_USAGE_VIEWER SNOWFLAKE database role is granted SELECT privilege on all READER_ACCOUNT_USAGE views. As reader accounts are created by clients, the READER_USAGE_VIEWER role is expected to be granted to those roles used to monitor reader account use.

View  
---  
[LOGIN_HISTORY view](/sql-reference/account-usage/login_history)  
[QUERY_HISTORY view](/sql-reference/account-usage/query_history)  
[RESOURCE_MONITORS view](/sql-reference/account-usage/resource_monitors)  
[STORAGE_USAGE view](/sql-reference/account-usage/storage_usage)  
[WAREHOUSE_METERING_HISTORY view](/sql-reference/account-usage/warehouse_metering_history)  
  
## Querying the Account Usage views¶

This section includes considerations when querying the Account Usage views along with query examples.

### Selecting columns¶

The Snowflake-specific views are subject to change. Avoid selecting all columns from these views. Instead, select the columns that you want. For example, if you want the `name` column, use `SELECT name`, rather than `SELECT *`.

### Reconciling cost views¶

There are several Account Usage views that contain data related to the cost of compute resources, storage, and data transfers. If you are trying to reconcile these views against a corresponding view in the [ORGANIZATION_USAGE schema](/sql-reference/organization-usage), you must first set the timezone of the session to UTC.

For example, if you are trying to reconcile ACCOUNT_USAGE.WAREHOUSE_METERING_HISTORY to the account’s data in ORGANIZATION_USAGE.WAREHOUSE_METERING_HISTORY, you must run the following command before querying the Account Usage view:
[code] 
    ALTER SESSION SET TIMEZONE = UTC;
    
[/code]

### Examples¶

The following examples show some typical/useful queries using the views in the ACCOUNT_USAGE schema.

Note

  * These examples assume the SNOWFLAKE database and the ACCOUNT_USAGE schema are in use for the current session. The examples also assume the ACCOUNTADMIN role (or a role granted IMPORTED PRIVILEGES on the database) is in use. If they are not in use, execute the following commands before running the queries in the examples:
[code] USE ROLE ACCOUNTADMIN;
        
        USE SCHEMA snowflake.account_usage;
        
[/code]




#### Examples: User login metrics¶

Average number of seconds between failed login attempts by user (month-to-date):

> 
[code]
>     select user_name,
>            count(*) as failed_logins,
>            avg(seconds_between_login_attempts) as average_seconds_between_login_attempts
>     from (
>           select user_name,
>                  timediff(seconds, event_timestamp, lead(event_timestamp)
>                      over(partition by user_name order by event_timestamp)) as seconds_between_login_attempts
>           from login_history
>           where event_timestamp > date_trunc(month, current_date)
>           and is_success = 'NO'
>          )
>     group by 1
>     order by 3;
>     
[/code]

Failed logins by user (month-to-date):

> 
[code]
>     select user_name,
>            sum(iff(is_success = 'NO', 1, 0)) as failed_logins,
>            count(*) as logins,
>            sum(iff(is_success = 'NO', 1, 0)) / nullif(count(*), 0) as login_failure_rate
>     from login_history
>     where event_timestamp > date_trunc(month, current_date)
>     group by 1
>     order by 4 desc;
>     
[/code]

Failed logins by user and connecting client (month-to-date):

> 
[code]
>     select reported_client_type,
>            user_name,
>            sum(iff(is_success = 'NO', 1, 0)) as failed_logins,
>            count(*) as logins,
>            sum(iff(is_success = 'NO', 1, 0)) / nullif(count(*), 0) as login_failure_rate
>     from login_history
>     where event_timestamp > date_trunc(month, current_date)
>     group by 1,2
>     order by 5 desc;
>     
[/code]

#### Examples: Warehouse performance¶

This query calculates virtual warehouse performance metrics such as throughput and latency for 15-minute time intervals over the course of one day.

In the code sample below, you can replace `CURRENT_WAREHOUSE()` with the name of a warehouse to calculate metrics for that warehouse. In addition, change the `time_from` and `time_to` dates in the WITH clause to specify the time period.

> 
[code]
>     WITH
>     params AS (
>     SELECT
>         CURRENT_WAREHOUSE() AS warehouse_name,
>         '2021-11-01' AS time_from,
>         '2021-11-02' AS time_to
>     ),
>     
>     jobs AS (
>     SELECT
>         query_id,
>         time_slice(start_time::timestamp_ntz, 15, 'minute','start') as interval_start,
>         qh.warehouse_name,
>         database_name,
>         query_type,
>         total_elapsed_time,
>         compilation_time AS compilation_and_scheduling_time,
>         (queued_provisioning_time + queued_repair_time + queued_overload_time) AS queued_time,
>         transaction_blocked_time,
>         execution_time
>     FROM snowflake.account_usage.query_history qh, params
>     WHERE
>         qh.warehouse_name = params.warehouse_name
>     AND start_time >= params.time_from
>     AND start_time <= params.time_to
>     AND execution_status = 'SUCCESS'
>     AND query_type IN ('SELECT','UPDATE','INSERT','MERGE','DELETE')
>     ),
>     
>     interval_stats AS (
>     SELECT
>         query_type,
>         interval_start,
>         COUNT(DISTINCT query_id) AS numjobs,
>         MEDIAN(total_elapsed_time)/1000 AS p50_total_duration,
>         (percentile_cont(0.95) within group (order by total_elapsed_time))/1000 AS p95_total_duration,
>         SUM(total_elapsed_time)/1000 AS sum_total_duration,
>         SUM(compilation_and_scheduling_time)/1000 AS sum_compilation_and_scheduling_time,
>         SUM(queued_time)/1000 AS sum_queued_time,
>         SUM(transaction_blocked_time)/1000 AS sum_transaction_blocked_time,
>         SUM(execution_time)/1000 AS sum_execution_time,
>         ROUND(sum_compilation_and_scheduling_time/sum_total_duration,2) AS compilation_and_scheduling_ratio,
>         ROUND(sum_queued_time/sum_total_duration,2) AS queued_ratio,
>         ROUND(sum_transaction_blocked_time/sum_total_duration,2) AS blocked_ratio,
>         ROUND(sum_execution_time/sum_total_duration,2) AS execution_ratio,
>         ROUND(sum_total_duration/numjobs,2) AS total_duration_perjob,
>         ROUND(sum_compilation_and_scheduling_time/numjobs,2) AS compilation_and_scheduling_perjob,
>         ROUND(sum_queued_time/numjobs,2) AS queued_perjob,
>         ROUND(sum_transaction_blocked_time/numjobs,2) AS blocked_perjob,
>         ROUND(sum_execution_time/numjobs,2) AS execution_perjob
>     FROM jobs
>     GROUP BY 1,2
>     ORDER BY 1,2
>     )
>     SELECT * FROM interval_stats;
>     
[/code]
> 
> Note
> 
> Analyze different statement types separately (e.g., SELECT statements independent of INSERT or DELETE or other statements).

  * The NUMJOBS value represents the throughput for that time interval.
  * The P50_TOTAL_DURATION (median) and P95_TOTAL_DURATION (peak) values represent latency.
  * The SUM_TOTAL_DURATION is the sum of the SUM_<job_stage>_TIME values for the different job stages (COMPILATION_AND_SCHEDULING, QUEUED, BLOCKED, EXECUTION).
  * Analyze the <job_stage>_RATIO values when the load (NUMJOBS) increases. Look for ratio changes or deviations from the average.
  * If the QUEUED_RATIO is high, there might not be sufficient capacity in the warehouse. Add more clusters or increase the warehouse size.



#### Examples: Warehouse credit usage¶

Credits used by each warehouse in your account (month-to-date):

> 
[code]
>     select warehouse_name,
>            sum(credits_used) as total_credits_used
>     from warehouse_metering_history
>     where start_time >= date_trunc(month, current_date)
>     group by 1
>     order by 2 desc;
>     
[/code]

Credits used over time by each warehouse in your account (month-to-date):

> 
[code]
>     select start_time::date as usage_date,
>            warehouse_name,
>            sum(credits_used) as total_credits_used
>     from warehouse_metering_history
>     where start_time >= date_trunc(month, current_date)
>     group by 1,2
>     order by 2,1;
>     
[/code]

#### Examples: Data storage usage¶

Billable terabytes stored in your account over time:

> 
[code]
>     select date_trunc(month, usage_date) as usage_month
>       , avg(storage_bytes + stage_bytes + failsafe_bytes) / power(1024, 4) as billable_tb
>     from storage_usage
>     group by 1
>     order by 1;
>     
[/code]

#### Examples: User query totals and execution times¶

Total jobs executed in your account (month-to-date):

> 
[code]
>     select count(*) as number_of_jobs
>     from query_history
>     where start_time >= date_trunc(month, current_date);
>     
[/code]

Total jobs executed by each warehouse in your account (month-to-date):

> 
[code]
>     select warehouse_name,
>            count(*) as number_of_jobs
>     from query_history
>     where start_time >= date_trunc(month, current_date)
>     group by 1
>     order by 2 desc;
>     
[/code]

Average query execution time by user (month-to-date):

> 
[code]
>     select user_name,
>            avg(execution_time) as average_execution_time
>     from query_history
>     where start_time >= date_trunc(month, current_date)
>     group by 1
>     order by 2 desc;
>     
[/code]

Average query execution time by query type and warehouse size (month-to-date):

> 
[code]
>     select query_type,
>            warehouse_size,
>            avg(execution_time) as average_execution_time
>     from query_history
>     where start_time >= date_trunc(month, current_date)
>     group by 1,2
>     order by 3 desc;
>     
[/code]

#### Examples: Obtain a query count for every login event¶

Join columns from LOGIN_HISTORY, QUERY_HISTORY, and SESSIONS to obtain a query count for each user login event.

> Note
> 
> The SESSIONS view records information starting on July 20-21, 2020, therefore the query result will only contain overlapping information for each of the three views starting from this date.
[code]
>     select l.user_name,
>            l.event_timestamp as login_time,
>            l.client_ip,
>            l.reported_client_type,
>            l.first_authentication_factor,
>            l.second_authentication_factor,
>            count(q.query_id)
>     from snowflake.account_usage.login_history l
>     join snowflake.account_usage.sessions s on l.event_id = s.login_event_id
>     join snowflake.account_usage.query_history q on q.session_id = s.session_id
>     group by 1,2,3,4,5,6
>     order by l.user_name
>     ;
>     
[/code]
