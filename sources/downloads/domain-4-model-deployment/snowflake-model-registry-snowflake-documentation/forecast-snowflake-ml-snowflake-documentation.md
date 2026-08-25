---
title: "FORECAST (SNOWFLAKE.ML) | Snowflake Documentation"
source: https://docs.snowflake.com/sql-reference/classes/forecast
cert_domain: domain-4-model-deployment
crawl_depth: 1
crawled: 2026-08-23
---

# FORECAST (SNOWFLAKE.ML)¶

A forecast model produces a forecast for a single time series or for multiple time series. You use [\- [](/user-guide/ml-functions/forecasting)](/sql-reference/classes/forecast/commands/create-forecast#label-class-forecast-create) to create and train the forecasting model, then use the model’s [<model_name>!FORECAST](/sql-reference/classes/forecast/methods/forecast#label-class-forecast-forecast) method to produce forecasts. The [<model_name>!EXPLAIN_FEATURE_IMPORTANCE](/sql-reference/classes/forecast/methods/explain_feature_importance#label-class-forecast-explain-feature-importance) method provides information about how each feature in the training data influences the forecast. The [<model_name>!SHOW_TRAINING_LOGS](/sql-reference/classes/forecast/methods/show_training_logs#label-class-forecast-show-training-logs) method provides error messages for any time series whose models failed to fit. The [<model_name>!SHOW_EVALUATION_METRICS](/sql-reference/classes/forecast/methods/show_evaluation_metrics#label-class-forecast-show-evaluation-metrics) method provides evaluation metrics on out-of-sample data.

Important

**Legal notice.** This Snowflake ML function is powered by machine learning technology, which you, not Snowflake, determine when and how to use. Machine learning technology and results provided may be inaccurate, inappropriate, or biased. Snowflake provides you with the machine learning models that you can use within your own workflows. Decisions based on machine learning outputs, including those built into automatic pipelines, should have human oversight and review processes to ensure model-generated content is accurate. Snowflake provides algorithms (without any pretraining) and you’re responsible for the data that you provide the algorithm (for example, for training and inference) and the decisions you make using the resulting model’s output. Queries for this feature or function are treated as any other SQL query and may be considered [metadata](/sql-reference/metadata).

**Metadata.** When you use Snowflake ML functions, Snowflake logs generic error messages returned by an ML function. These error logs help us troubleshoot issues that arise and improve these functions to serve you better.

For further information, see [Snowflake AI Trust and Safety FAQ](https://www.snowflake.com/en/legal/snowflake-ai-trust-and-safety/).

## FORECAST commands¶

  * [CREATE SNOWFLAKE.ML.FORECAST](/sql-reference/classes/forecast/commands/create-forecast)
  * [DROP SNOWFLAKE.ML.FORECAST](/sql-reference/classes/forecast/commands/drop-forecast)
  * [SHOW SNOWFLAKE.ML.FORECAST](/sql-reference/classes/forecast/commands/show-forecast)



## FORECAST methods¶

  * [<model_name>!FORECAST](/sql-reference/classes/forecast/methods/forecast)
  * [<model_name>!EXPLAIN_FEATURE_IMPORTANCE](/sql-reference/classes/forecast/methods/explain_feature_importance)
  * [<model_name>!SHOW_EVALUATION_METRICS](/sql-reference/classes/forecast/methods/show_evaluation_metrics)
  * [<model_name>!SHOW_TRAINING_LOGS](/sql-reference/classes/forecast/methods/show_training_logs)
