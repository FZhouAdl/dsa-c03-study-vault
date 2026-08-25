---
source_pdf: domain-2-data-preparation-and-feature-engineering/working-with-dataframes-in-snowpark-python-snowflake-documentation.md
part: "2.0"
keywords: snowpark dataframe, lazy evaluation, pushdown, deduplication, sampling, joins
---
# Data Preparation with Snowpark (★★★)
#domain-2 #data-prep
## Overview Table
| Item | Key Point |
|---|---|
| Snowpark DataFrame | Relational dataset evaluated **lazily** — a query definition, not data in memory |
| Lazy evaluation | Transformations build a SQL plan; nothing runs until an **action** (`collect`, `show`, `count`, `save_as_table`) |
| Pushdown | All computation transpiles to SQL and executes **in the warehouse**, not the client |
| Objective 2.1 tasks | Aggregate, Joins, Identify critical data, Remove duplicates, Remove irrelevant fields, Handle missing values, Cast types, Sample data |
| Result retrieval | `to_pandas()` pulls the result set to the client — required before local plotting (matplotlib/plotly) |
## Workflow Diagram
```
   ┌──────────┐   ┌──────────────┐   ┌───────────────┐   ┌────────────┐   ┌──────────────────┐
   │   LOAD   │──▶│    CLEAN     │──▶│   TRANSFORM   │──▶│   SAMPLE   │──▶│  FEATURE STORE   │
   └──────────┘   └──────────────┘   └───────────────┘   └────────────┘   └──────────────────┘
   session.table  drop_duplicates    group_by / agg     df.sample(...)     Entity + FeatureView
   session.read   fillna / dropna    joins               SQL SAMPLE         register_feature_view
   json/csv @stage cast types        derived columns     (BERNOULLI/SYSTEM) generate_dataset()
   session.sql    drop fields        filters / window    -> training data
```
## Creating DataFrames
| Source | Code |
|---|---|
| Table/view | `df = session.table("MY_TABLE")` — no data pulled until action |
| Local values | `session.create_dataframe([[1,"a"]], schema=["id","v"])` |
| Range | `session.range(1, 10, 2)` — handy for date grids |
| Staged files | `session.read.schema(s).csv("@stage/dir")`, `.json("@my_stage/data.json")` |
| SQL query | `session.sql("SELECT ...")` — prefer `table()`/`read` for tooling support |
## Cleaning Operations (Objective 2.1)
| Task | Snowpark Python | SQL equivalent |
|---|---|---|
| Remove duplicates | `df.drop_duplicates(["CUST_ID","TXN_ID"])` | `SELECT DISTINCT` / `ROW_NUMBER()` dedupe |
| Drop irrelevant fields | `df.drop("COL_A","COL_B")` or `select(...)` keep-list | `SELECT <col_list>` |
| Handle missing values | `df.fillna(value={"AMT": 0})`, `df.dropna(subset=["KEY"])` | `NVL`/`COALESCE`, `ZEROIFNULL`, `WHERE col IS NOT NULL` |
| Identify critical data | `df.filter(col("IS_ACTIVE") == True)`; `COUNT(*)/COUNT(col)` null-gap check | same |
| Type casting | `df.with_column("AMT", col("AMT").cast(T.DecimalType(10,2)))` | `CAST(x AS NUMBER(10,2))` / `::` |
| Aggregate | `df.group_by("REGION").agg(F.sum("AMT").as_("TOTAL"))` | `GROUP BY` |
```python
import snowflake.snowpark.functions as F
from snowflake.snowpark.types import DecimalType

df = (session.table("RAW_TXNS")
      .drop_duplicates(["TXN_ID"])
      .fillna({"AMOUNT": 0})
      .with_column("AMOUNT", F.col("AMOUNT").cast(DecimalType(12, 2)))
      .filter(F.col("STATUS") == "COMPLETED"))
```
> [!warning]
> Chaining order matters: each transformation returns a NEW DataFrame operating on the previous result. `select(...).filter(...)` differs from `filter(...).select(...)` when referencing derived columns. Missing-value handling varies by meaning — `fillna(0)` suits amounts/counts, `dropna(subset=[...])` suits key columns that must exist, mean/median imputation suits numeric features for modeling (see feature engineering note).
## Joins
| Join type | Syntax / note |
|---|---|
| Inner (default) | `lhs.join(rhs, lhs.col("K") == rhs.col("K"))` |
| Shared key names | `lhs.join(rhs, ["K"])` |
| Left / right / full outer | third arg of join: `"left"`, `"right"`, `"full_outer"` |
| Cross join | `df1.cross_join(df2)` or join with no condition |
| Self-join | Copy first: `from copy import copy; rhs = copy(lhs)` — direct `df.join(df,...)` cannot resolve column refs |
| Column collisions | Random prefixes (`l_xxxx_`/`r_xxxx_`) unless `alias()`/`lsuffix=`/`rsuffix=` used |
| Point-in-time | ASOF JOIN (SQL) matches rows by closest timestamp — used by Feature Store retrieval |
## Sampling Data
```python
df.sample(n=1000)          # target row count (approximate semantics)
df.sample(fraction=0.05)   # ~5% of rows
```
SQL: `SAMPLE (1000 ROWS)`, `SAMPLE BERNOULLI (5)`, `SAMPLE SYSTEM (5)`.
> [!warning]
> `BERNOULLI` samples each row independently; `SYSTEM` samples whole micro-partition blocks (faster, coarser). Both are probabilistic — size is approximate unless an explicit `ROWS` count is requested. Sampling without stratification can under-represent rare classes (e.g. 0.9% fraud rows); use stratified/filtered sampling for imbalanced targets. In Snowpark pandas, `sample(frac=0.1)` before `to_pandas()` reduces the volume pulled to memory.
## Actions vs Transformations
| Transformation (no execution) | Action (executes SQL) |
|---|---|
| `select`, `filter`, `with_column(s)` | `collect()` → list of `Row` |
| `join`, `group_by`, `agg` | `count()` |
| `sort`, `drop_duplicates` | `show()` (prints ≤10 rows) |
| `sample`, `cast` | `write.mode("overwrite").save_as_table("T")` |
- `df.queries` exposes the generated SQL before execution (useful for review/debugging).
- `schema` property does **not** trigger execution; `to_pandas()` materializes to the client (memory risk on huge data).
- Scaling trick from the demo: scale the warehouse up before `save_as_table`, scale down after — compute is the driver.
> [!warning]
> "DataFrame does nothing until…" questions always resolve to an **action**. `describe()` runs on a Snowpark DataFrame? No — call `df.describe()` then `.show()`/`.to_pandas()` to force it.
## Exam Patterns
| Scenario/Keyword | Answer |
|---|---|
| "When is SQL actually sent?" | Lazily — only on an action: `collect/show/count/save_as_table` |
| "Where does Snowpark code execute?" | Pushed down as SQL into the Snowflake warehouse |
| "Join a DataFrame with itself" | Create a copy first: `copy(df)` |
| "Remove repeated rows" | `drop_duplicates()` |
| "Replace NULL amounts with 0" | `fillna` / `NVL` / `ZEROIFNULL` |
| "Get ~5% subset of a huge table" | `df.sample(fraction=0.05)` / `SAMPLE` clause |
| "Bring results back to plot in matplotlib" | `to_pandas()` |
| "Ensure a day exists for every customer/date" | `session.range(...)` + cross join (demo pattern) |
| "Check where critical columns have missing data" | `COUNT(*) vs COUNT(col)` gap / `dropna` on key subset |
## Related Notes
- [exploratory-data-analysis](exploratory-data-analysis.md)
- [feature-engineering-techniques](feature-engineering-techniques.md)
- [feature-store-and-notebooks](feature-store-and-notebooks.md)
- [visualization-in-snowsight](visualization-in-snowsight.md)
## Source Documents
- [Working with DataFrames in Snowpark Python](../../sources/downloads/domain-2-data-preparation-and-feature-engineering/working-with-dataframes-in-snowpark-python-snowflake-documentation.md)
- [Snowpark Python Demo: Feature Engineering](../../sources/downloads/domain-2-data-preparation-and-feature-engineering/snowpark-python-demo-feature-engineering.md)
- [ASOF JOIN](../../sources/downloads/domain-2-data-preparation-and-feature-engineering/snowflake-feature-store-snowflake-documentation/asof-join-snowflake-documentation.md)