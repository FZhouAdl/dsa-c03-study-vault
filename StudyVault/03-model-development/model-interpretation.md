---
source_pdf: domain-3-model-development/model-explainability-snowflake-documentation.md
part: "3.0"
keywords: shap, shapley values, partial dependence, confidence intervals, feature impact
---
# Model Interpretation (★★)

#domain-3 #model-interpretation

## Overview Table

| Item | Key Point |
|---|---|
| Feature impact | Which features move predictions most (global importance) |
| SHAP / Shapley values | Additive per-prediction attributions from game theory; Snowflake Model Registry `explain()` |
| Partial dependence plots | Marginal average effect of one/two features across prediction surface |
| Confidence intervals | Uncertainty range around estimates/predictions |

## SHAP in Snowflake

| Item | Key Point |
|---|---|
| Basis | Shapley values: average marginal contribution of a feature over all feature combinations — fair attribution |
| Property | Contributions **add up**: base value + Σ SHAP = final prediction; can be positive or negative |
| Background data | Representative sample defining "average" inputs; needed for meaningful explanations; ≤1000 rows via `sample_input_data` when logging |
| Enabled | Default for supported models, Snowpark ML ≥1.6.2; uses SHAP library |
| Supported models | XGBoost, CatBoost, LightGBM, scikit-learn (native + `snowflake.ml.modeling` classes) |
| Retrieval | Python: `mv.run(input_data, function_name="explain")`; SQL: `mv_alias!EXPLAIN(...)` |

House example (avg price $100k, prediction $250k):

| Feature | Value | Contribution vs avg |
|---|---|---|
| Size | 2000 sqft | +$50,000 |
| Location | Beachside | +$75,000 |
| Bedrooms | 3 | +$50,000 |
| Pets allowed | No | −$25,000 |
| **Total** | | **+$150,000** → $250k |

```python
reg = Registry(session)
mv = reg.get_model("DIAMOND_MODEL").default
explanations = mv.run(input_data, function_name="explain")
```

```sql
WITH MV_ALIAS AS MODEL DB.SCHEMA.DIAMOND_MODEL VERSION EXPLAIN_V0
SELECT * FROM DB.SCHEMA.DIAMOND_DATA,
     TABLE(MV_ALIAS!EXPLAIN(CUT, COLOR, CLARITY, CARAT));
```

> [!warning] Caveats
> - Shapley computation is expensive; tree-based models encode some background internally but still explain better with explicit background data.
> - Models logged before Snowpark ML 1.6.2 lack explainability — versions are immutable, must log a new version.
> - SHAP explains the model's association with features, NOT causal effects.

## Partial Dependence Plots (PDP)

| Item | Key Point |
|---|---|
| Definition | Average model output as feature X varies over its range, marginalizing (averaging) all other features |
| Reading | Curve shows direction/shape of marginal effect: monotonic? plateau? threshold? |
| Limits | Assumes feature independence; averages can hide interaction/subgroup effects (see ICE curves for per-row views) |

## Feature Impact & Confidence Intervals

| Concept | Key Point |
|---|---|
| Global feature impact | Ranking (e.g., mean |SHAP|, gain) of which features drive predictions overall |
| Local explanation | Per-prediction attributions (single SHAP row) for debugging/regulatory cases |
| Confidence interval | Range likely containing true parameter/performance at confidence level (e.g., 95%); narrower = more certainty |
| CI vs prediction interval | CI about mean estimate; PI about individual future observation (wider) |
| Use in validation | Report metric CIs (e.g., bootstrap AUC), not single point values |

## Exam Patterns

| Scenario/Keyword | Answer |
|---|---|
| "attributions that sum to difference from average" | SHAP/Shapley values |
| "Snowflake registry method returning explanations" | `mv.run(data, function_name="explain")` / `!EXPLAIN` in SQL |
| "background data used for?" | Reference distribution ("average" inputs) Shapley compares against |
| "average effect of feature across dataset" | Partial dependence plot |
| "range estimate of coefficient/performance" | Confidence interval |
| "finance/healthcare needs reason for decision" | Model explainability — SHAP local attributions |
| Explainability not on old logged model | Log NEW version (immutable versions), pass sample_input_data |

## Related Notes

- [hyperparameter-tuning-and-validation](hyperparameter-tuning-and-validation.md)
- [model-training-pipelines](model-training-pipelines.md)

## Source Documents

- [Model Explainability | Snowflake Documentation](../../sources/downloads/domain-3-model-development/model-explainability-snowflake-documentation.md)
