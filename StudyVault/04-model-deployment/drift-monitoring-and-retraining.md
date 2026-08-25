---
source_pdf: domain-4-model-deployment/snowflake-model-registry-snowflake-documentation/ml-observability-monitoring-model-behavior-over-time-snowflake-documen.md
part: "4.0"
keywords: data drift, concept drift, model retraining, ML observability, PSI
---
# Drift Monitoring and Retraining (★★★)
#domain-4 #drift-monitoring

## Overview Table
| Item | Key Point |
|---|---|
| Drift types | Data drift (input X shift), concept drift (X→y relationship change), label drift (y distribution shift) |
| Detection | Compare training vs serving distributions: PSI, KL divergence, KS test; monitor prediction shifts too |
| Snowflake tool | ML Observability — `CREATE MODEL MONITOR` per model version, baseline table for drift comparison |
| Metrics tracked | Drift metrics, performance metrics (accuracy/precision/recall/AUC/RMSE), statistical metrics (counts, nulls) |
| Retraining automation | Tasks + streams + stored procedures → retrain → `log_model()` new registry version |
| Model decay | Deployed model quality degrades over time as world changes — the reason monitoring exists |

## Drift Types
| Type | Definition | Signature example |
|---|---|---|
| **Data drift** (covariate shift) | Distribution of input features changes vs training data | New region added; age histogram shifts |
| **Concept drift** | P(y\|X) relationship changes; same inputs now map to different outputs | Fraud patterns change post-policy update |
| **Label drift** (prior shift) | Distribution of target y changes | Churn rate doubles after pricing change |

> [!warning]
> Exam heuristic: inputs moved → data drift; inputs unchanged but correct answers changed → concept drift. "Same input rows no longer get same predictions" points at concept drift or model decay.

## Detection Methods
| Method | What it measures |
|---|---|
| PSI (Population Stability Index) | Binned distribution distance between baseline and current; standard drift metric in Snowflake monitors |
| KL divergence | Information-theoretic distance between distributions |
| Kolmogorov–Smirnov test | Statistical test of whether two samples come from same distribution |

Key comparisons from objective 4.2:
1. Do serving-time feature distributions look like training data? → data drift check.
2. Do the same data points give the same predictions after deployment? → prediction stability / concept drift check.

## ML Observability (Snowflake Native)
```sql
CREATE MODEL MONITOR my_monitor WITH
  MODEL          = ml.registry.my_model
  VERSION        = v1
  FUNCTION       = predict
  SOURCE         = my_log_table           -- ID, timestamp, features, prediction, actual
  BASELINE_TABLE = my_training_baseline   -- required for drift metrics
  WAREHOUSE      = my_wh
  AGGREGATION_WINDOW = '1 DAY'
  SEGMENT_COLUMNS = ('region');
```

| Fact | Detail |
|---|---|
| Granularity | One monitor per model version; monitor lives in same schema as version; max ~250/account |
| Logs | Table with ID, timestamp, features, predictions, optional ground-truth label |
| Refresh | Automatic; suspends after 5 consecutive refresh failures (`ALTER MODEL MONITOR ... RESUME` to restart) |
| Segments | Up to 5 string categorical segment columns (e.g., region, customer tier) |
| Query API | `MODEL_MONITOR_DRIFT_METRIC(...)`, `MODEL_MONITOR_PERFORMANCE_METRIC(...)`, `MODEL_MONITOR_STAT_METRIC(...)` |

```sql
SELECT * FROM TABLE(
  MODEL_MONITOR_DRIFT_METRIC('my_monitor', 'PSI', 'FEATURE_1',
                             'DAY', $start_ts, $end_ts));
```

> [!warning]
> Drift calculation requires a baseline (typically training data); adding a baseline later means drop + recreate the monitor. Prediction column needed; actuals optional but required for accuracy-style metrics.

## Evaluation Metrics for Effectiveness (Objective 4.2)
| Metric | Task | Meaning |
|---|---|---|
| Accuracy | Classification | (TP+TN)/all — misleading on imbalanced data |
| Precision | Classification | TP/(TP+FP) — cost of false positives matters |
| Recall | Classification | TP/(TP+FN) — cost of false negatives matters |
| F1 | Classification | Harmonic mean of precision and recall |
| AUC | Binary classification | Ranking quality across all thresholds; 0.5 = random, 1.0 = perfect |
| RMSE | Regression | Root mean squared error in target units; penalizes large errors |
| Log loss | Classification (global) | Penalizes overconfident wrong probabilities |

Classification ML functions expose these via `model!SHOW_EVALUATION_METRICS()`, `SHOW_GLOBAL_EVALUATION_METRICS()`, `SHOW_THRESHOLD_METRICS()` (ROC/PR curve data), `SHOW_CONFUSION_MATRIX()`. Registry versions store custom scores via `mv.set_metric(...)`.

## Retraining Loop
```text
            ┌────────────────────────────────────────────────────┐
            │                                                    │
            v                                                    │
   ┌─────────────────┐    drift/perf breach     ┌───────────────┐ │
   │ SERVING MODEL   │ ───────────────────────> │ RETRAIN JOB   │ │
   │ registry ver vN │   (monitor metric/alert) │ task + proc   │ │
   └────────┬────────┘                          └──────┬────────┘ │
            │                                          │          │
     predictions +                             train on fresh
     actuals logged                            labeled data;
            │                                  evaluate offline
            v                                          │
   ┌─────────────────┐                                 v
   │ MODEL MONITOR   │                         ┌───────────────┐
   │ drift: PSI/KL/KS│                         │ REGISTER NEW  │
   │ perf: AUC/P/R/  │                         │ VERSION vN+1  │
   │      RMSE       │                         │ reg.log_model │
   └─────────────────┘                         └──────┬────────┘
                                                       │ validate,
                                                       │ promote default
                                                       └──────────>
```

## Automation of Model Retraining (Objective 4.3)
| Component | Role |
|---|---|
| Stream / Dynamic Table | Capture newly arrived labeled data |
| Task (scheduled) | Trigger stored procedure on schedule or stream arrival |
| Stored procedure | Train model, compute metrics, call `reg.log_model(..., version_name=v_next)` |
| Monitor + alert | Fire when drift/performance crosses threshold → start pipeline |
| Promotion step | Set new version as `default` (or tag `stage=prod`) after validation; old versions retained for rollback |

## Metadata Tagging for Lifecycle (Objective 4.3)
```sql
CREATE TAG stage ALLOWED_VALUES 'dev','staging','prod';
ALTER MODEL my_model SET TAG stage = 'prod';
```
```python
m.set_tag("stage", "prod")
m.get_tag("stage")
```
- Tags defined first (`CREATE TAG`, optionally allowed values), then applied to models for governance/lifecycle tracking.
- Metrics differ: free-form JSON values attached per **version**; tags attach to the **model** object.

## Exam Patterns
| Scenario/Keyword | Answer |
|---|---|
| "Feature histograms differ between training and production" | Data drift — compare with PSI/KS against baseline |
| "Same customers now churn even though features identical" | Concept drift — X→y relationship changed |
| "Monitor deployed sklearn model registered in Snowflake" | CREATE MODEL MONITOR on that model version + log table + baseline |
| "Which metric for regression degradation?" | RMSE |
| "Imbalanced fraud detection, missing fraud is expensive" | Optimize recall (watch precision trade-off); AUC for threshold-free ranking |
| "Automate weekly retraining" | Scheduled task → stored procedure → `log_model()` new version |
| "Return to previous good model" | Repoint default version (`m.default = ...`) |

## Related Notes
- [Model Registry and Versioning](model-registry-and-versioning.md)
- [Deployment Patterns](deployment-patterns.md)
- [Practice: Model Deployment](model-deployment-practice.md)

## Source Documents
- ../../sources/downloads/domain-4-model-deployment/snowflake-model-registry-snowflake-documentation/ml-observability-monitoring-model-behavior-over-time-snowflake-documen.md
- ../../sources/downloads/domain-4-model-deployment/snowflake-model-registry-snowflake-documentation.md
- ../../sources/downloads/domain-4-model-deployment/classification-snowflake-ml-functions-snowflake-documentation.md
