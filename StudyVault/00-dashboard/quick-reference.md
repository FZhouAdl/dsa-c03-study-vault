---
source_pdf: sources/SnowProDataScientistStudyGuide.pdf
part: all
keywords: quick reference, formulas, cheat sheet
---
# DSA-C03 Quick Reference

#dashboard #quick-reference

> [!important] How to use
> Every section links back to the full concept note. This sheet compresses formulas and decision rules for last-minute review.

## Exam Structure

| Item | Detail |
|---|---|
| Prerequisite | Active SnowPro Core certification |
| Domains | 1.0 Data Science Concepts (17%), 2.0 Prep & Feature Engineering (27%), 3.0 Model Development (31%), 4.0 Model Deployment (25%) |
| Format | Scenario-based questions, real-world examples |

## Domain 1 — Statistics & ML Basics

→ [ML Fundamentals](../01-data-science-concepts/ml-fundamentals.md) · [ML Lifecycle & MLOps](../01-data-science-concepts/ml-lifecycle-and-mlops.md) · [Statistics](../01-data-science-concepts/statistics-for-data-science.md)

| Concept | Formula / Rule |
|---|---|
| Standardization (z-score) | `(x − μ) / σ` |
| Min-Max scaling | `(x − min) / (max − min)` → [0,1] |
| L2 normalization | `x / ‖x‖` (unit vector) |
| Mean | `Σx / n` — pulled toward outliers in **skewed** distributions |
| Variance | `Σ(x − μ)² / n` — σ = √variance (STDEV) |
| Central Limit Theorem | Distribution of sample means → Normal as n↑, regardless of population shape; mean μ, SE = `σ / √n` |
| Confidence Interval (known σ) | `point_est ± z·(σ/√n)`; wider n→ narrower; 95% ≈ z=1.96 |
| t-test | Unknown σ / small n; more conservative than z; df = n−1 |
| Bootstrap | Resample with replacement; estimate SE/CI without parametric assumptions |
| R² | Fraction of response variability explained by linear model (0.85 → 85%) — NOT slope, NOT correlation itself |

## Domain 2 — Data Prep & EDA

→ [Data Prep](../02-data-preparation-and-feature-engineering/data-preparation-with-snowpark.md) · [EDA](../02-data-preparation-and-feature-engineering/exploratory-data-analysis.md) · [Feature Engineering](../02-data-preparation-and-feature-engineering/feature-engineering-techniques.md) · [Feature Store & Notebooks](../02-data-preparation-and-feature-engineering/feature-store-and-notebooks.md) · [Visualization](../02-data-preparation-and-feature-engineering/visualization-in-snowsight.md)

| Keyword | Meaning | Function |
|---|---|---|
| Count distinct-ish | Approx cardinality | `APPROX_COUNT_DISTINCT`, `APPROX_PERCENTILE` |
| **Most-frequent value** | Frequency of values | **`APPROX_TOP_K`** (official sample answer) |
| Binning | Continuous → intervals | `WIDTH_BUCKET`, `NTILE`, `CASE` |
| Row ranking | Partitioned ordering | Window fns: `ROW_NUMBER`, `NTILE`, `PARTITION BY` |
| Missing values | Fill/drop | `fillna`, `df.na.drop/fill`, imputation |
| Duplicates | Dedupe | `drop_duplicates` |
| Dedupe + description | Snowpark DataFrame | Lazy, **pushdown** to warehouse |
| SQL linear regression | Slope/`intercept`/fit | `REGR_SLOPE`, `REGR_INTERCEPT`, `REGR_R2` |
| Point-in-time features | Avoid leakage | Feature Store `ASOF JOIN` |

## Domain 3 — Model Development

→ [Connecting Tools](../03-model-development/connecting-tools-to-snowflake.md) · [Cortex GenAI](../03-model-development/cortex-genai-and-llms.md) · [Training Pipelines](../03-model-development/model-training-pipelines.md) · [Tuning & Validation](../03-model-development/hyperparameter-tuning-and-validation.md) · [Interpretation](../03-model-development/model-interpretation.md)

| Entity | Role |
|---|---|
| Snowpark | DataFrame pushdown compute (Python/Scala/Java) |
| Python Connector | Direct driver; `.fetch_pandas_all()` for pandas |
| Snowpark ML (`snowflake.ml.modeling`) | sklearn-style estimators/preprocessors running in warehouse |
| UDF | Scalar, per-row Python function |
| UDTF | Tabular, many (row, column) outputs, per-partition |
| Stored Procedure | Multi-statement orchestration; owner/caller rights |
| Vectorized UDF | pandas-batch input, higher inference throughput |
| Dynamic Table | Declarative, continuous materialization |
| Task + Stream | Schedule + change detection (`SYSTEM$STREAM_HAS_DATA`) |

| Task | Metric |
|---|---|
| Probabilistic classification | **log loss**; also cross-entropy |
| Threshold-independent ranking | **AUC** (ROC) |
| Regression (outlier-sensitive) | **RMSE** = √(mean squared error); also MAE, R² |
| Expected payout | Confusion matrix × cost/revenue matrix, sum |

## Domain 4 — Deployment & Monitoring

→ [Deployment Patterns](../04-model-deployment/deployment-patterns.md) · [Model Registry](../04-model-deployment/model-registry-and-versioning.md) · [Drift & Retraining](../04-model-deployment/drift-monitoring-and-retraining.md)

| Concern | Tool / Rule |
|---|---|
| Store + version + retrieve | Snowflake Model Registry (semantic versions, default, aliases, rollback) |
| Batch inference in warehouse | UDF-based from registry; scalar or vectorized |
| External hosted model | External functions via API integration | 
| Heavy/GPU/HTTP serving | Snowpark Container Services (SPCS) |
| Store predictions | `CREATE TABLE AS SELECT ... !PREDICT(...)`, Dynamic Tables, stage commands |
| Monitor drift | Compare train vs serving distributions: PSI / KL / KS; label, data, concept drift |
| Retrain automation | Task + stream + stored proc → new registry version |
| Metrics in monitoring | AUC, accuracy, precision, recall (classification) ; RMSE (regression) |

## Must-know Patterns

→ [Exam Traps](exam-traps.md) · [MOC](moc.md)

| Pattern | Answer |
|---|---|
| "present + iterate live while presenting" | Snowflake **Notebooks** (not dashboard/worksheets) |
| Correct workload order | Data collection → viz/explore → feature eng → train → deploy → monitor |
| Confusion matrix cell | "not overbooked & predicted not overbooked" = **True Negative** |
| R² = 0.85 | 85% of variability explained — not slope |
| Frequency estimator | `APPROX_TOP_K` |