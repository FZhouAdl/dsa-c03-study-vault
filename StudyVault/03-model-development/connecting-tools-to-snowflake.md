---
source_pdf: domain-3-model-development/using-pandas-dataframes-with-the-python-connector-snowflake-documentat.md
part: "3.0"
keywords: snowpark, python connector, pandas, snowpark ml, IDE connectivity
---
# Connecting Tools to Snowflake (★★)

#domain-3 #connectivity

## Overview Table

| Item | Key Point |
|---|---|
| Python Connector | DB driver (DBAPI). Runs SQL, fetches results; pandas support via `fetch_pandas_all()` / `fetch_pandas_batches()` |
| Snowpark | DataFrame API with **pushdown**: transformations compile to SQL, run on warehouse compute; client only gets results |
| Snowpark ML (`snowflake.ml`) | `snowflake.ml.modeling` sklearn-style estimators + MLOps: Model Registry, Feature Store |
| External IDE (VS Code) | Connect via connection definitions (`connections.toml`) / Snowflake extension; same account URL + auth as any driver |
| Snowpark languages | Scala, Java, Python (Python most common for DS) |

## Python Connector vs Snowpark

| Aspect | Python Connector | Snowpark |
|---|---|---|
| Nature | Low-level database driver | DataFrame/SQL abstraction layer (built on connector) |
| Compute location | Data pulled to client for pandas ops | Pushdown — compute in Snowflake warehouse |
| Pandas support | `cursor.fetch_pandas_all()`, `fetch_pandas_batches()`, `write_pandas()` | `.to_pandas()` on DataFrame; vectorized UDFs consume pandas batches |
| Install | `pip install "snowflake-connector-python[pandas]"` (needs PyArrow) | `pip install snowflake-snowpark-python` |
| Best for | Scripts, ad-hoc pulls, writing local DataFrames back | Pipelines at scale, feature engineering near data |

```python
# Connector + pandas
import snowflake.connector
conn = snowflake.connector.connect(account=..., user=..., ...)
cur = conn.cursor().execute("SELECT * FROM train")
df = cur.fetch_pandas_all()          # or fetch_pandas_batches()
```

```python
# Snowpark pushdown
from snowflake.snowpark import Session
session = Session.builder.configs({...}).create()
df = session.table("train").filter(col("amt") > 0)   # lazy, compiled to SQL
df.group_by("segment").agg(sum("amt")).show()        # runs in warehouse
```

## Snowpark ML

| Component | Purpose |
|---|---|
| `snowflake.ml.modeling.*` | Drop-in sklearn-style estimators/preprocessors operating on Snowpark DataFrames (distributed versions of fit/transform/predict) |
| Model Registry | Versioned model storage in schema; deploy as vectorized UDF for inference |
| Feature Store | Managed feature definitions, materialization, lineage |

## Connecting from an IDE (VS Code)

| Item | Key Point |
|---|---|
| Auth | Account identifier URL + SSO/MFA/key-pair; same as drivers |
| Config | `~/.snowflake/connections.toml` named connections; Snowflake VS Code extension uses these |
| Worksheets vs notebooks | Snowsight Python worksheets/notebooks run server-side; VS Code runs client-side against session |

## Exam Patterns

| Scenario/Keyword | Answer |
|---|---|
| "pandas support", "fetch_pandas" | Python Connector (with `[pandas]` extra / PyArrow) |
| "pushdown", "transformations run where?" | Snowpark DataFrame → compiled SQL on warehouse |
| "sklearn-style estimators in Snowflake" | Snowpark ML (`snowflake.ml.modeling`) |
| "versioned models + inference deployment" | Snowpark ML Model Registry |
| Which Snowpark language? | Scala / Java / Python all supported |

## Related Notes

- [cortex](cortex-genai-and-llms.md)
- [model-training-pipelines](model-training-pipelines.md)

## Source Documents

- [Using pandas DataFrames with the Python Connector](../../sources/downloads/domain-3-model-development/using-pandas-dataframes-with-the-python-connector-snowflake-documentat.md)
- [Training Machine Learning Models with Snowpark Python](../../sources/downloads/domain-3-model-development/training-machine-learning-models-with-snowpark-python-snowflake-docume.md)
