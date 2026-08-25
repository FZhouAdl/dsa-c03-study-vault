---
title: "SNOWFLAKE database roles | Snowflake Documentation"
source: https://docs.snowflake.com/sql-reference/snowflake-db-roles
cert_domain: domain-3-model-development
crawl_depth: 1
crawled: 2026-08-23
---

# SNOWFLAKE database roles¶

When an account is provisioned, the SNOWFLAKE database is automatically imported. The database is an example of Snowflake using [Secure Data Sharing](/user-guide/data-sharing-gs) to provide object metadata and other usage metrics for your organization and accounts.

Access to schema objects in the SNOWFLAKE database is controlled by different [database roles](/user-guide/security-access-control-considerations#label-access-control-considerations-database-roles). The following sections describe each SNOWFLAKE database role, its associated privileges, and the associated schema objects the role is granted access to.

## ACCOUNT_USAGE schema¶

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
  
## READER_ACCOUNT_USAGE schema¶

The READER_USAGE_VIEWER SNOWFLAKE database role is granted SELECT privilege on all READER_ACCOUNT_USAGE views. As reader accounts are created by clients, the READER_USAGE_VIEWER role is expected to be granted to those roles used to monitor reader account use.

View  
---  
[LOGIN_HISTORY view](/sql-reference/account-usage/login_history)  
[QUERY_HISTORY view](/sql-reference/account-usage/query_history)  
[RESOURCE_MONITORS view](/sql-reference/account-usage/resource_monitors)  
[STORAGE_USAGE view](/sql-reference/account-usage/storage_usage)  
[WAREHOUSE_METERING_HISTORY view](/sql-reference/account-usage/warehouse_metering_history)  
  
## ORGANIZATION_USAGE schema¶

The ORGANIZATION_USAGE_VIEWER, ORGANIZATION_BILLING_VIEWER, and ORGANIZATION_ACCOUNTS_VIEWER SNOWFLAKE database roles are granted the SELECT privilege on Organization Usage views in the shared SNOWFLAKE database.

View| ORGANIZATION_BILLING_VIEWER Role| ORGANIZATION_USAGE_VIEWER Role| ORGANIZATION_ACCOUNTS_VIEWER Role  
---|---|---|---  
[ACCOUNTS view](/sql-reference/organization-usage/accounts)| | | ✔  
[ANOMALIES_IN_CURRENCY_DAILY view](/sql-reference/organization-usage/anomalies_in_currency_daily)| ✔| |   
[CONTRACT_ITEMS view](/sql-reference/organization-usage/contract_items)| ✔| |   
[LISTING_AUTO_FULFILLMENT_USAGE_HISTORY view](/sql-reference/organization-usage/listing_auto_fulfillment_usage_history)| ✔| |   
[RATE_SHEET_DAILY view](/sql-reference/organization-usage/rate_sheet_daily)| ✔| |   
[REMAINING_BALANCE_DAILY view](/sql-reference/organization-usage/remaining_balance_daily)| ✔| |   
[USAGE_IN_CURRENCY_DAILY view](/sql-reference/organization-usage/usage_in_currency_daily)| ✔| |   
[MARKETPLACE_DISBURSEMENT_REPORT View](/collaboration/views/marketplace-disbursement-report-org)| ✔| |   
[DATA_TRANSFER_DAILY_HISTORY view](/sql-reference/organization-usage/data_transfer_daily_history)| | ✔|   
[DATA_TRANSFER_HISTORY view](/sql-reference/organization-usage/data_transfer_history)| | ✔|   
[DATABASE_STORAGE_USAGE_HISTORY view](/sql-reference/organization-usage/database_storage_usage_history)| | ✔|   
[AUTOMATIC_CLUSTERING_HISTORY view](/sql-reference/organization-usage/automatic_clustering_history)| | ✔|   
[MARKETPLACE_PAID_USAGE_DAILY View](/collaboration/views/marketplace-paid-usage-daily-org)| | ✔|   
[MATERIALIZED_VIEW_REFRESH_HISTORY view](/sql-reference/account-usage/materialized_view_refresh_history)| | ✔|   
[METERING_DAILY_HISTORY view](/sql-reference/organization-usage/metering_daily_history)| | ✔|   
[MONETIZED_USAGE_DAILY View](/collaboration/views/monetized-usage-daily-org)| | ✔|   
[PIPE_USAGE_HISTORY view](/sql-reference/organization-usage/pipe_usage_history)| | ✔|   
[QUERY_ACCELERATION_HISTORY view](/sql-reference/organization-usage/query_acceleration_history)| | ✔|   
[REPLICATION_GROUP_USAGE_HISTORY view](/sql-reference/organization-usage/replication_group_usage_history)| | ✔|   
[REPLICATION_USAGE_HISTORY view](/sql-reference/organization-usage/replication_usage_history)| | ✔|   
[SEARCH_OPTIMIZATION_HISTORY view](/sql-reference/organization-usage/search_optimization_history)| | ✔|   
[STAGE_STORAGE_USAGE_HISTORY view](/sql-reference/organization-usage/stage_storage_usage_history)| | ✔|   
[STORAGE_DAILY_HISTORY view](/sql-reference/organization-usage/storage_daily_history)| | ✔|   
[WAREHOUSE_METERING_HISTORY view](/sql-reference/organization-usage/warehouse_metering_history)| | ✔|   
  
## CORE schema¶

The CORE_VIEWER SNOWFLAKE database role is granted to the PUBLIC role in all Snowflake accounts containing a shared SNOWFLAKE database. The USAGE privilege is granted to all Snowflake-defined functions and bundles in the CORE schema.

### Budget class¶

The BUDGET_CREATOR Snowflake database role is granted the USAGE privilege on the SNOWFLAKE.CORE schema and the BUDGET class in the schema. This grant allows users with the BUDGET_CREATOR role to create instances of the BUDGET class.

For more information, see [Create a custom role to create budgets](/user-guide/budgets/custom-budget#label-custom-budgets-owner-role).

### Tag objects¶

The CORE_VIEWER database role is granted the APPLY privilege on the [classification system tags](/user-guide/classify-intro#label-classify-classification-tags) SNOWFLAKE.CORE.PRIVACY_CATEGORY and SNOWFLAKE.CORE.SEMANTIC_CATEGORY. These grants allow users with a role that is granted the CORE_VIEWER database role to assign these system tags to columns.

## ALERT schema¶

The ALERT_VIEWER SNOWFLAKE database role is granted the USAGE privilege on the functions defined in this schema.

## ML schema¶

The ML_USER SNOWFLAKE database role is granted to the PUBLIC role in all Snowflake accounts that contain a shared SNOWFLAKE database and allows customers to access and use [ML functions](/guides-overview-ml-functions). Users must also have the USAGE privilege on the ML schema to call these functions.

## MONITORING schema¶

The MONITORING_VIEWER database role has the SELECT privilege on all views in the MONITORING schema.

The MONITORING_VIEWER database role is granted to the PUBLIC role in all Snowflake accounts containing a shared SNOWFLAKE database.

## SNOWFLAKE.CLASSIFICATION_ADMIN database role¶

The SNOWFLAKE.CLASSIFICATION_ADMIN database role allows a data engineer or steward to create an instance of the [CLASSIFICATION_PROFILE](/sql-reference/classes/classification_profile) class. A classification profile is used to implement [sensitive data classification](/user-guide/classify-auto).

## SNOWFLAKE.CORTEX_AGENT_USER database role¶

You can use the SNOWFLAKE.CORTEX_AGENT_USER database role to grant your users access to Snowflake Cortex Agents API without granting access to other Cortex features. Using the Cortex Agents API requires _either_ the SNOWFLAKE.CORTEX_USER database role _or_ the SNOWFLAKE.CORTEX_AGENT_USER database role.

By default, the SNOWFLAKE.CORTEX_USER database role is granted to the PUBLIC role. For fine-grained access control, revoke access from the PUBLIC role and grant access to the SNOWFLAKE.CORTEX_AGENT_USER database role. For more information, see [Limiting access to specific roles](/user-guide/snowflake-cortex/cortex-agents-setup#label-snowflake-agents-access).

## SNOWFLAKE.AI_FUNCTIONS_USER database role¶

The SNOWFLAKE.AI_FUNCTIONS_USER database role is used to grant customers access to Snowflake Cortex scalar AI functions (all Cortex AI functions except the aggregate functions AI_AGG and AI_SUMMARIZE_AGG) without granting access to Cortex services such as Cortex Agent, Cortex Analyst, Cortex Fine-tuning, or Cortex Search. Calling scalar AI functions requires _either_ the SNOWFLAKE.CORTEX_USER database role _or_ the SNOWFLAKE.AI_FUNCTIONS_USER database role.

By default, this role is not granted to any roles. If you want users to have access to scalar AI functions, grant this database role to appropriate roles. For details, see [Cortex LLM Functions required privileges](/user-guide/snowflake-cortex/aisql-privileges-and-access#label-cortex-llm-privileges).

## SNOWFLAKE.CORTEX_EMBED_USER database role¶

The SNOWFLAKE.CORTEX_EMBED_USER database role is used to grant customers access to Snowflake Cortex embedding functions AI_EMBED, AI_MULTI_EMBED, SNOWFLAKE.CORTEX.EMBED_768, and SNOWFLAKE.CORTEX_EMBED_TEXT_1024 without granting access to other Cortex features. Calling these embedding functions requires _either_ the SNOWFLAKE.CORTEX_USER database role _or_ the SNOWFLAKE.CORTEX_EMBED_USER database role. This role is not granted to any roles by default.

By default, this role is not granted to any roles. If you want users to have access to the embedding functions, grant this database role to appropriate roles. For details, see [Cortex LLM Functions required privileges](/user-guide/snowflake-cortex/aisql-privileges-and-access#label-cortex-llm-privileges)

## SNOWFLAKE.CORTEX_REST_API_USER database role¶

The SNOWFLAKE.CORTEX_REST_API_USER database role is used to grant customers access to the Snowflake Cortex REST API without granting access to other Cortex features such as Cortex AI functions, Cortex Agent, Cortex Analyst, Cortex Fine-tuning, or Cortex Search. Using the Cortex REST API requires _either_ the SNOWFLAKE.CORTEX_USER database role _or_ the SNOWFLAKE.CORTEX_REST_API_USER database role.

By default, this role is not granted to any roles. If you want users to have access to the Cortex REST API without granting broader Cortex privileges, grant this database role to appropriate roles. For details, see [Limiting access using the Cortex REST API user role](/user-guide/snowflake-cortex/cortex-rest-api#label-cortex-rest-api-user-role).

## SNOWFLAKE.CORTEX_USER database role¶

This SNOWFLAKE.CORTEX_USER database role is used to grant customers access to Snowflake Cortex features. By default, this role is granted to the PUBLIC role. The PUBLIC role is automatically granted to all users and roles, so this allows all users in your account to use Snowflake Cortex LLM functions.

If you don’t want all users to have this privilege, you can revoke access from the PUBLIC role and grant access to specific roles. For details, see [Cortex LLM Functions required privileges](/user-guide/snowflake-cortex/aisql-privileges-and-access#label-cortex-llm-privileges).

## SNOWFLAKE.COPILOT_USER database role¶

The SNOWFLAKE.COPILOT_USER database role allows customers to access Cortex Code features in Snowsight. Initially, this database role is granted to the PUBLIC role. The PUBLIC role is automatically granted to all users and roles, so this allows all users in your account to use Cortex Code. If you want to limit access to Cortex Code features in Snowsight, you can revoke access from the PUBLIC role and grant access to specific roles. For details, see [Access control requirements](/user-guide/cortex-code/cortex-code-snowsight#label-cortex-code-snowsight-access-control).

## Using SNOWFLAKE database roles¶

Administrators can use the [GRANT DATABASE ROLE](/sql-reference/sql/grant-database-role) to assign a SNOWFLAKE database role to another role, which can then be granted to a user. This would allow the user to access a specific subset of views in the SNOWFLAKE database.

In the following example a role is created which can be used to view SNOWFLAKE database object metadata, and does the following:

  1. Creates a custom role.
  2. Grants the OBJECT_VIEWER role to the custom role.
  3. Grants the custom role to a user.



To create and grant the custom role, do the following:

  1. Create the `CAN_VIEWMD` role, using [CREATE ROLE](/sql-reference/sql/create-role) that will be used to grant access to object metadata.

Only users with the USERADMIN system role or higher, or another role with the CREATE ROLE privilege on the account, can create roles.
[code] CREATE ROLE CAN_VIEWMD COMMENT = 'This role can view metadata per SNOWFLAKE database role definitions';
         
[/code]

  2. Grant the OBJECT_VIEWER role to the CAN_VIEWMD role.

Only users with the OWNERSHIP role can grant SNOWFLAKE database roles. For additional information, refer to [GRANT DATABASE ROLE](/sql-reference/sql/grant-database-role).
[code] GRANT DATABASE ROLE OBJECT_VIEWER TO ROLE CAN_VIEWMD;
         
[/code]

  3. Assign `CAN_VIEWMD` role to user `smith`.

Only users with the SECURITYADMIN role can grant roles to users. For additional options, refer to [GRANT ROLE](/sql-reference/sql/grant-role).
[code] GRANT ROLE CAN_VIEWMD TO USER smith;
         
[/code]
