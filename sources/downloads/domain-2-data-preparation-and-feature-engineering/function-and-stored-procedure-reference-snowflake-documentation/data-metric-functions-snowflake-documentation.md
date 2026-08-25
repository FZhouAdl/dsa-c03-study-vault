---
title: "Data metric functions | Snowflake Documentation"
source: https://docs.snowflake.com/sql-reference/functions-data-metric
cert_domain: domain-2-data-preparation-and-feature-engineering
crawl_depth: 1
crawled: 2026-08-23
---

# Data metric functions¶

Snowflake provides built-in system data metric functions to measure data quality for tables and views:

  * [ACCEPTED_VALUES (system data metric function)](/sql-reference/functions/dmf_accepted_values)
  * [APPROX_QUANTILE_25 (system data metric function)](/sql-reference/functions/dmf_approx_quantile_25)
  * [APPROX_QUANTILE_50 (system data metric function)](/sql-reference/functions/dmf_approx_quantile_50)
  * [APPROX_QUANTILE_99 (system data metric function)](/sql-reference/functions/dmf_approx_quantile_99)
  * [AVG (system data metric function)](/sql-reference/functions/dmf_avg)
  * [BLANK_COUNT (system data metric function)](/sql-reference/functions/dmf_blank_count)
  * [BLANK_PERCENT (system data metric function)](/sql-reference/functions/dmf_blank_percent)
  * [CASE_FORMAT_VIOLATION_COUNT (system data metric function)](/sql-reference/functions/dmf_case_format_violation_count)
  * [CASE_FORMAT_VIOLATION_PERCENT (system data metric function)](/sql-reference/functions/dmf_case_format_violation_percent)
  * [DATA_METRIC_SCHEDULED_TIME (system data metric function)](/sql-reference/functions/dmf_data_metric_schedule_time)
  * [DUPLICATE_COUNT (system data metric function)](/sql-reference/functions/dmf_duplicate_count)
  * [FRESHNESS (system data metric function)](/sql-reference/functions/dmf_freshness)
  * [FUTURE_TIMESTAMP_COUNT (system data metric function)](/sql-reference/functions/dmf_future_timestamp_count)
  * [FUTURE_TIMESTAMP_PERCENT (system data metric function)](/sql-reference/functions/dmf_future_timestamp_percent)
  * [INVALID_JSON_COUNT (system data metric function)](/sql-reference/functions/dmf_invalid_json_count)
  * [INVALID_JSON_PERCENT (system data metric function)](/sql-reference/functions/dmf_invalid_json_percent)
  * [INVALID_NUMERIC_TYPE_CAST_COUNT (system data metric function)](/sql-reference/functions/dmf_invalid_numeric_type_cast_count)
  * [INVALID_NUMERIC_TYPE_CAST_PERCENT (system data metric function)](/sql-reference/functions/dmf_invalid_numeric_type_cast_percent)
  * [MAX (system data metric function)](/sql-reference/functions/dmf_max)
  * [MEDIAN (system data metric function)](/sql-reference/functions/dmf_median)
  * [MIN (system data metric function)](/sql-reference/functions/dmf_min)
  * [NEGATIVE_COUNT (system data metric function)](/sql-reference/functions/dmf_negative_count)
  * [NEGATIVE_PERCENT (system data metric function)](/sql-reference/functions/dmf_negative_percent)
  * [NULL_COUNT (system data metric function)](/sql-reference/functions/dmf_null_count)
  * [NULL_PERCENT (system data metric function)](/sql-reference/functions/dmf_null_percent)
  * [REFERENTIAL_INTEGRITY_COUNT (system data metric function)](/sql-reference/functions/dmf_referential_integrity_count)
  * [ROW_COUNT (system data metric function)](/sql-reference/functions/dmf_row_count)
  * [SCHEMA_CHANGE_COUNT (system data metric function)](/sql-reference/functions/dmf_schema_change_count)
  * [SPECIAL_CHARACTER_COUNT (system data metric function)](/sql-reference/functions/dmf_special_character_count)
  * [SPECIAL_CHARACTER_PERCENT (system data metric function)](/sql-reference/functions/dmf_special_character_percent)
  * [STDDEV (system data metric function)](/sql-reference/functions/dmf_stddev)
  * [STRING_LENGTH_AVG (system data metric function)](/sql-reference/functions/dmf_string_length_avg)
  * [STRING_LENGTH_MAX (system data metric function)](/sql-reference/functions/dmf_string_length_max)
  * [STRING_LENGTH_MIN (system data metric function)](/sql-reference/functions/dmf_string_length_min)
  * [UNIQUE_COUNT (system data metric function)](/sql-reference/functions/dmf_unique_count)
  * [UNTRIMMED_STRING_COUNT (system data metric function)](/sql-reference/functions/dmf_untrimmed_string_count)
  * [UNTRIMMED_STRING_PERCENT (system data metric function)](/sql-reference/functions/dmf_untrimmed_string_percent)
  * [VARIANCE (system data metric function)](/sql-reference/functions/dmf_variance)
  * [ZERO_COUNT (system data metric function)](/sql-reference/functions/dmf_zero_count)
  * [ZERO_PERCENT (system data metric function)](/sql-reference/functions/dmf_zero_percent)



For details, see [System data metric functions](/user-guide/data-quality-system-dmfs).
