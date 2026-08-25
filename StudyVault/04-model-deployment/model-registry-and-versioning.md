---
source_pdf: domain-4-model-deployment/snowflake-model-registry-snowflake-documentation.md
part: "4.0"
keywords: model registry, log_model, model versioning, SHOW MODELS, default version
---
# Model Registry and Versioning (★★★)
#domain-4 #model-registry

## Overview Table
| Item | Key Point |
|---|---|
| What | Secure management of models + metadata as **first-class schema-level objects** in Snowflake |
| Main API classes | `snowflake.ml.registry.Registry`, `snowflake.ml.model.Model`, `snowflake.ml.model.ModelVersion` |
| Register = "log" | `log_model()` serializes (pickles) the Python object, stores artifacts on an internal stage, adds metadata |
| Versions | Unlimited per model; any string naming convention; name+version unique per schema |
| Default deployment | Warehouse inference via UDF-based methods (`mv.run()`, SQL `MODEL(m)!method`) |
| Governance | RBAC privileges, tags, comments, metrics; monitored via ML Observability |

## Logging a Model
```python
from snowflake.ml.registry import Registry

reg = Registry(session=sp_session, database_name="ML", schema_name="REGISTRY")
mv = reg.log_model(
    clf,                                  # picklable Python model object
    model_name="my_model",
    version_name="v1",
    conda_dependencies=["scikit-learn"],  # deployed with the model
    metrics={"test_accuracy": 0.96},
    sample_input_data=train_features,     # derives signature; OR use signatures=
    comment="Churn classifier v1",
)
```

| `log_model` argument | Purpose |
|---|---|
| `model`, `model_name` | Required. Picklable object; schema-wide identifier (name immutable after logging) |
| `version_name` | Optional string; auto-generated if missing |
| `conda_dependencies` | Conda packages deployed with model; Snowflake channel assumed in warehouse, conda-forge for SPCS |
| `sample_input_data` / `signatures` | One required (except Snowpark ML/MLflow/HF pipeline models) — defines method signatures |
| `metrics` | Dict of key-value metrics attached to the version |
| `task` | e.g., `task.Task.TABULAR_BINARY_CLASSIFICATION`; required to use ML Observability |
| `target_platforms` | `"WAREHOUSE"` and/or `"SNOWPARK_CONTAINER_SERVICES"`; fails if not runnable there |
| `options` | e.g., `relax_version`, `function_type`, `volatility`, `method_options` |

> [!warning]
> Tags cannot be set inside `log_model` — the first logged version creates the model object; set tags afterward with `m.set_tag(...)`.

## Retrieving Models and Versions
| Operation | Python API | SQL |
|---|---|---|
| List models | `reg.show_models()` → pandas DF; `reg.models()` → list | `SHOW MODELS [LIKE ...] [IN SCHEMA ...]` |
| Get one model | `m = reg.get_model("MyModel")` (a **reference**, not a copy) | — |
| List versions | `m.versions()` / `m.show_versions()` | `SHOW VERSIONS IN MODEL my_model` |
| Get a version | `mv = m.version("v1")` or `mv = m.default` | `MODEL(m)!predict(...)`, alias for specific version |
| Methods available | `mv.show_functions()` | `SHOW FUNCTION IN MODEL m VERSION v` |

## Version Lifecycle: Default, Aliases, Rollback
```python
m.default          # current default version (ModelVersion object)
m.default = "v2"   # promote v2 -> default  (= rollback point)
m.delete_version("rc1")
```

```sql
ALTER MODEL my_model SET DEFAULT_VERSION = V1;   -- rollback: repoint default
ALTER MODEL my_model DROP VERSION rc1;
```

- System aliases usable wherever a version name is expected: `DEFAULT`, `FIRST` (oldest), `LAST` (newest).
- Custom aliases assigned via `ALTER MODEL`; an alias maps to exactly one version at a time.
- Rollback pattern: old versions are retained → set default back to previous version; consumers using unqualified model reference instantly get it.

> [!warning]
> The model itself is immutable after logging (artifacts cannot change); only metadata (comment, tags, metrics, default version) is mutable. "Updating" a model always means logging a new version.

## Metadata: Comments, Tags, Metrics
| Metadata | Set/Get | Notes |
|---|---|---|
| Comment | `m.comment = "..."`, `mv.comment` | `description` is synonym; `ALTER MODEL ... SET COMMENT` in SQL |
| Tags | `m.set_tag("live_version","v1")`, `get_tag`, `show_tags`, `unset_tag` | Tag names must pre-exist via `CREATE TAG` (object tagging); defined allowed values → governance |
| Metrics | `mv.set_metric("auc", 0.91)`, `show_metrics`, `delete_metric` | Any JSON-serializable value (scalar/dict/matrix); no predefined names needed |

- Tags = governance/lifecycle labels (dev/staging/prod); metrics = evaluation numbers tied to a version.

## Privileges
| Privilege | Grants |
|---|---|
| `CREATE MODEL` on schema (or ownership) | Create/log models |
| USAGE on model | Warehouse inference only; no visibility into internals/artifacts |
| READ on model | SPCS inference **plus** metadata visibility (comments, tags, metrics) |

```sql
GRANT USAGE ON ALL MODELS IN SCHEMA ml.registry TO ROLE scientist;
GRANT READ  ON FUTURE MODELS IN SCHEMA ml.registry TO ROLE auditor;
```

## Limits and Storage Facts
| Fact | Value |
|---|---|
| Max versions per model | 1000 |
| Max methods per version | 10 (500 args each) |
| Max total model size (warehouse deploy) | 15 GB |
| Metadata incl. metrics | 100 KB |
| Artifact access | Internal stage via `snow://model/<name>/versions/<ver>/` URLs; `LIST`/`GET` |

## Exam Patterns
| Scenario/Keyword | Answer |
|---|---|
| "Register trained sklearn/xgboost model" | `Registry(...).log_model(...)` with `conda_dependencies` + `sample_input_data` |
| "Deploy new version safely, keep old serving" | Log same `model_name`, new `version_name`; switch `default` when validated |
| "Rollback bad production model" | `m.default = "v1"` or `ALTER MODEL ... SET DEFAULT_VERSION` |
| "List all registered models from SQL" | `SHOW MODELS;` |
| "See versions/functions of a model" | `SHOW VERSIONS IN MODEL` / `mv.show_functions()` |
| "Attach accuracy score to a version" | `mv.set_metric(...)` (or `metrics=` at log time) |
| "Mark which version is production" | Tag via `set_tag` (tag created with `CREATE TAG`) |
| "Grant team inference without exposing internals" | USAGE privilege (READ only if SPCS/metadata needed) |
| "FORECAST model in registry?" | No — ML functions bypass the registry; Cortex fine-tuned LLMs show in UI but not managed by API |

## Related Notes
- [Deployment Patterns](deployment-patterns.md)
- [Drift Monitoring and Retraining](drift-monitoring-and-retraining.md)
- [Practice: Model Deployment](model-deployment-practice.md)

## Source Documents
- ../../sources/downloads/domain-4-model-deployment/snowflake-model-registry-snowflake-documentation.md
- ../../sources/downloads/domain-4-model-deployment/snowflake-model-registry-snowflake-documentation/show-models-snowflake-documentation.md
- ../../sources/downloads/domain-4-model-deployment/snowflake-model-registry-snowflake-documentation/snowflake-native-batch-inference-sql-snowflake-documentation.md
