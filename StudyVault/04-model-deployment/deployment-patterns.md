---
source_pdf: domain-4-model-deployment/snowflake-model-registry-snowflake-documentation.md
part: "4.0"
keywords: model deployment, vectorized UDF, external functions, Snowpark Container Services, batch inference
---
# Deployment Patterns (★★★)
#domain-4 #model-deployment

## Overview Table
| Item | Key Point |
|---|---|
| Default deployment | Model Registry logs model → inference runs as UDF-based method in a **virtual warehouse** |
| Vectorized Python UDF | Receives **pandas DataFrame batches**, returns pandas array/Series → higher throughput |
| External hosted model | Called via **external functions** through an API integration (e.g., SageMaker endpoint) |
| Pre-built models | `snowflake.ml.modeling` (Anaconda channel) + SQL ML functions (`SNOWFLAKE.ML.CLASSIFICATION`, FORECAST) |
| SPCS | Containerized serving, GPU/large models, REST endpoints via `create_service()` |
| Storing predictions | `CREATE TABLE AS SELECT ... !PREDICT(...)` or Dynamic Tables for continuous inference |

## Deployment Decision Flow
```text
                    Where should the model run?
                              |
        +---------------------+----------------------+
        |                     |                      |
  Trained in Python      Model hosted         Need GPU / >15GB model /
  (sklearn, xgboost...)  outside Snowflake    custom env / low-latency HTTP?
        |                     |                      |
        v                     v                      v
  SNOWFLAKE MODEL        EXTERNAL FUNCTIONS     SNOWPARK CONTAINER
  REGISTRY               via API integration    SERVICES (SPCS)
  log_model -> UDF       -> proxy -> remote     mv.create_service()
  mv.run() / SQL         endpoint (SaaS/        REST endpoint on
  MODEL(m)!predict       SageMaker)             compute pool
        |                                        |
  CPU model <15GB,                          GPU/large models,
  batch scoring in warehouse                online real-time inference
```

## In-Snowflake: Registry-Based Warehouse Inference
| Step | Command/API |
|---|---|
| Open registry | `Registry(session=sp_session, database_name="ML", schema_name="REGISTRY")` |
| Log (register) | `reg.log_model(clf, model_name="m", version_name="v1", conda_dependencies=[...])` |
| Inference (Python) | `mv.run(test_features, function_name="predict")` — executes **in a warehouse** |
| Inference (SQL) | `SELECT MODEL(my_model)!predict(...) FROM my_table;` |
| Specific version | `SELECT MODEL(my_model, LAST)!predict(...) FROM my_table;` |

```python
from snowflake.ml.registry import Registry

reg = Registry(session=sp_session, database_name="ML", schema_name="REGISTRY")
mv = reg.log_model(clf, model_name="my_model", version_name="v1",
                   conda_dependencies=["scikit-learn"],
                   sample_input_data=train_features)  # default target platform: WAREHOUSE
preds = mv.run(test_features, function_name="predict")  # lazy for Snowpark DFs
```

> [!warning]
> `mv.run()` executes in the session's warehouse. Snowpark DataFrames are lazy — inference actually runs only when `collect()`, `show()`, or `to_pandas()` is called.

## Scalar vs Vectorized Python UDFs
| Aspect | Scalar UDF | Vectorized UDF |
|---|---|---|
| Input granularity | One row at a time | **pandas DataFrame batches** (up to a few thousand rows each) |
| Output | Single value | pandas array/Series with **same length as input DataFrame** |
| Throughput | Lower (per-row call overhead) | Potentially better; best when code is efficient on batches |
| Annotation | Plain handler | `@vectorized(input=pandas.DataFrame)` or `_sf_vectorized_input` attribute |
| Batch control | n/a | `max_batch_size=N` caps rows per input DataFrame |
| Time limit | — | Handler call must complete within **180 seconds** |

```sql
CREATE FUNCTION predict_batch(x FLOAT, y FLOAT)
  RETURNS FLOAT LANGUAGE PYTHON RUNTIME_VERSION = 3.12
  PACKAGES = ('pandas') HANDLER = 'predict_batch'
AS $$
import pandas
from _snowflake import vectorized

@vectorized(input=pandas.DataFrame, max_batch_size=1000)
def predict_batch(df):
    return df[0] + df[1]   # arguments accessed by index: df[0], df[1]
$$;
```

- Calling code unchanged — all batching handled by the UDF framework.
- NULL FLOAT encodes as NaN on input; NaN output is interpreted back as NULL.

> [!warning]
> There is no guarantee which handler instance sees which batch — keep vectorized handlers stateless and order-independent.

## Pre-built Models
| Option | Where it lives | Notes |
|---|---|---|
| `snowflake.ml.modeling` classes | Anaconda channel | sklearn/XGBoost-style API; fully loggable to Model Registry |
| `SNOWFLAKE.ML.CLASSIFICATION` | SQL ML function class | Gradient boosting; AUC loss (binary), logistic loss (multi-class); evaluation via `!SHOW_EVALUATION_METRICS()` etc. |
| FORECAST and other SQL ML functions | SQL classes | Train/infer entirely in SQL |

> [!warning]
> SQL ML functions (e.g., `SNOWFLAKE.ML.CLASSIFICATION`, FORECAST) do **not** appear in the Model Registry. Registry works with Python-trained models from the Snowflake ML ecosystem.

## External Hosted Models (External Functions)
| Item | Key Point |
|---|---|
| What | Function whose logic executes **outside Snowflake**, e.g., cloud-provider ML API |
| Plumbing | API integration → external cloud proxy/gateway → remote model endpoint |
| Use when | Model cannot be ported into Snowflake; proprietary hosted API; existing serving stack (e.g., SageMaker) |
| Trade-offs | Network latency per row, external availability dependency, data leaves the Snowflake boundary, API costs |

> [!warning]
> Objective wording "external hosted model → external functions" is simplified: external functions require an API integration plus a relay service (e.g., API Gateway/Lambda) in front of the remote endpoint; you never point Snowflake directly at the model host.

## Storing Predictions
```sql
CREATE OR REPLACE TABLE my_predictions AS
SELECT *, my_model!PREDICT(INPUT_DATA => {*}) AS predictions
FROM prediction_purchase_data;

SELECT predictions:class AS predicted_class,
       predictions['probability']['purchase'] AS p_purchase
FROM my_predictions;
```

- Continuous variant: Dynamic Table calling `MODEL(...)!predict(...)`, refresh every TARGET_LAG window, processes only new rows (INCREMENTAL).
- Incremental refresh requires invoked model functions to be **IMMUTABLE** (custom models default to VOLATILE).

## Stage Commands & Model Artifacts
| Command | Purpose |
|---|---|
| `LIST 'snow://model/my_model/versions/V3/'` | View artifacts (MANIFEST.yml, model.zip, env/conda.yml, function .py files) |
| `GET 'snow://model/<model>/versions/<version>/MANIFEST.yml' file:///tmp/x/` | Download artifact |
| `session.file.get("snow://model/...", "local_dir")` | Snowpark Python equivalent |

- Registry stores models as **first-class schema-level objects** backed by staged artifacts plus metadata.
- Artifacts are immutable; normal stage-path syntax does not work — use `snow://` URLs.
- Viewing/downloading artifacts requires **ownership**; USAGE privilege alone is not enough.

## Snowpark Container Services (SPCS)
| Aspect | Detail |
|---|---|
| What | Model deployed as managed service in an SPCS compute pool; optional public HTTP endpoint |
| Best for | Large (>15 GB) models, GPU inference, custom pip/OS environments, low-latency online inference |
| Deploy API | `mv.create_service(service_name=..., service_compute_pool=..., ingress_enabled=True, gpu_requests=...)` |
| Invoke | REST HTTP endpoint (port 5000) or SQL `service_name!method_name(...)` |
| Dependencies | SPCS builds pull from **conda-forge**; Snowflake conda channel exists only in warehouses |

> [!warning]
> Warehouse runtime is CPU-only and capped around a 15 GB total model size; GPU needs, larger models, or custom OS-level dependencies push deployment to SPCS.

## Exam Patterns
| Scenario/Keyword | Answer |
|---|---|
| "Batch-score millions of rows with a sklearn model inside Snowflake" | Log to Model Registry → `mv.run()` / SQL `MODEL(m)!predict()` on warehouse |
| "UDF that receives pandas DataFrames for better throughput" | Vectorized Python UDF (`@vectorized(input=pandas.DataFrame)`) |
| "Model served by SageMaker / external API, called from SQL" | External function via API integration |
| "GPU / deep learning / low-latency HTTP serving" | Snowpark Container Services (`create_service()`) |
| "Persist predictions for exploration" | `CREATE TABLE ... AS SELECT ...!PREDICT(...)` |
| "Inspect/download serialized model files" | `LIST` / `GET` on `snow://model/...` URLs |
| "Is a FORECAST/classification ML function visible in registry?" | No — SQL ML functions stay outside the Model Registry |

## Related Notes
- [Model Registry and Versioning](model-registry-and-versioning.md)
- [Drift Monitoring and Retraining](drift-monitoring-and-retraining.md)
- [Practice: Model Deployment](model-deployment-practice.md)

## Source Documents
- ../../sources/downloads/domain-4-model-deployment/snowflake-model-registry-snowflake-documentation.md
- ../../sources/downloads/domain-4-model-deployment/vectorized-python-udfs-snowflake-documentation.md
- ../../sources/downloads/domain-4-model-deployment/classification-snowflake-ml-functions-snowflake-documentation.md
- ../../sources/downloads/domain-4-model-deployment/snowflake-model-registry-snowflake-documentation/snowflake-native-batch-inference-sql-snowflake-documentation.md
- ../../sources/downloads/domain-4-model-deployment/snowflake-model-registry-snowflake-documentation/deploy-models-for-real-time-inference-rest-api-snowflake-documentation.md
- ../../sources/downloads/domain-4-model-deployment/extending-snowflake-with-functions-and-procedures-snowflake-documentat.md
