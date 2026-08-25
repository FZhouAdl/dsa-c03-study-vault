---
source_pdf: sources/SnowProDataScientistStudyGuide.pdf
part: "1.0"
keywords: ml lifecycle, mlops, model deployment, model monitoring, model versioning
---
# ML Lifecycle and MLOps (★★★)
#domain-1 #ml-lifecycle

## Overview Table

| Item | Key Point |
|---|---|
| Lifecycle stages (in order) | Data collection → Data visualization & exploration → Feature engineering → Training models → Model deployment → Model monitoring & evaluation |
| Monitoring metrics | Explainability, precision, recall, accuracy, confusion matrix |
| Model versioning | Track models/datasets over time; Snowflake Model Registry |
| MLOps | Automates + operationalizes the loop: retrain, redeploy, monitor |

## Lifecycle Stages (Objective 1.3)

```
+------------------+     +---------------------+     +---------------------+
| Data collection  | --> | Visualization &     | --> | Feature engineering |
+------------------+     | exploration         |     +---------------------+
                         +---------------------+              |
                                                              v
+------------------+     +---------------------+     +---------------------+
| Model monitoring | <-- | Model deployment    | <-- | Training models     |
| & evaluation     |     +---------------------+     +---------------------+
+------------------+          ^                                |
                              |        feedback/retrain        |
                              +--------------------------------+
```

| Stage | What Happens | Exam Cue |
|---|---|---|
| **Data collection** | Gather raw data from sources into Snowflake | First step of every sequence question |
| **Data visualization & exploration** | EDA: profiling, initial patterns, distributions | Happens BEFORE feature engineering |
| **Feature engineering** | Scaling, encoding, derived features | After EDA, before training |
| **Training models** | Fit candidate models, tune hyperparameters | Uses engineered features |
| **Model deployment** | Serve predictions to applications | After a model is trained and validated |
| **Model monitoring & evaluation** | Track performance in production, detect drift | Last step; feeds retraining |

> [!warning]
> Sequence questions are common. The official sample answer order is: **Data Collection > Visualization/Exploration > Feature Engineering > Training > Deployment > Monitoring**. Wrong options swap training/deployment before exploration or put monitoring before deployment.

## Model Monitoring & Evaluation

### Confusion Matrix (binary classification)

|  | Predicted Positive | Predicted Negative |
|---|---|---|
| **Actual Positive** | True Positive (TP) | False Negative (FN) |
| **Actual Negative** | False Positive (FP) | True Negative (TN) |

| Metric | Formula | Meaning |
|---|---|---|
| **Accuracy** | (TP + TN) / all | Overall share correct; misleading on imbalanced data |
| **Precision** | TP / (TP + FP) | Of predicted positives, how many were right |
| **Recall** | TP / (TP + FN) | Of actual positives, how many were caught |
| **F1** | 2·P·R / (P + R) | Harmonic mean of precision & recall |

> [!warning]
> Official sample: flight *not* overbooked, predicted *not* overbooked = **True Negative**. "Negative" class is whatever you define as the negative outcome — read the scenario's positive/negative definition before answering.

### Regression Evaluation

- **R²**: proportion of variability in the response variable explained by the linear association with explanatory variables. R² = 0.85 ⇒ 85% of variability explained.
- RMSE/MAE: average prediction error size.

### Model Explainability

- Models can be **black boxes**; explainability attributes predictions to input features.
- Snowflake Model Registry provides explainability via **Shapley values** (SHAP): average marginal contribution of each feature across all feature combinations; contributions can be positive or negative relative to background data.
- Important for regulated industries (finance, healthcare) needing to justify model decisions.

## Model Versioning & MLOps

- **Model versioning** keeps multiple iterations of models (+ metadata, lineage) so you can roll back and compare.
- In Snowflake: the **Model Registry** stores models as versioned objects with methods for inference and explainability.
- **MLOps** wraps the lifecycle in automation: CI/CD for models, scheduled retraining (e.g., Tasks), drift monitoring, governance.

## Exam Patterns

| Scenario/Keyword | Answer |
|---|---|
| Correct order of workload activities | Collect → Explore/Viz → Feature Engineering → Train → Deploy → Monitor |
| Not overbooked, predicted not overbooked | True Negative |
| Caught all fraud but many false alarms | High recall, low precision |
| R² = 0.85 interpretation | 85% of response variability explained by linear association |
| Attribute prediction to feature contributions | Shapley values / model explainability |
| Roll back to previous model iteration | Model versioning / Model Registry |
| Production accuracy degrading over time | Monitor for drift, retrain |

## Related Notes

- [ml-fundamentals](ml-fundamentals.md)
- [statistics-for-data-science](statistics-for-data-science.md)
- [practice questions](data-science-concepts-practice.md)

## Source Documents

- [snowpro-data-scientist-study-guide.txt](../../sources/snowpro-data-scientist-study-guide.txt) (Domain 1.0 objectives lines 134–180; sample questions lines 402–470)
- [Model Explainability | Snowflake Documentation](../../sources/downloads/domain-3-model-development/model-explainability-snowflake-documentation.md)
- [Snowflake Model Registry | Snowflake Documentation](../../sources/downloads/domain-4-model-deployment/snowflake-model-registry-snowflake-documentation.md)
