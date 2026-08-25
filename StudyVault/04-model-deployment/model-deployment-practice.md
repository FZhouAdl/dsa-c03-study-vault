---
source_pdf: sources/SnowProDataScientistStudyGuide.pdf
part: "4.0"
keywords: model deployment practice, model registry, vectorized UDF, drift, retraining
---
# Model Deployment Practice (Domain 4)
#practice #domain-4

## Related Concepts
- [Deployment Patterns](deployment-patterns.md)
- [Model Registry and Versioning](model-registry-and-versioning.md)
- [Drift Monitoring and Retraining](drift-monitoring-and-retraining.md)

---

> [!hint]- Key patterns (click to expand)
> | Keyword | Answer |
> | Log + version + discover models | Snowflake Model Registry (log_model, SHOW MODELS) |
> | High-throughput batch inference | Vectorized Python UDF (pandas batches) |
> | Call external hosted model | External function via API integration |
> | GPU / >15GB / HTTP serving | Snowpark Container Services |
> | Rollback | ALTER MODEL SET DEFAULT_VERSION (old versions persist) |
> | Input distribution shift | Data drift (PSI/KL/KS) |
> | Same data, different predictions | Concept drift |
> | Regression monitoring | RMSE |
> | Classification monitoring | accuracy / precision / recall / AUC |


## Q1 — Registry API (recall)
Which Python class manages models within a schema in the Snowflake Model Registry, and which method logs a new model version? What does it return?

> [!hint]- Hint
> Three core classes exist; logging is a single call on the registry object.

> [!answer]- Show answer
> `snowflake.ml.registry.Registry`, opened with `Registry(session=..., database_name=..., schema_name=...)`. Logging is done with `reg.log_model(model_obj, model_name=..., version_name=..., conda_dependencies=..., sample_input_data=...)`, which returns a `snowflake.ml.model.ModelVersion`. The other two core classes are `snowflake.ml.model.Model` (a model) and `snowflake.ml.model.ModelVersion` (a version of a model).

## Q2 — SQL discovery and rollback (recall)
Write the SQL to list all models, list the versions of one model, and repoint production back to a prior version.

> [!hint]- Hint
> Two SHOW commands plus one ALTER; promotion/rollback is a metadata change.

> [!answer]- Show answer
> `SHOW MODELS [LIKE '...'] [IN SCHEMA ...];` lists models (metadata columns include `name`, `versions`, `default_version_name`). `SHOW VERSIONS IN MODEL my_model;` lists that model's versions (with `is_default_version`, `functions`, `metadata`). Rollback = repoint default: `ALTER MODEL my_model SET DEFAULT_VERSION = V1;`. Old versions are retained, so any consumer calling the unqualified model immediately gets the re-pointed version.

## Q3 — Vectorized UDF definition (recall)
What is a vectorized Python UDF, how is a handler annotated, and what two constraints apply?

> [!hint]- Hint
> Batch in, batch out; annotation is a decorator or an attribute.

> [!answer]- Show answer
> A vectorized Python UDF receives **batches of rows as pandas DataFrames** and returns a pandas array/Series whose length must equal the input DataFrame. Annotate with `@vectorized(input=pandas.DataFrame, max_batch_size=N)` (imported from `_snowflake`) or set `handler._sf_vectorized_input = pandas.DataFrame`. Constraints: each handler call must complete within **180 seconds**, and each input batch holds up to a few thousand rows (cap with `max_batch_size`). Batching and query syntax are otherwise identical to scalar UDFs.

## Q4 — Classification metric definitions (recall)
Define accuracy, precision, recall, and AUC for a binary classifier. Which configuration makes accuracy misleading?

> [!hint]- Hint
> Two of these condition on the predicted positives; two condition differently.

> [!answer]- Show answer
> Accuracy = (TP+TN)/all — overall correct rate; Precision = TP/(TP+FP) — share of predicted positives that are actually positive; Recall (sensitivity) = TP/(TP+FN) — share of actual positives caught; AUC = probability a random positive ranks above a random negative, **threshold-independent** (0.5 = random, 1.0 = perfect). Accuracy is misleading under **class imbalance**: a 99/1 split lets a "predict majority always" model hit 99% accuracy while catching zero positives.

## Q5 — RMSE for regression monitoring (recall)
What does RMSE measure and why is it preferred over MSE when reporting regression degradation, such as in a model monitor?

> [!hint]- Hint
> Units matter, and squaring inflates large errors.

> [!answer]- Show answer
> RMSE = square root of the mean squared error between predictions and actuals. It is in the **same units as the target** ("predictions off by ~$3,400 on average"), directly interpretable for stakeholders, unlike MSE whose squared units are not comparable to target values. Because errors are squared, RMSE penalizes large misses heavily — a useful sensitivity for detecting degradation over time via a model monitor's performance metrics.

## Q6 — Monitor setup requirements (recall)
Per Snowflake ML Observability, name three requirements/constraints when creating a model monitor for a registered model version.

> [!hint]- Hint
> Think placement, baseline, granularity, and count caps.

> [!answer]- Show answer
> Any three of: monitor must be created in the **same schema as the model version** (needs CREATE MODEL MONITOR privilege); exactly **one monitor per model version**; a **baseline table is required for drift** metrics (adding one later requires drop + recreate); aggregation window specified in days (1 day minimum); max ~250 monitors per account and 500 monitored features; log source must have ID + timestamp + prediction columns (actuals optional, needed for accuracy metrics); segment columns must be string categorical (max 5).

## Q7 — Scalar vs vectorized UDF choice (application)
A team must score 500M rows with an sklearn pipeline exposed as a Python UDF; row-by-row execution is too slow. Which UDF style should they use, and what one annotation and one constraint apply?

> [!hint]- Hint
> Think pandas batch processing instead of per-row calls; there is a per-call time limit.

> [!answer]- Show answer
> Use a **vectorized Python UDF**: annotate the handler with `@vectorized(input=pandas.DataFrame)` (or `_sf_vectorized_input`) so it receives pandas DataFrame batches and returns a pandas array/Series of equal length — substantially better throughput when the scoring code operates efficiently on batches. Constraint: each handler call must finish within **180 seconds**, so set a `max_batch_size` (e.g., 1000) to cap rows per DataFrame and stay inside the limit.

## Q8 — Drift type identification (application)
Scenario A: the PSI of feature `income` versus the training baseline jumps sharply three months after deployment. Scenario B: feature distributions are unchanged, but after a policy change the same customer profiles genuinely churn more often and measured accuracy keeps falling. Identify the drift type in each and state what detection metric surfaces it.

> [!hint]- Hint
> Scenario A is about the inputs; Scenario B is about the mapping.

> [!answer]- Show answer
> Scenario A = **data drift** (covariate shift): serving-time input distribution differs from the training distribution while the X→y mapping has not been shown to change; surfaced by **drift metrics** (PSI/KL/KS) on features. Scenario B = **concept drift**: P(y|X) changed even though inputs are stable; input PSI may stay calm while **performance metrics** (accuracy/precision/recall, RMSE) degrade. Remedy in both: retrain on representative recent labeled data and log a new registry version.

## Q9 — External functions vs in-Snowflake deployment (analysis)
A team already hosts a fraud model on SageMaker. Compare calling it from SQL via an external function versus re-deploying it in Snowflake via the Model Registry. Give one advantage of each.

> [!hint]- Hint
> Weigh operational reuse and ownership against data locality, latency, and governance.

> [!answer]- Show answer
> External function: keeps the existing managed endpoint and its ops/ownership untouched — fastest path to integrate a proprietary or non-portable model; but adds per-request network latency, an external availability dependency, API/egress cost, and sends data outside Snowflake, needing an API integration plus a proxy/relay in front of the hosted endpoint. Registry deployment: inference runs where the data lives (warehouse UDF via `mv.run()` / `MODEL(m)!predict`), no per-row network hop, centrally governed artifacts with RBAC, tags, and versioning; but requires a picklable Python model within warehouse limits (~15 GB, CPU) or SPCS, and re-logging/re-testing in Snowflake.

## Q10 — Precision/recall tradeoff in monitoring (analysis)
For a fraud monitor, raising the decision threshold lifts precision from 0.6 to 0.9 while recall drops from 0.85 to 0.4. Interpret both operating points and state which to choose if a missed fraud costs $50k and a false review costs $200.

> [!hint]- Hint
> Precision guards false positives; recall guards false negatives. Compare the costs.

> [!answer]- Show answer
> At recall 0.85 / precision 0.6, 85% of actual fraud is caught but ~40% of flagged cases are false alarms. At recall 0.4 / precision 0.9, only 40% of fraud is caught while flagged cases are 90% trustworthy. Missed-fraud cost dwarfs false-review cost by $50k/$200 = 250×, so minimizing false negatives dominates: the **low-threshold point (recall 0.85)** is preferable — but the correct method is to threshold-tune from ROC/PR curve data (e.g., `SHOW_THRESHOLD_METRICS()` in the classification ML function, or the monitor's performance metrics) to the point that minimizes expected cost = FP·$200 + FN·$50k, rather than picking a precision target in the abstract.

---

## Summary
> [!summary]- Key takeaways
> - Registry flow: `Registry` → `log_model` → `ModelVersion.run()` / SQL `MODEL(m)!predict`; discovery via `SHOW MODELS`, `SHOW VERSIONS IN MODEL`.
> - Rollback = metadata flip of `default` version (`ALTER MODEL ... SET DEFAULT_VERSION`); old versions persist.
> - Vectorized UDFs (pandas batches, `@vectorized`, 180 s limit) beat scalar row-wise UDFs for bulk scoring.
> - External functions integrate hosted models (SageMaker) at the price of latency, cost, and data-boundary crossing.
> - Data drift = input distributions shift (PSI/KL/KS); concept drift = X→y changes (performance metrics fall); retrain and register new version.
> - Classification: accuracy/precision/recall/AUC; regression: RMSE; account for imbalance and cost of FP vs FN when monitoring.