---
source_pdf: domain-2-data-preparation-and-feature-engineering/snowflake-feature-store-snowflake-documentation.md
part: "2.0"
keywords: feature store, feature view, entity, dataset, snowflake notebooks, scheduling
---
# Snowpark Feature Store & Notebooks (★★◎)
#domain-2 #feature-store
## Overview Table
| Item | Key Point |
|---|---|
| Feature Store | Central, governed, reusable repository of features inside Snowflake (Objective 2.3) |
| Backing objects | Store = **schema**; feature view = **dynamic table** (managed) or **view** (external); entity = **tag**; feature = column |
| FeatureView | Encapsulates a Python (Snowpark) or SQL transform of raw data into related features |
| Managed vs external | `refresh_freq` set → Snowflake refreshes the feature table automatically (dynamic table); `refresh_freq=None` → you own the pipeline (e.g. dbt) |
| Datasets | Point-in-time correct training/inference data built from a **spine** (entity keys + timestamps) |
| Snowflake Notebooks | Cell-based SQL/Python/Markdown dev environment; schedule as a **task**; packages via Anaconda channel |
## Pipeline Diagram
```
 RAW SOURCE ──▶ feature transformation (Snowpark DF / SQL)
   table/view      GroupBy, window, derive              Feature Store schema
      │                     │                                  │
      ▼                     ▼                                  ▼
   stream/dbt ┄┄┄▶ FeatureView (refresh_freq) ──▶ feature table (dynamic table / view)
                                                          │
                                          Entity join_keys + spine dataframe
                                                          ▼
                                              Dataset ──▶ model training / inference
```
## Feature Store Concepts
| Object | Python API | What it is |
|---|---|---|
| Feature store | `FeatureStore(...)` (`snowflake.ml.feature_store`) | A schema; all objects stored in it; RBAC applies |
| Entity | `Entity(name=..., join_keys=[...])` | Subject of features (customer, product); defines join keys; implemented as a tag; immutable after creation |
| Feature view | `FeatureView(name, entities, feature_df, timestamp_col, refresh_freq)` | Transform pipeline; one pipeline per SQL/Python transform; all features refresh on the same schedule |
| Dataset | `fs.generate_dataset(name, spine_df, features, spine_timestamp_col)` | Immutable, ML-optimized dataset; tables/variables for scikit-learn/TensorFlow/PyTorch via `read.to_pandas()` |
```python
from snowflake.ml.feature_store import FeatureStore, Entity, FeatureView

fs = FeatureStore(session, database="ANALYTICS", name="CUSTOMER_FS", warehouse="ML_WH")
entity = Entity(name="CUSTOMER", join_keys=["CUSTOMER_ID"]); fs.register_entity(entity)

fv = FeatureView(name="CUST_SPEND", entities=[entity],
                 feature_df=spend_features,      # Snowpark DataFrame
                 timestamp_col="TX_DATE", refresh_freq="5 minutes")
fs.register_feature_view(feature_view=fv, version="1", block=True)

train_ds = fs.generate_dataset(name="SPEND_TRAIN", spine_df=spine,
                               features=[fv], spine_timestamp_col="ASOF_DATE")
train_pdf = train_ds.read.to_pandas()
```
- A feature store can combine feature views from multiple stores; `list_feature_views(entity_name=...)` filters by entity; Snowsight UI + Universal Search for discovery.
- **Point-in-time correctness** via SQL **ASOF JOIN** under the covers — features are fetched relative to each row's spine timestamp (no future leakage).
- Lineage: source → feature → dataset → model automatically retained (ML Lineage); models auto-retrieve correct feature values at inference.
## Managed vs External Feature Views
| Property | Snowflake-managed | External |
|---|---|---|
| Refresh | Snowflake, incrementally, on `refresh_freq` (min 1 minute / cron) | You or dbt maintain the feature table |
| Backing object | Dynamic table (storage cost) | View (no extra storage cost) |
| feature_df | Transform logic over raw source | Usually a simple projection of the feature table |
| Change tracking | Needed on source tables for incremental refresh (else `refresh_mode='FULL'`) | n/a |
| Discoverability | Feature descriptions via `attach_feature_desc({col: desc})` | same |
- Updating: `fs.update_feature_view(name, version, refresh_freq/warehouse/desc)`; **definitions/columns are immutable** — change features ⇒ create a new version. Deleting a version breaks pipelines using it.
- Postgres-backed online serving: combined feature-view name + version ≤ 46 chars (identifier truncation pitfalls).
## Snowflake Notebooks (Objective 2.4)
| Capability | Notes |
|---|---|
| Cells | Python, SQL, Markdown; SQL cell output refrerencable from Python cells |
| Compute | Warehouse Runtime (Python 3.9, Snowpark+Streamlit preinstalled) or Container Runtime (CPU/GPU, pip/conda) |
| Packages | Add from the **Snowflake Anaconda channel** (must accept terms; restart session after adding); or import from a stage (no wheels on warehouse runtime) |
| Scheduling | Create schedule ⇒ Snowsight creates a **task**; runs cells top-down, non-interactive; run history 7 days; needs EXECUTE TASK / CREATE TASK privileges |
| External scheduler | `EXECUTE NOTEBOOK DB.SCHEMA.NAME('--env staging')` (Airflow etc.) |
| Git / Jupyter | Notebooks in Workspaces = next-gen, Jupyter-compatible; features integrated w/ governed data |
- Keep Snowpark blocks in one cell so the whole chain pushes down; `show()`/`to_pandas()` are the materialization points.
## Exam Patterns
| Scenario/Keyword | Answer |
|---|---|
| "Register a reusable feature for multiple models" | Feature Store + `register_feature_view` |
| "Ensure training features don't use future data" | Point-in-time / ASOF JOIN via spine timestamps |
| "Who refreshes Snowflake-managed feature views?" | Snowflake, on `refresh_freq` (dynamic table) |
| "Register features from an external dbt pipeline" | External feature view, `refresh_freq=None` |
| "Find features for the CUSTOMER entity" | `list_feature_views(entity_name="CUSTOMER")` or Snowsight UI |
| "Run a notebook daily in production" | Schedule it (creates a task) |
| "Use a package not in Anaconda channel" | Import from a Snowflake stage (module/folder only) |
## Related Notes
- [data-preparation-with-snowpark](data-preparation-with-snowpark.md)
- [feature-engineering-techniques](feature-engineering-techniques.md)
- [visualization-in-snowsight](visualization-in-snowsight.md)
## Source Documents
- [Snowflake Feature Store overview](../../sources/downloads/domain-2-data-preparation-and-feature-engineering/snowflake-feature-store-snowflake-documentation.md)
- [Working with entities](../../sources/downloads/domain-2-data-preparation-and-feature-engineering/snowflake-feature-store-snowflake-documentation/working-with-entities-snowflake-documentation.md)
- [Working with feature views](../../sources/downloads/domain-2-data-preparation-and-feature-engineering/snowflake-feature-store-snowflake-documentation/working-with-feature-views-snowflake-documentation.md)
- [Advanced Guide to the Snowflake Feature Store](../../sources/downloads/domain-2-data-preparation-and-feature-engineering/snowflake-feature-store-snowflake-documentation/advanced-guide-to-snowflake-feature-store.md)
- [ASOF JOIN](../../sources/downloads/domain-2-data-preparation-and-feature-engineering/snowflake-feature-store-snowflake-documentation/asof-join-snowflake-documentation.md)
- [About Snowflake Notebooks](../../sources/downloads/domain-2-data-preparation-and-feature-engineering/about-legacy-snowflake-notebooks-snowflake-documentation.md)
- [Import Python packages in notebooks](../../sources/downloads/domain-2-data-preparation-and-feature-engineering/import-python-packages-to-use-in-notebooks-snowflake-documentation.md)
- [Schedule notebook runs](../../sources/downloads/domain-2-data-preparation-and-feature-engineering/schedule-notebook-runs-snowflake-documentation.md)
- [Getting Started with the Feature Store API](../../sources/downloads/domain-2-data-preparation-and-feature-engineering/snowflake-feature-store-snowflake-documentation/getting-started-with-snowflake-feature-store-api.md)