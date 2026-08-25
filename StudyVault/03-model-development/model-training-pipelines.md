---
source_pdf: domain-3-model-development/training-machine-learning-models-with-snowpark-python-snowflake-docume.md
part: "3.0"
keywords: udf, udtf, stored procedure, dynamic tables, task streams
---
# Model Training Pipelines (★★★)

#domain-3 #model-training

## Overview Table

| Item | Key Point |
|---|---|
| Pipeline stages | raw → transform → train → validate → register (see diagram) |
| Transformation automation | Dynamic Tables (declarative materialized), Tasks + Streams, UDFs/UDTFs, stored procedures |
| Training in Snowflake | Python stored procedure (single-node on Snowpark-optimized warehouse); UDTF-based distributed training; Snowpark ML estimators |
| Training outside Snowflake | External functions call external API endpoints (e.g., SageMaker-hosted training) |
| Inference | Vectorized UDFs (batch pandas) — best ML inference performance |

## Pipeline Flow Diagram

```
 RAW DATA            TRANSFORM                TRAIN                  VALIDATE              REGISTER
+-----------+   +------------------+   +---------------------+   +----------------+   +------------------+
| raw tables|   | Dynamic Table /  |   | Python Stored Proc  |   | hold-out / CV  |   | Model Registry   |
| streams ->|-->| Task+Stream DAG  |-->>| (Snowpark-optimized |-->| metrics: AUC,  |-->| versioned model  |
| ext tables|   | SQL / UDF /UDTF  |     | warehouse) or UDTF  |   | RMSE, log loss |   | + explainability |
+-----------+   +------------------+   +---------------------+   +----------------+   +------------------+
        ^                                                        |
        |                inference: vectorized UDF on new data <-+
```

## SP vs UDF vs UDTF — Decision Tree

```
Need output per row/group inside a SELECT?
├─ No → orchestration, DDL/DML, multi-step?  --> STORED PROCEDURE (CALL proc();)
└─ Yes
   ├─ one scalar value per row             --> UDF      (SELECT f(col) FROM t)
   └─ set of rows per partition/group      --> UDTF     (TABLE(f(col)) join)
```

### Decision Table

| Criterion | Stored Procedure | UDF | UDTF |
|---|---|---|---|
| Returns | Scalar (or tabular where supported), optional | Value per row | Rows per partition |
| Called as | Standalone `CALL` (not usable in expression) | Inside any SQL expression | In FROM clause with `TABLE(...)` |
| DDL/DML allowed | Yes (SELECT, UPDATE, CREATE...) | Queries only | Queries only |
| Statements per CALL | One procedure per statement (can nest others) | Many UDFs per statement | One per FROM slot |
| Typical DS use | Train model, orchestrate pipeline, write artifacts to stage | Per-row inference/scoring | Batch/partition training or inference, per-group processing |
| Rights model | Owner's rights (default) or caller's rights | Invoker context | Invoker context |

## Automation of Data Transformation

| Mechanism | Key Point |
|---|---|
| Dynamic Tables | Declarative `TARGET_LAG` + `WAREHOUSE`; Snowflake auto-refreshes materialized result via DAG — no manual task chaining |
| CREATE TASK | Scheduled or DAG (`AFTER` parent tasks); runs single SQL/statement; created **suspended**, must RESUME |
| Streams | CDC change records on table; consumed by DML advancing offset |
| SYSTEM$STREAM_HAS_DATA | `WHEN` guard on task so it skips run when stream empty — avoids needless warehouse start |

```sql
CREATE TASK t_train
  WAREHOUSE = wh
  SCHEDULE = '1 HOUR'
  WHEN SYSTEM$STREAM_HAS_DATA('raw_stream')
AS CALL train_proc();

ALTER TASK t_train RESUME;  -- tasks start suspended
```

> [!warning] Caveats
> - SYSTEM$STREAM_HAS_DATA avoids false negatives but NOT false positives; if TRUE you must consume the stream (any DML) or the guard keeps firing.
> - Dynamic Tables ≠ Streams: DT = declaratively maintained result; stream = change-data-capture feed.

## Python Stored Procedures for Training

| Guideline | Detail |
|---|---|
| Warehouse | Snowpark-optimized, `WAREHOUSE_SIZE='MEDIUM'` = 1 node; don't mix other workloads |
| Nested queries | Use separate query warehouse (`session.use_warehouse`) tuned to data size |
| Pattern | Load `.to_pandas()` → split → sklearn Pipeline/GridSearchCV fit → joblib dump → `session.file.put(... '@ml_models')` |
| Return | VARIANT (e.g., R² on train/test) |

```sql
CREATE OR REPLACE PROCEDURE train()
  RETURNS VARIANT LANGUAGE PYTHON RUNTIME_VERSION = 3.12
  PACKAGES = ('snowflake-snowpark-python','scikit-learn','joblib')
  HANDLER = 'main'
AS $$
def main(session):
    df = session.table('FEATURES').to_pandas()
    ...
    return {"R2_train": model.score(X_train, y_train)}
$$;
CALL train();
```

## UDF Registration and Vectorized Batches

```python
from snowflake.snowpark.functions import udf, pandas_udf
from snowflake.snowpark.types import PandasDataFrameType, FloatType

# scalar UDF via decorator
@udf(name="score_row", replace=True,
     is_permanent=True, stage_location="@ml_models",
     packages=["snowflake-snowpark-python", "scikit-learn"])
def predict(x: float) -> float:
    return float(model.predict([[x]])[0])

# vectorized UDF: batches as pandas DataFrames (best ML inference perf)
@pandas_udf(name="score_batch", replace=True,
            input_types=[PandasDataFrameType([float, float])],
            return_type=FloatType())
def batch_predict(df):
    return pd.Series(model.predict(df))
```

| UDF design rule | Why |
|---|---|
| Expensive init (load model file) in module scope, not handler | Runs once per invocation context, not per row |
| Handler thread-safe, avoid cross-row shared state | Rows may be processed in any order across instances/threads |
| Single-threaded handlers | Snowflake partitions/scales data across warehouse |
| NULL handling | SQL NULL ↔ Python None (VARIANT JSON null also → None) |
| Memory/time limits | Errors if exceeded → use Snowpark-optimized warehouses |

## Training Outside Snowflake — External Functions

| Item | Key Point |
|---|---|
| External function | SQL-callable wrapper invoking an external API endpoint (via API integration/proxy: AWS/API Gateway etc.) |
| Use case | Send features out to a service that trains or scores remotely; results return to Snowflake |

## Exam Patterns

| Scenario/Keyword | Answer |
|---|---|
| "orchestrate multi-step, DML/DDL, schedule training" | Python stored procedure (+ Task scheduling) |
| "return value used in SELECT per row" | UDF |
| "return multiple rows per group/partition" | UDTF |
| "automated declarative refresh of transformed table" | Dynamic Table (TARGET_LAG) |
| "task should skip when no new data" | WHEN SYSTEM$STREAM_HAS_DATA(stream) |
| "fastest ML inference in SQL" | Vectorized UDF (pandas batches) |
| "train on single big-memory node" | Stored procedure on Snowpark-optimized warehouse |
| "call external REST endpoint from SQL" | External function |
| "newly created task state" | Suspended — must ALTER TASK ... RESUME |

## Related Notes

- [connecting-tools-to-snowflake](connecting-tools-to-snowflake.md)
- [hyperparameter-tuning-and-validation](hyperparameter-tuning-and-validation.md)

## Source Documents

- [Training Machine Learning Models with Snowpark Python](../../sources/downloads/domain-3-model-development/training-machine-learning-models-with-snowpark-python-snowflake-docume.md)
- [Choosing whether to write a stored procedure or a user-defined function](../../sources/downloads/domain-3-model-development/choosing-whether-to-write-a-stored-procedure-or-a-user-defined-functio.md)
- [Stored procedures overview](../../sources/downloads/domain-3-model-development/stored-procedures-overview-snowflake-documentation.md)
- [Designing Python UDFs](../../sources/downloads/domain-3-model-development/designing-python-udfs-snowflake-documentation.md)
- [CREATE TASK](../../sources/downloads/domain-3-model-development/create-task-snowflake-documentation.md)
- [SYSTEM$STREAM_HAS_DATA](../../sources/downloads/domain-3-model-development/system-stream-has-data-snowflake-documentation.md)
