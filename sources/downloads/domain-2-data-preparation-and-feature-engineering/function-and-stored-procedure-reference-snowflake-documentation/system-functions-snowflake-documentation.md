---
title: "System functions | Snowflake Documentation"
source: https://docs.snowflake.com/sql-reference/functions-system
cert_domain: domain-2-data-preparation-and-feature-engineering
crawl_depth: 1
crawled: 2026-08-23
---

# System functions¶

Snowflake provides the following types of system functions:

  * Control functions that allow you to execute actions in the system (for example, aborting a query).
  * Information functions that return information about the system (for example, calculating the clustering depth of a table).
  * Information functions that return information about queries (for example, information about EXPLAIN plans).



Many of these system functions have the prefix `SYSTEM$` (for example, `SYSTEM$TYPEOF`). For the system functions that use this prefix, you must specify the prefix when calling the function. For example:
[code] 
    SELECT SYSTEM$TYPEOF('a');
    
[/code]

Function Name| Notes  
---|---  
**Control**|   
[EXECUTE_AI_EVALUATION](/sql-reference/functions/execute_ai_evaluation)|   
[SYSTEM$ABORT_SESSION](/sql-reference/functions/system_abort_session)|   
[SYSTEM$ABORT_TRANSACTION](/sql-reference/functions/system_abort_transaction)|   
[SYSTEM$ACTIVATE_CMK_INFO](/sql-reference/functions/system_activate_cmk_info)|   
[SYSTEM$ACTIVATE_CMK_INFO_POSTGRES](/sql-reference/functions/system_activate_cmk_info_postgres)|   
[SYSTEM$ADD_EVENT (for Snowflake Scripting)](/sql-reference/functions/system_add_event)|   
[SYSTEM$ADD_PRIVATELINK_ENDPOINT_HOSTNAME](/sql-reference/functions/system_add_privatelink_endpoint_hostname)|   
[SYSTEM$ADD_REFERENCE](/sql-reference/functions/system_add_reference)|   
[SYSTEM$AUTHORIZE_PRIVATELINK](/sql-reference/functions/system_authorize_privatelink)|   
[SYSTEM$AUTHORIZE_STAGE_PRIVATELINK_ACCESS](/sql-reference/functions/system_authorize_stage_privatelink_access)|   
[SYSTEM$AUTHORIZE_SNOWFLAKE_MANAGED_STORAGE_VOLUME_PRIVATELINK_ACCESS](/sql-reference/functions/system_authorize_snowflake_managed_storage_volume_privatelink_access)|   
[SYSTEM$BEGIN_DEBUG_APPLICATION](/sql-reference/functions/system_begin_debug_application)|   
[SYSTEM$BLOCK_INTERNAL_STAGES_PUBLIC_ACCESS](/sql-reference/functions/system_block_internal_stages_public_access)|   
[SYSTEM$BLOCK_INTERNAL_STAGES_PUBLIC_ACCESS_WITH_EXCEPTION](/sql-reference/functions/system_block_internal_stages_public_access_with_exception)|   
[SYSTEM$BLOCK_SNOWFLAKE_MANAGED_STORAGE_VOLUME_PUBLIC_ACCESS](/sql-reference/functions/system_block_snowflake_managed_storage_volume_public_access)|   
[SYSTEM$BLOCK_SNOWFLAKE_MANAGED_STORAGE_VOLUME_PUBLIC_ACCESS_WITH_EXCEPTION](/sql-reference/functions/system_block_snowflake_managed_storage_volume_public_access_with_exception)|   
[SYSTEM$CANCEL_ALL_QUERIES](/sql-reference/functions/system_cancel_all_queries)|   
[SYSTEM$CANCEL_QUERY](/sql-reference/functions/system_cancel_query)|   
[SYSTEM$CLEANUP_DATABASE_ROLE_GRANTS](/sql-reference/functions/system_cleanup_database_role_grants)|   
[SYSTEM$COMMIT_MOVE_ORGANIZATION_ACCOUNT](/sql-reference/functions/system_commit_move_organization_account)|   
[SYSTEM$CONVERT_PIPES_SQS_TO_SNS](/sql-reference/functions/system_convert_pipes_sqs_to_sns)|   
[SYSTEM$CREATE_BILLING_EVENT](/sql-reference/functions/system_create_billing_event)|   
[SYSTEM$CREATE_BILLING_EVENTS](/sql-reference/functions/system_create_billing_events)|   
[SYSTEM$CREATE_EVALUATION_DATASET](/sql-reference/functions/system_create_evaluation_dataset)|   
[SYSTEM$DEACTIVATE_CMK_INFO](/sql-reference/functions/system_deactivate_cmk_info)|   
[SYSTEM$DEPROVISION_PRIVATELINK_ENDPOINT](/sql-reference/functions/system_deprovision_privatelink_endpoint)|   
[SYSTEM$DEPROVISION_PRIVATELINK_ENDPOINT_TSS](/sql-reference/functions/system_deprovision_privatelink_endpoint_tss)|   
[SYSTEM$DEREGISTER_CMK_INFO](/sql-reference/functions/system_deregister_cmk_info)|   
[SYSTEM$DEREGISTER_CMK_INFO_POSTGRES](/sql-reference/functions/system_deregister_cmk_info_postgres)|   
[SYSTEM$DISABLE_BEHAVIOR_CHANGE_BUNDLE](/sql-reference/functions/system_disable_behavior_change_bundle)|   
[SYSTEM$DISABLE_DATABASE_REPLICATION](/sql-reference/functions/system_disable_database_replication)|   
[SYSTEM$DISABLE_FEATURE_GROUPS](/sql-reference/functions/system_disable_feature_groups)|   
[SYSTEM$DISABLE_FEATURE_GROUPS_PREVIEW](/sql-reference/functions/system_disable_feature_groups_preview)|   
[SYSTEM$DISABLE_GLOBAL_DATA_SHARING_FOR_ACCOUNT](/sql-reference/functions/system_disable_global_data_sharing_for_account)|   
[SYSTEM$DISABLE_PREVIEW_ACCESS](/sql-reference/functions/system_disable_preview_access)|   
[SYSTEM$DISABLE_PRIVATELINK_ACCESS_ONLY](/sql-reference/functions/system_disable_privatelink_access_only)|   
[SYSTEM$ENABLE_BEHAVIOR_CHANGE_BUNDLE](/sql-reference/functions/system_enable_behavior_change_bundle)|   
[SYSTEM$ENABLE_FEATURE_GROUPS](/sql-reference/functions/system_enable_feature_groups)|   
[SYSTEM$ENABLE_GLOBAL_DATA_SHARING_FOR_ACCOUNT](/sql-reference/functions/system_enable_global_data_sharing_for_account)|   
[SYSTEM$ENABLE_PREVIEW_ACCESS](/sql-reference/functions/system_enable_preview_access)|   
[SYSTEM$END_DEBUG_APPLICATION](/sql-reference/functions/system_end_debug_application)|   
[SYSTEM$ENFORCE_PRIVATELINK_ACCESS_ONLY](/sql-reference/functions/system_enforce_privatelink_access_only)|   
[SYSTEM$FINISH_OAUTH_FLOW](/sql-reference/functions/system_finish_oauth_flow)|   
[SYSTEM$GLOBAL_ACCOUNT_SET_PARAMETER](/sql-reference/functions/system_global_account_set_parameter)|   
[SYSTEM$INITIATE_MOVE_ORGANIZATION_ACCOUNT](/sql-reference/functions/system_initiate_move_organization_account)|   
[SYSTEM$ISSUE_PER_ACCOUNT_APP_SERVICE_CERTIFICATE](/sql-reference/functions/system_issue_per_account_app_service_certificate)|   
[SYSTEM$ISSUE_WORKLOAD_IDENTITY_FEDERATION_TOKEN](/sql-reference/functions/system_issue_workload_identity_federation_token)|   
[SYSTEM$LINK_ACCOUNT_OBJECTS_BY_NAME](/sql-reference/functions/system_link_account_objects_by_name)|   
[SYSTEM$LINK_ORGANIZATION_USER](/sql-reference/functions/system_link_organization_user)|   
[SYSTEM$LINK_ORGANIZATION_USER_GROUP](/sql-reference/functions/system_link_organization_user_group)|   
[SYSTEM$MIGRATE_SAML_IDP_REGISTRATION](/sql-reference/functions/system_migrate_saml_idp_registration)|   
[SYSTEM$OPT_IN_INTERNAL_STAGE_NETWORK_LOGS](/sql-reference/functions/system_opt_in_internal_stage_network_logs)|   
[SYSTEM$OPT_OUT_INTERNAL_STAGE_NETWORK_LOGS](/sql-reference/functions/system_opt_out_internal_stage_network_logs)|   
[SYSTEM$OPT_OUT_MALICIOUS_IP_PROTECTION_BY_CATEGORY](/sql-reference/functions/system_opt_out_malicious_ip_protection_by_category)|   
[SYSTEM$PIPE_FORCE_RESUME](/sql-reference/functions/system_pipe_force_resume)|   
[SYSTEM$PIPE_REBINDING_WITH_NOTIFICATION_CHANNEL](/sql-reference/functions/system_pipe_rebinding_with_notification_channel)|   
[SYSTEM$PROVISION_PRIVATELINK_ENDPOINT](/sql-reference/functions/system_provision_privatelink_endpoint)|   
[SYSTEM$PROVISION_PRIVATELINK_ENDPOINT_TSS](/sql-reference/functions/system_provision_privatelink_endpoint_tss)|   
[SYSTEM$REGISTER_CMK_INFO](/sql-reference/functions/system_register_cmk_info)|   
[SYSTEM$REGISTER_CMK_INFO_POSTGRES](/sql-reference/functions/system_register_cmk_info_postgres)|   
[SYSTEM$REGISTER_PRIVATELINK_ENDPOINT](/sql-reference/functions/system_register_privatelink_endpoint)|   
[SYSTEM$REMOVE_ALL_REFERENCES](/sql-reference/functions/system_remove_all_references)|   
[SYSTEM$REMOVE_PRIVATELINK_ENDPOINT_HOSTNAME](/sql-reference/functions/system_remove_privatelink_endpoint_hostname)|   
[SYSTEM$REMOVE_REFERENCE](/sql-reference/functions/system_remove_reference)|   
[SYSTEM$RESTORE_PRIVATELINK_ENDPOINT](/sql-reference/functions/system_restore_privatelink_endpoint)|   
[SYSTEM$RESTORE_PRIVATELINK_ENDPOINT_TSS](/sql-reference/functions/system_restore_privatelink_endpoint_tss)|   
[SYSTEM$REVOKE_PRIVATELINK](/sql-reference/functions/system_revoke_privatelink)|   
[SYSTEM$REVOKE_STAGE_PRIVATELINK_ACCESS](/sql-reference/functions/system_revoke_stage_privatelink_access)|   
[SYSTEM$REVOKE_SNOWFLAKE_MANAGED_STORAGE_VOLUME_PRIVATELINK_ACCESS](/sql-reference/functions/system_revoke_snowflake_managed_storage_volume_privatelink_access)|   
[SYSTEM$SCHEDULE_ASYNC_REPLICATION_GROUP_REFRESH](/sql-reference/functions/system_schedule_async_replication_group_refresh)|   
[SYSTEM$SEND_NOTIFICATIONS_TO_CATALOG](/sql-reference/functions/system_send_notifications_to_catalog)|   
[SYSTEM$SET_APPLICATION_RESTRICTED_FEATURE_ACCESS](/sql-reference/functions/system_set_application_restricted_feature_access)|   
[SYSTEM$SET_CATALOG_INTEGRATION](/sql-reference/functions/system_set_catalog_integration)|   
[SYSTEM$SET_DEFAULT_COLUMNS_OVERRIDE_FOR_SHOW_COMMAND](/sql-reference/functions/system_set_default_columns_override_for_show_command)|   
[SYSTEM$SET_DEFAULT_COLUMNS_OVERRIDE_FOR_SYSTEM_OBJECT](/sql-reference/functions/system_set_default_columns_override_for_system_object)|   
[SYSTEM$SET_EVENT_SHARING_ACCOUNT_FOR_REGION](/sql-reference/functions/system_set_event_sharing_account_for_region)|   
[SYSTEM$SET_PRIVATELINK_ENDPOINT_HOSTNAME](/sql-reference/functions/system_set_privatelink_endpoint_hostname)|   
[SYSTEM$SET_REFERENCE](/sql-reference/functions/system_set_reference)|   
[SYSTEM$SET_ROW_TIMESTAMP_ON_ALL_SUPPORTED_TABLES](/sql-reference/functions/system_set_row_timestamp_on_all_supported_tables)|   
[SYSTEM$SNOWFLAKE_MANAGED_STORAGE_VOLUME_PUBLIC_ACCESS_STATUS](/sql-reference/functions/system_snowflake_managed_storage_volume_public_access_status)|   
[SYSTEM$SNOWPIPE_STREAMING_UPDATE_CHANNEL_OFFSET_TOKEN](/sql-reference/functions/system_snowpipe_streaming_update_channel_offset_token)|   
[SYSTEM$START_OAUTH_FLOW](/sql-reference/functions/system_start_oauth_flow)|   
[SYSTEM$START_USER_EMAIL_VERIFICATION](/sql-reference/functions/system_start_user_email_verification)|   
[SYSTEM$TASK_DEPENDENTS_ENABLE](/sql-reference/functions/system_task_dependents_enable)|   
[SYSTEM$TRIGGER_LISTING_REFRESH](/sql-reference/functions/system_trigger_listing_refresh)|   
[SYSTEM$UNBLOCK_INTERNAL_STAGES_PUBLIC_ACCESS](/sql-reference/functions/system_unblock_internal_stages_public_access)|   
[SYSTEM$UNBLOCK_SNOWFLAKE_MANAGED_STORAGE_VOLUME_PUBLIC_ACCESS](/sql-reference/functions/system_unblock_snowflake_managed_storage_volume_public_access)|   
[SYSTEM$UNLINK_ORGANIZATION_USER](/sql-reference/functions/system_unlink_organization_user)|   
[SYSTEM$UNLINK_ORGANIZATION_USER_GROUP](/sql-reference/functions/system_unlink_organization_user_group)|   
[SYSTEM$UNREGISTER_PRIVATELINK_ENDPOINT](/sql-reference/functions/system_unregister_privatelink_endpoint)|   
[SYSTEM$UNSET_DEFAULT_COLUMNS_OVERRIDE_FOR_SHOW_COMMAND](/sql-reference/functions/system_unset_default_columns_override_for_show_command)|   
[SYSTEM$UNSET_DEFAULT_COLUMNS_OVERRIDE_FOR_SYSTEM_OBJECT](/sql-reference/functions/system_unset_default_columns_override_for_system_object)|   
[SYSTEM$UNSET_EVENT_SHARING_ACCOUNT_FOR_REGION](/sql-reference/functions/system_unset_event_sharing_account_for_region)|   
[SYSTEM$UNVERIFY_DNS_DOMAIN](/sql-reference/functions/system_unverify_dns_domain)|   
[SYSTEM$USER_TASK_CANCEL_ONGOING_EXECUTIONS](/sql-reference/functions/system_user_task_cancel_ongoing_executions)|   
[SYSTEM$VERIFY_DNS_DOMAIN](/sql-reference/functions/system_verify_dns_domain)|   
[SYSTEM$WAIT](/sql-reference/functions/system_wait)|   
**Information**|   
[EXTRACT_SEMANTIC_CATEGORIES](/sql-reference/functions/extract_semantic_categories)|   
[GET_ANACONDA_PACKAGES_REPODATA](/sql-reference/functions/get_anaconda_packages_repodata)|   
[SHOW_PYTHON_PACKAGES_DEPENDENCIES](/sql-reference/functions/show_python_packages_dependencies)|   
[SYSTEM$ALLOWLIST](/sql-reference/functions/system_allowlist)|   
[SYSTEM$ALLOWLIST_PRIVATELINK](/sql-reference/functions/system_allowlist_privatelink)|   
[SYSTEM$APP_COMPATIBILITY_CHECK](/sql-reference/functions/system_app_compatibility_check)|   
[SYSTEM$APPLICATION_GET_LOG_LEVEL](/sql-reference/functions/system_application_get_log_level)|   
[SYSTEM$APPLICATION_GET_METRIC_LEVEL](/sql-reference/functions/system_application_get_metric_level)|   
[SYSTEM$APPLICATION_GET_TRACE_LEVEL](/sql-reference/functions/system_application_get_trace_level)|   
[SYSTEM$AUTO_REFRESH_STATUS](/sql-reference/functions/system_auto_refresh_status)|   
[SYSTEM$BEHAVIOR_CHANGE_BUNDLE_STATUS](/sql-reference/functions/system_behavior_change_bundle_status)|   
[SYSTEM$CATALOG_LINK_STATUS](/sql-reference/functions/system_catalog_link_status)|   
[SYSTEM$CKE_HASH_FUNCTION](/sql-reference/functions/system_cke_hash_function)|   
[SYSTEM$CLIENT_VERSION_INFO](/sql-reference/functions/system_client_version_info)|   
[SYSTEM$CLIENT_VULNERABILITY_INFO](/sql-reference/functions/system_client_vulnerability_info)|   
[SYSTEM$CLUSTERING_DEPTH](/sql-reference/functions/system_clustering_depth)|   
[SYSTEM$CLUSTERING_INFORMATION](/sql-reference/functions/system_clustering_information)|   
[SYSTEM$CLUSTERING_RATIO](/sql-reference/functions/system_clustering_ratio)| Deprecated; use the other clustering functions instead.  
[SYSTEM$CURRENT_USER_TASK_NAME](/sql-reference/functions/system_current_user_task_name)|   
[SYSTEM$DATA_METRIC_SCAN](/sql-reference/functions/system_data_metric_scan)|   
[SYSTEM$DATABASE_REFRESH_HISTORY](/sql-reference/functions/system_database_refresh_history)| Deprecated; use [DATABASE_REFRESH_HISTORY](/sql-reference/functions/database_refresh_history) instead.  
[SYSTEM$DATABASE_REFRESH_PROGRESS, SYSTEM$DATABASE_REFRESH_PROGRESS_BY_JOB](/sql-reference/functions/system_database_refresh_progress)| Deprecated; use [DATABASE_REFRESH_PROGRESS , DATABASE_REFRESH_PROGRESS_BY_JOB](/sql-reference/functions/database_refresh_progress) instead.  
[SYSTEM$DECODE_PAT](/sql-reference/functions/system_decode_pat)|   
[SYSTEM$DESC_ICEBERG_ACCESS_IDENTITY](/sql-reference/functions/system_desc_iceberg_access_identity)|   
[SYSTEM$ENCODE_CKE_PRIMARY_KEY](/sql-reference/functions/system_encode_cke_primary_key)|   
[SYSTEM$ESTIMATE_AUTOMATIC_CLUSTERING_COSTS](/sql-reference/functions/system_estimate_automatic_clustering_costs)|   
[SYSTEM$ESTIMATE_SEARCH_OPTIMIZATION_COSTS](/sql-reference/functions/system_estimate_search_optimization_costs)|   
[SYSTEM$EVALUATE_DATA_QUALITY_EXPECTATIONS](/sql-reference/functions/system_evaluate_data_quality_expectations)|   
[SYSTEM$EVALUATE_DATA_QUALITY_EXPECTATIONS_PERSIST_RESULT](/sql-reference/functions/system_evaluate_data_quality_expectations_persist_result)|   
[SYSTEM$EXECUTE_CATALOG_OPERATION](/sql-reference/functions/system_execute_catalog_operation)|   
[EXPLAIN_PRIVILEGES](/sql-reference/functions/explain_privileges)|   
[SYSTEM$EXPORT_TDS_FROM_SEMANTIC_VIEW](/sql-reference/functions/system_export_tds_from_semantic_view)|   
[SYSTEM$EXTERNAL_TABLE_PIPE_STATUS](/sql-reference/functions/system_external_table_pipe_status)|   
[SYSTEM$GENERATE_SAML_CSR](/sql-reference/functions/system_generate_saml_csr)|   
[SYSTEM$GENERATE_SCIM_ACCESS_TOKEN](/sql-reference/functions/system_generate_scim_access_token)|   
[SYSTEM$GET_ALERT_TEMPLATE](/sql-reference/functions/system_get_alert_template)|   
[SYSTEM$GET_ALL_DEFAULT_COLUMNS_OVERRIDES](/sql-reference/functions/system_get_all_default_columns_overrides)|   
[SYSTEM$GET_ALL_REFERENCES](/sql-reference/functions/system_get_all_references)|   
[SYSTEM$GET_AWS_SNS_IAM_POLICY](/sql-reference/functions/system_get_aws_sns_iam_policy)|   
[SYSTEM$GET_CATALOG_LINKED_DATABASE_CONFIG](/sql-reference/functions/system_get_catalog_linked_database_config)|   
[SYSTEM$GET_CLASSIFICATION_RESULT](/sql-reference/functions/system_get_classification_result)|   
[SYSTEM$GET_CMK_AKV_CONSENT_URL](/sql-reference/functions/system_get_cmk_akv_consent_url)|   
[SYSTEM$GET_CMK_CONFIG](/sql-reference/functions/system_get_cmk_config)|   
[SYSTEM$GET_CMK_CONFIG_POSTGRES](/sql-reference/functions/system_get_cmk_config_postgres)|   
[SYSTEM$GET_CMK_INFO](/sql-reference/functions/system_get_cmk_info)|   
[SYSTEM$GET_CMK_INFO_POSTGRES](/sql-reference/functions/system_get_cmk_info_postgres)|   
[SYSTEM$GET_CMK_KMS_KEY_POLICY](/sql-reference/functions/system_get_cmk_kms_key_policy)|   
[SYSTEM$GET_COMPUTE_POOL_PENDING_MAINTENANCE](/sql-reference/functions/system_get_compute_pool_pending_maintenance)|   
[SYSTEM$GET_DBT_LOG](/sql-reference/functions/system_get_dbt_log)|   
[SYSTEM$GET_DEBUG_STATUS](/sql-reference/functions/system_get_debug_status)|   
[SYSTEM$GET_DEFAULT_COLUMNS_OVERRIDE_FOR_SHOW_COMMAND](/sql-reference/functions/system_get_default_columns_override_for_show_command)|   
[SYSTEM$GET_DEFAULT_COLUMNS_OVERRIDE_FOR_SYSTEM_OBJECT](/sql-reference/functions/system_get_default_columns_override_for_system_object)|   
[SYSTEM$GET_DIRECTORY_TABLE_STATUS](/sql-reference/functions/system_get_directory_table_status)|   
[SYSTEM$GET_GCP_KMS_CMK_GRANT_ACCESS_CMD](/sql-reference/functions/system_get_gcp_kms_cmk_grant_access_cmd)|   
[SYSTEM$GET_HASH_FOR_APPLICATION](/sql-reference/functions/system_get_hash_for_application)|   
[SYSTEM$GET_ICEBERG_TABLE_INFORMATION](/sql-reference/functions/system_get_iceberg_table_information)|   
[SYSTEM$GET_INSTANCE_FAMILY_PLACEMENT_GROUPS](/sql-reference/functions/system_get_instance_family_placement_groups)|   
[SYSTEM$GET_LOGIN_FAILURE_DETAILS](/sql-reference/functions/system_get_login_failure_details)|   
[SYSTEM$GET_PREDECESSOR_RETURN_VALUE](/sql-reference/functions/system_get_predecessor_return_value)|   
[SYSTEM$GET_PREVIEW_ACCESS_STATUS](/sql-reference/functions/system_get_preview_access_status)|   
[SYSTEM$GET_PRIVATELINK](/sql-reference/functions/system_get_privatelink)|   
[SYSTEM$GET_PRIVATELINK_AUTHORIZED_ENDPOINTS](/sql-reference/functions/system_get_privatelink_authorized_endpoints)|   
[SYSTEM$GET_PRIVATELINK_CONFIG](/sql-reference/functions/system_get_privatelink_config)|   
[SYSTEM$GET_PRIVATELINK_ENDPOINTS_INFO](/sql-reference/functions/system_get_privatelink_endpoints_info)|   
[SYSTEM$GET_PRIVATELINK_ENDPOINT_REGISTRATIONS](/sql-reference/functions/system_get_privatelink_endpoint_registrations)|   
[SYSTEM$GET_PURCHASE_ATTRIBUTES](/sql-reference/functions/system_get_purchase_attributes)|   
[SYSTEM$GET_REFERENCED_OBJECT_ID_HASH](/sql-reference/functions/system_get_referenced_object_id_hash)|   
[SYSTEM$GET_SERVICE_CALLER_TOKEN_EXPIRY](/sql-reference/functions/system_get_service_caller_token_expiry)| Returns the expiration timestamp of a caller’s rights login token.  
[SYSTEM$GET_SERVICE_DNS_DOMAIN](/sql-reference/functions/system_get_service_dns_domain)|   
[SYSTEM$GET_SERVICE_LOGS](/sql-reference/functions/system_get_service_logs)|   
[SYSTEM$GET_SERVICE_STATUS — Deprecated](/sql-reference/functions/system_get_service_status)| Deprecated; use the [SHOW SERVICE CONTAINERS IN SERVICE](/sql-reference/sql/show-service-containers-in-service) command instead.  
[SYSTEM$GET_SNOWFLAKE_EGRESS_IP_RANGES](/sql-reference/functions/system_get_snowflake_egress_ip_ranges)|   
[SYSTEM$GET_SNOWFLAKE_PLATFORM_INFO](/sql-reference/functions/system_get_snowflake_platform_info)|   
[SYSTEM$GET_STAGE_PRIVATELINK_AUTHORIZED_ENDPOINTS](/sql-reference/functions/system_get_stage_privatelink_authorized_endpoints)|   
[SYSTEM$GET_TABLE_ARCHIVE_METADATA](/sql-reference/functions/system_get_table_archive_metadata)|   
[SYSTEM$GET_TAG](/sql-reference/functions/system_get_tag)|   
[SYSTEM$GET_TAG_ALLOWED_VALUES](/sql-reference/functions/system_get_tag_allowed_values)|   
[SYSTEM$GET_TAG_ON_CURRENT_COLUMN](/sql-reference/functions/system_get_tag_on_current_column)|   
[SYSTEM$GET_TAG_ON_CURRENT_TABLE](/sql-reference/functions/system_get_tag_on_current_table)|   
[SYSTEM$GET_TASK_GRAPH_CONFIG](/sql-reference/functions/system_get_task_graph_config)|   
[SYSTEM$HOLD_PRIVILEGE_ON_ACCOUNT](/sql-reference/functions/system_hold_privilege_on_account)|   
[SYSTEM$INTERNAL_STAGES_PUBLIC_ACCESS_STATUS](/sql-reference/functions/system_internal_stages_public_access_status)|   
[SYSTEM$IS_APPLICATION_ALL_MANDATORY_TELEMETRY_EVENT_DEFINITIONS_ENABLED](/sql-reference/functions/system_is_application_all_mandatory_telemetry_event_definitions_enabled)|   
[SYSTEM$IS_APPLICATION_AUTHORIZED_FOR_TELEMETRY_EVENT_SHARING](/sql-reference/functions/system_is_application_authorized_for_telemetry_event_sharing)|   
[SYSTEM$IS_APPLICATION_INSTALLED_FROM_SAME_ACCOUNT](/sql-reference/functions/system_is_application_installed_from_same_account)|   
[SYSTEM$IS_APPLICATION_SHARING_EVENTS_WITH_PROVIDER](/sql-reference/functions/system_is_application_sharing_events_with_provider)|   
[SYSTEM$IS_GLOBAL_DATA_SHARING_ENABLED_FOR_ACCOUNT](/sql-reference/functions/system_is_global_data_sharing_enabled_for_account)|   
[SYSTEM$IS_LISTING_PURCHASED](/sql-reference/functions/system_is_listing_purchased)|   
[SYSTEM$IS_LISTING_TRIAL](/sql-reference/functions/system_is_listing_trial)|   
[SYSTEM$LAST_CHANGE_COMMIT_TIME](/sql-reference/functions/system_last_change_commit_time)|   
[SYSTEM$LIST_ALERT_TEMPLATES](/sql-reference/functions/system_list_alert_templates)|   
[SYSTEM$LIST_APPLICATION_RESTRICTED_FEATURES](/sql-reference/functions/system_list_application_restricted_features)|   
[SYSTEM$LIST_ICEBERG_TABLES_FROM_CATALOG](/sql-reference/functions/system_list_iceberg_tables_from_catalog)|   
[SYSTEM$LIST_NAMESPACES_FROM_CATALOG](/sql-reference/functions/system_list_namespaces_from_catalog)|   
[SYSTEM$LOCATE_DBT_ARCHIVE](/sql-reference/functions/system_locate_dbt_archive)|   
[SYSTEM$LOCATE_DBT_ARTIFACTS](/sql-reference/functions/system_locate_dbt_artifacts)|   
[SYSTEM$LOG, SYSTEM$LOG_<level> (for Snowflake Scripting)](/sql-reference/functions/system_log)|   
[SYSTEM$PIPE_STATUS](/sql-reference/functions/system_pipe_status)|   
[SYSTEM$QUERY_REFERENCE](/sql-reference/functions/system_query_reference)|   
[SYSTEM$READ_YAML_FROM_SEMANTIC_VIEW](/sql-reference/functions/system_read_yaml_from_semantic_view)|   
[SYSTEM$READ_OSI_YAML_FROM_SEMANTIC_VIEW](/sql-reference/functions/system_read_osi_yaml_from_semantic_view)|   
[SYSTEM$REFERENCE](/sql-reference/functions/system_reference)|   
[SYSTEM$REGISTRY_LIST_IMAGES](/sql-reference/functions/system_registry_list_images)| Deprecated; use the [SHOW IMAGES IN IMAGE REPOSITORY](/sql-reference/sql/show-images-in-image-repository) command instead.  
[SYSTEM$REPORT_HEALTH_STATUS](/sql-reference/functions/system_report_health_status)|   
[](/sql-reference/functions/system_sap_bdc_list_shares)|   
[SYSTEM$SET_RETURN_VALUE](/sql-reference/functions/system_set_return_value)|   
[SYSTEM$SET_SPAN_ATTRIBUTES (for Snowflake Scripting)](/sql-reference/functions/system_set_span_attributes)|   
[SYSTEM$SHOW_ACTIVE_BEHAVIOR_CHANGE_BUNDLES](/sql-reference/functions/system_show_active_behavior_change_bundles)|   
[SYSTEM$SHOW_BUDGETS_FOR_RESOURCE](/sql-reference/functions/system_show_budgets_for_resource)|   
[SYSTEM$SHOW_BUDGETS_IN_ACCOUNT](/sql-reference/functions/system_show_budgets_in_account)|   
[SYSTEM$SHOW_EVENT_SHARING_ACCOUNTS](/sql-reference/functions/system_show_event_sharing_accounts)|   
[SYSTEM$SHOW_MOVE_ORGANIZATION_ACCOUNT_STATUS](/sql-reference/functions/system_show_move_organization_account_status)|   
[SYSTEM$SHOW_OAUTH_CLIENT_SECRETS](/sql-reference/functions/system_show_oauth_client_secrets)|   
[SYSTEM$SHOW_SENSITIVE_DATA_MONITORED_ENTITIES](/sql-reference/functions/system_show_sensitive_data_monitored_entities)|   
[SYSTEM$STREAM_BACKLOG](/sql-reference/functions/system_stream_backlog)| This function is a [table function](/sql-reference/functions-table).  
[SYSTEM$STREAM_GET_TABLE_TIMESTAMP](/sql-reference/functions/system_stream_get_table_timestamp)|   
[SYSTEM$STREAM_HAS_DATA](/sql-reference/functions/system_stream_has_data)|   
[SYSTEM$SUPPORTED_DBT_VERSIONS](/sql-reference/functions/system_supported_dbt_versions)|   
[SYSTEM$TASK_RUNTIME_INFO](/sql-reference/functions/system_task_runtime_info)|   
[SYSTEM$TYPEOF](/sql-reference/functions/system_typeof)|   
[SYSTEM$VALIDATE_STORAGE_INTEGRATION](/sql-reference/functions/system_validate_storage_integration)|   
[SYSTEM$VERIFY_CATALOG_INTEGRATION](/sql-reference/functions/system_verify_catalog_integration)|   
[SYSTEM$VERIFY_CMK_INFO](/sql-reference/functions/system_verify_cmk_info)|   
[SYSTEM$VERIFY_CMK_INFO_POSTGRES](/sql-reference/functions/system_verify_cmk_info_postgres)|   
[SYSTEM$VERIFY_EXTERNAL_OAUTH_TOKEN](/sql-reference/functions/system_verify_ext_oauth_token)|   
[SYSTEM$VERIFY_EXTERNAL_VOLUME](/sql-reference/functions/system_verify_external_volume)|   
[SYSTEM$WHITELIST](/sql-reference/functions/system_whitelist)| Deprecated; use [SYSTEM$ALLOWLIST](/sql-reference/functions/system_allowlist) instead.  
[SYSTEM$WAIT_FOR_SERVICES](/sql-reference/functions/system_wait_for_services)|   
[SYSTEM$WHITELIST_PRIVATELINK](/sql-reference/functions/system_whitelist_privatelink)| Deprecated; use [SYSTEM$ALLOWLIST_PRIVATELINK](/sql-reference/functions/system_allowlist_privatelink) instead.  
**Query Information**|   
[EXPLAIN_GRANTABLE_PRIVILEGES](/sql-reference/functions/explain_grantable_privileges)|   
[EXPLAIN_JSON](/sql-reference/functions/explain_json)|   
[GET_QUERY_OPERATOR_STATS](/sql-reference/functions/get_query_operator_stats)|   
[GET_PYTHON_PROFILER_OUTPUT (SNOWFLAKE.CORE)](/sql-reference/functions/get_python_profiler_output)|   
[SYSTEM$ESTIMATE_QUERY_ACCELERATION](/sql-reference/functions/system_estimate_query_acceleration)|   
[SYSTEM$EXPLAIN_PLAN_JSON](/sql-reference/functions/system_explain_plan_json)|   
[SYSTEM$EXPLAIN_JSON_TO_TEXT](/sql-reference/functions/system_explain_json_to_text)|   
[SYSTEM$GET_RESULTSET_STATUS](/sql-reference/functions/system_get_resultset_status)|
