---
source_pdf: sources/SnowProDataScientistStudyGuide.pdf
part: all
keywords: MOC, study map, tag index, weak areas, DSA-C03
---
# DSA-C03 Study Map (MOC)

#dashboard

## Certification Overview

| Aspect | Detail |
|---|---|
| Exam | SnowPro Advanced: Data Scientist (DSA-C03) |
| Prerequisite | SnowPro Core Certified |
| Weight | D1 17% · D2 27% · D3 31% · D4 25% |
| Effort | ~10–13 hours guide + practice |
| Scope | Data science principles, Snowflake ML, data prep & features, model lifecycle, GenAI/Cortex |

## Topic Map

### Domain 1.0 — Data Science Concepts (17%)

| Topic | Level | Notes | Status |
|---|---|---|---|
| ML paradigms: supervised / unsupervised / reinforcement; problem types (regression, binary/multi-class, time-series, image, segmentation, clustering, GenAI association) | ★★★ | [ML Fundamentals](../01-data-science-concepts/ml-fundamentals.md) | [ ] |
| ML lifecycle: collection → exploration → feature eng → training → deployment → monitoring/evaluation → versioning | ★★★ | [ML Lifecycle & MLOps](../01-data-science-concepts/ml-lifecycle-and-mlops.md) | [ ] |
| Statistics: distributions & skew, CLT, z/t tests, bootstrapping, confidence intervals, variance | ★★★ | [Statistics for Data Science](../01-data-science-concepts/statistics-for-data-science.md) | [ ] |

### Domain 2.0 — Data Preparation & Feature Engineering (27%)

| Topic | Level | Notes | Status |
|---|---|---|---|
| Clean & prepare with Snowpark/SQL: aggregates, joins, dedupe, missing values, casting, sampling | ★★★ | [Data Preparation with Snowpark](../02-data-preparation-and-feature-engineering/data-preparation-with-snowpark.md) | [ ] |
| EDA: profiling, window functions, MIN/MAX/AVG/STDEV/VARIANCE, APPROX_* functions, SQL linear regression | ★★ | [Exploratory Data Analysis](../02-data-preparation-and-feature-engineering/exploratory-data-analysis.md) | [ ] |
| Feature engineering: scaling/encoding/normalization, binning, derived features, binarizing | ★★★ | [Feature Engineering Techniques](../02-data-preparation-and-feature-engineering/feature-engineering-techniques.md) | [ ] |
| Snowpark Feature Store + Snowflake Notebooks | ★★ | [Feature Store & Notebooks](../02-data-preparation-and-feature-engineering/feature-store-and-notebooks.md) | [ ] |
| Visualization: Snowsight, statistical summaries, outliers, graph libraries | ★★ | [Visualization in Snowsight](../02-data-preparation-and-feature-engineering/visualization-in-snowsight.md) | [ ] |

### Domain 3.0 — Model Development (31%)

| Topic | Level | Notes | Status |
|---|---|---|---|
| Connect tools: Snowpark, Snowpark ML, Python connector + pandas, external IDEs, snowpark languages | ★★ | [Connecting Tools to Snowflake](../03-model-development/connecting-tools-to-snowflake.md) | [ ] |
| Cortex GenAI/LLM: embeddings, prompt engineering, fine-tuning, task-specific models | ★★★ | [Cortex GenAI & LLMs](../03-model-development/cortex-genai-and-llms.md) | [ ] |
| Training pipelines: UDF/UDTF/stored procedures, dynamic tables, external functions, automation | ★★★ | [Model Training Pipelines](../03-model-development/model-training-pipelines.md) | [ ] |
| Hyperparameter tuning, metric selection, partitioning/CV, down/up-sampling; validation (ROC, residuals, metrics) | ★★★ | [Hyperparameter Tuning & Validation](../03-model-development/hyperparameter-tuning-and-validation.md) | [ ] |
| Interpretation: feature impact, partial dependence plots, confidence intervals, SHAP | ★★ | [Model Interpretation](../03-model-development/model-interpretation.md) | [ ] |

### Domain 4.0 — Model Deployment (25%)

| Topic | Level | Notes | Status |
|---|---|---|---|
| Deploy in production: vectorized/scalar UDFs, pre-built models, external functions, predict storage, stages, SPCS | ★★★ | [Deployment Patterns](../04-model-deployment/deployment-patterns.md) | [ ] |
| Model Registry: logging, retrieving, versioning, metadata, tags, rollback | ★★★ | [Model Registry & Versioning](../04-model-deployment/model-registry-and-versioning.md) | [ ] |
| Effectiveness & retraining: drift/model decay, distribution comparison, AUC/precision/recall/RMSE, automation | ★★★ | [Drift Monitoring & Retraining](../04-model-deployment/drift-monitoring-and-retraining.md) | [ ] |

## Practice Notes

| Set | Questions | Link |
|---|---|---|
| Domain 1 | 10 | [Data Science Concepts Practice](../01-data-science-concepts/data-science-concepts-practice.md) |
| Domain 2 | 12 | [Data Preparation & Feature Engineering Practice](../02-data-preparation-and-feature-engineering/data-preparation-feature-engineering-practice.md) |
| Domain 3 | 12 | [Model Development Practice](../03-model-development/model-development-practice.md) |
| Domain 4 | 10 | [Model Deployment Practice](../04-model-deployment/model-deployment-practice.md) |
| Official samples | 5 | In the guide itself (confusion matrix, APPX_TOP_K, R², sequence, notebook) |

## Study Tools

| Tool | Description | Link |
|---|---|---|
| Exam Traps | Common traps / wrong answers per domain | [Exam Traps](exam-traps.md) |
| Quick Reference | Formula & keyword cheat sheet | [Quick Reference](quick-reference.md) |

## Tag Index

Registry: `#dsa-c03` (top) → `#domain-N` (domain) → detail → technique → type. Detail tags always co-attach their `#domain-N`.

| Tag | Related Topics | Rule |
|---|---|---|
| `#dsa-c03` | Everything | Top-level, attaches to every note |
| `#domain-1` … `#domain-4` | Domain notes | Co-attach with every detail tag |
| `#ml-fundamentals` `#ml-lifecycle` `#statistics` | Domain 1 concepts | Detail |
| `#data-prep` `#eda` `#feature-engineering` `#feature-store` `#visualization` | Domain 2 concepts | Detail |
| `#connectivity` `#cortex-genai` `#model-training` `#model-validation` `#model-interpretation` | Domain 3 concepts | Detail |
| `#model-deployment` `#model-registry` `#drift-monitoring` | Domain 4 concepts | Detail |
| `#dashboard` `#practice` `#exam-traps` `#quick-reference` | Notes types | Type |

> **Tag rule**: `#dsa-c03` + `#domain-N` on all notes; add one detail tag per concept note. Practice/dashboard files use type tags — never invent unregistered tags.

## Weak Areas

- [ ] R² interpretation vs slope / correlation → [Statistics](../01-data-science-concepts/statistics-for-data-science.md) → [Exam Traps](exam-traps.md)
- [ ] APPROX_TOP_K vs APPROX_COUNT_DISTINCT vs APPROX_PERCENTILE → [EDA](../02-data-preparation-and-feature-engineering/exploratory-data-analysis.md) → [Exam Traps](exam-traps.md)
- [ ] Stored procedure vs UDF vs UDTF decision → [Training Pipelines](../03-model-development/model-training-pipelines.md) → [Exam Traps](exam-traps.md)
- [ ] Drift types (data vs concept vs label) → [Drift Monitoring](../04-model-deployment/drift-monitoring-and-retraining.md) → [Exam Traps](exam-traps.md)
- [ ] Workflow sequence order (monitoring after deployment) → [ML Lifecycle](../01-data-science-concepts/ml-lifecycle-and-mlops.md) → [Exam Traps](exam-traps.md)

## Non-core Topic Policy

| Source | Content | Handling |
|---|---|---|
| `domain-2/…/snowflake-resource-library*.md` | HubSpot video/whitepaper landing stubs (email-gated) | **Excluded** from notes — no usable content |
| `00-certification-overview/access-denied.md` | Login-gated ILT training page | **Excluded** |
| `00-certification-overview/*` (blog/webinars/lab index) | Marketing index pages | Included only in source-citation lists |
| Third-party links in guide (medium, investopedia, sklearn…) | Not downloaded (not official Snowflake) | Referenced as supplementary knowledge only |

## Related
- [Quick Reference](quick-reference.md) · [Exam Traps](exam-traps.md)