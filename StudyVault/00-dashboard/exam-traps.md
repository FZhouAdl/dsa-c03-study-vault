---
source_pdf: sources/SnowProDataScientistStudyGuide.pdf
part: all
keywords: exam traps, common mistakes, weak areas, misconceptions
---
# Exam Traps

#dashboard #exam-traps

> [!warning] Purpose
> This note collects only the points candidates most often get wrong or misread. Each trap links to the concept note that resolves it.

## Domain 1 — Data Science Concepts

> [!danger]- Trap: R² misread as slope or correlation
> - R² = 0.85 does **not** mean "y changes 0.85 per 1 unit of x" and is not the slope.
> - It means 85% of variability in the response is explained by the linear model.
> - Many confuse with Pearson r or the regression coefficient.
> - Fix: R² = coefficient of determination → [statistics-for-data-science](../01-data-science-concepts/statistics-for-data-science.md)

> [!danger]- Trap: Confusion matrix cell misnamed
> - "Not overbooked; model said not overbooked" → **True Negative** (correctly rejected negative).
> - TN vs TP vs FN vs FP confusion under scenario phrasing.
> - Fix: row = actual, col = predicted → [ml-fundamentals](../01-data-science-concepts/ml-fundamentals.md)

> [!danger]- Trap: Workload sequence ordering
> - Viz/exploration comes **before** feature engineering; monitoring comes **after** deployment.
> - Misreading "monitor before deploy" is a classic wrong answer.
> - Fix: Data collection → viz/explore → feature eng → train → deploy → monitor → [ml-lifecycle](../01-data-science-concepts/ml-lifecycle-and-mlops.md)

> [!danger]- Trap: CLT assumptions
> - CLT ≠ "data becomes normal"; it says the **sampling distribution** of the mean becomes normal as n grows.
> - SE = σ/√n not σ. Small n / unknown σ → t-test not z.
> - Fix: [statistics-for-data-science](../01-data-science-concepts/statistics-for-data-science.md)

## Domain 2 — Data Prep & Feature Engineering

> [!danger]- Trap: APPROX_TOP_K vs APPROX_COUNT_DISTINCT
> - "Approx number of times a value appears" → most frequent = **APPROX_TOP_K**, not APPROX_COUNT_DISTINCT (cardinality) or APPROX_PERCENTILE.
> - Official sample #3 answer is APPROX_TOP_K.
> - Fix: [exploratory-data-analysis](../02-data-preparation-and-feature-engineering/exploratory-data-analysis.md)

> [!danger]- Trap: Leakage via scaling
> - Fit scaler only on **training** split; apply same fitted transform to val/test; never refit on combined data.
> - Binning/encodings must also be fit inside CV to avoid overfitting.
> - Fix: [feature-engineering-techniques](../02-data-preparation-and-feature-engineering/feature-engineering-techniques.md)

> [!danger]- Trap: "Normalization" is overloaded
> - Snowflake ML `Normalizer` = L2 unit-vector row scaling, not necessarily Min-Max.
> - Standardization = z-score `(x−μ)/σ`.
> - Fix: [feature-engineering-techniques](../02-data-preparation-and-feature-engineering/feature-engineering-techniques.md)

## Domain 3 — Model Development

> [!danger]- Trap: SP vs UDF vs UDTF
> - Stored procedure = multi-statement orchestration & side effects (DML).
> - UDF = scalar per-row; UDTF = tabular, per-partition.
> - Picking UDF when you need DML tasks or need to return multiple rows → wrong.
> - Fix: [model-training-pipelines](../03-model-development/model-training-pipelines.md)

> [!danger]- Trap: Metric choice in validation
> - log loss = probabilistic classification; AUC = rank/threshold-free; RMSE = regression & sensitive to outliers.
> - Using RMSE for classification scenarios or AUC for regression → error.
> - Fix: [hyperparameter-tuning-and-validation](../03-model-development/hyperparameter-tuning-and-validation.md)

> [!danger]- Trap: Residual plot interpretation
> - Non-random/funnel vs fan shape → **heteroscedasticity** or missing nonlinearity — model not valid despite high R².
> - Fix: [hyperparameter-tuning-and-validation](../03-model-development/hyperparameter-tuning-and-validation.md)

## Domain 4 — Deployment & Monitoring

> [!danger]- Trap: Drift type confusion
> - **Data drift**: input distribution shift (training vs serving look different).
> - **Concept drift**: "same data, different predictions once deployed" — relationship X→y changed.
> - Fix: [drift-monitoring-and-retraining](../04-model-deployment/drift-monitoring-and-retraining.md)

> [!danger]- Trap: Scalar vs vectorized UDF throughput
> - Vectorized receives **pandas DataFrame batches** → far higher inference throughput than scalar per-row.
> - For production batch scoring of many records, choose vectorized.
> - Fix: [deployment-patterns](../04-model-deployment/deployment-patterns.md)

> [!danger]- Trap: Version rollback in registry
> - Registry versions are immutable; set a different version to 'default' to roll back — do not delete-and-recreate.
> - Fix: [model-registry-and-versioning](../04-model-deployment/model-registry-and-versioning.md)

> [!danger]- Trap: Precision/recall tradeoff vs ROC
> - AUC is threshold-independent but doesn't show precision/recall at operational threshold; monitor both.
> - Fix: [drift-monitoring-and-retraining](../04-model-deployment/drift-monitoring-and-retraining.md)

## Related
- [MOC](moc.md) → Weak Areas section
- [Quick Reference](quick-reference.md)