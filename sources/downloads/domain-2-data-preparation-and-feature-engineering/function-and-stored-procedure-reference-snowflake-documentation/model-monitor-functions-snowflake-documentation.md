---
title: "Model monitor functions | Snowflake Documentation"
source: https://docs.snowflake.com/sql-reference/functions-model-monitors
cert_domain: domain-2-data-preparation-and-feature-engineering
crawl_depth: 1
crawled: 2026-08-23
---

# Model monitor functions¶

Model monitors allow you to track the performance of your machine learning models in production. Snowflake supports two types of monitors. See [ML Observability](/developer-guide/snowflake-ml/model-registry/model-observability) for model version monitors and [Gateway Monitoring & A/B Testing](/developer-guide/snowflake-ml/inference/gateway-monitor-and-ab-testing) for gateway model monitors.

You can use the following functions to retrieve metrics from the model monitors.

>   * MODEL_MONITOR_DRIFT_METRIC
>   * MODEL_MONITOR_PERFORMANCE_METRIC
>   * MODEL_MONITOR_STAT_METRIC
> 


Each function requires the name of a model monitor and the name of a metric to be retrieved from that model.

## List of functions¶

Function name| Notes  
---|---  
[MODEL_MONITOR_DRIFT_METRIC](/sql-reference/functions/model-monitor-drift-metric)|   
[MODEL_MONITOR_PERFORMANCE_METRIC](/sql-reference/functions/model-monitor-performance-metric)|   
[MODEL_MONITOR_STAT_METRIC](/sql-reference/functions/model-monitor-stat-metric)|
