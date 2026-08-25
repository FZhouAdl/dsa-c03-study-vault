---
source_pdf: domain-2-data-preparation-and-feature-engineering/snowpark-python-demo-feature-engineering.md
part: "2.0"
keywords: feature engineering, scaling, encoding, binning, derived features, snowpark pandas
---
# Feature Engineering Techniques (★★★)
#domain-2 #feature-engineering
## Overview Table
| Item | Key Point |
|---|---|
| Objective 2.3 | Preprocessing (scaling/encoding/normalization), Data Transformations, Binarizing data, Snowpark Feature Store |
| Scaling | Standardization (z-score) vs Min-Max rescaling vs normalization (unit length) — pick by algorithm needs |
| Encoding | Label encoding (ordinal) vs one-hot encoding (unordered categories) |
| Derived features | Aggregations of raw data (average spend, frequency windows) computed with group_by/window |
| Binning | Continuous → intervals via `WIDTH_BUCKET`, `NTILE`, or `CASE`/`IFF` |
| Displayparity libraries | `snowflake.ml.modeling.preprocessing` (StandardScaler, OneHotEncoder) run distributed on the warehouse |
## Standardization / Normalization / Scaling
| Method | Formula | When | Snowflake ML / SQL |
|---|---|---|---|
| Standardization (z-score) | `(x − mean) / std` | Distance-based algos (SVM, K-means, L2-regularized LR); keeps outliers' relative spread | `snowflake.ml.modeling.preprocessing.StandardScaler` |
| Min-Max scaling | `(x − min) / (max − min)` → bounded [0,1] | Features with known bounded range (images, percentages) | `...MinMaxScaler` |
| Normalization | `x / ‖x‖` unit vector per row | Text/feature-vector similarity (cosine) | `...Normalizer` |
> [!warning]
> Terminology trap: "normalization" is overloaded. In ML preprocessing it can mean Min-Max rescaling or L2 vector norm; "standardization" (z-score) is definitely `(x−mean)/std`. Fit scalers on the training split only; apply the same fitted transform to validation/test to avoid leakage.
## Snowflake ML Preprocessors (distributed)
```python
from snowflake.ml.modeling.preprocessing import StandardScaler, OneHotEncoder
from snowflake.ml.modeling.pipeline import Pipeline

scaler  = StandardScaler(input_cols=['AGE','INCOME'], output_cols=['AGE_SCALED','INCOME_SCALED'])
encoder = OneHotEncoder(input_cols=['CITY'], output_cols=['CITY_ENCODED'])

pipe = Pipeline(steps=[('scaling', scaler), ('encoding', encoder)])
pipe.fit_transform(df).show()   # executes distributed in the warehouse
```
- OSS/scikit-learn preprocessors run locally/in-memory (prototyping); Snowflake ML preprocessors **push down** to the warehouse — use for large datasets.
- Snowflake ML preprocessors are a subset of scikit-learn's but cover the common cases.
- Unsupported high-cardinality/encoding needs: implement with Snowpark `IFF`/`CASE` columns or `COUNT_DISTINCT` gating.
## DataFrames: pandas vs Snowpark vs Snowpark pandas (Objective 2.3)
| Aspect | pandas | Snowpark DataFrame | pandas on Snowflake (`snowflake.snowpark.modin`) |
|---|---|---|---|
| Import | `import pandas as pd` | `session.table(...)` | `import modin.pandas as pd; import snowflake.snowpark.modin.plugin` |
| Compute | local CPU/memory | warehouse via SQL pushdown | **Snowflake engine** (transpiled to SQL + local hybrid), snowpark pandas API reference at `snowflake.snowpark.modin.pandas` |
| Eval model | eager | lazy (action triggers) | mimics eager; internally lazily-evaluated query graph |
| Scale | single machine, memory-bound | distributed | distributed for large, in-memory for small (hybrid execution) |
| Best for | small local data / prototyping | expressive relational pipelines | existing pandas code + pandas-savvy teams on big governed data |
| Conversion | — | `df.to_snowpark_pandas()` | `.to_snowpark()` → back to Snowpark DataFrame; `.to_pandas()` → materialize locally |
- Best practices: avoid `for` loops/`iterrows` (query complexity explodes); `to_pandas()` before third-party libs (matplotlib, scikit-learn) unless Snowpark ML used; calling `snapshot` pulls data — reduce first (`sample(frac=0.1)`, `head()`).
## Derived Features (e.g. average spend)
Demo pattern (fraud): per-customer 1/7/30-day transaction count + average amount built from windows, then joined back to transaction rows.
```python
from snowflake.snowpark import Window
w_1d = Window.partition_by(F.col("CUSTOMER_ID")).order_by(F.col("TX_DATE"))

feats = (df_cust_day
         .select(F.col("CUSTOMER_ID"), F.col("TX_DATE"),
                 F.lag(F.col("TX_AMOUNT"), 1).over(w_1d).as_("AMT_PREV"),
                 F.avg(F.col("TX_AMOUNT")).over(
                     w_1d.rowsBetween(-7, -1)).as_("AVG_AMT_7D"),
                 F.count(F.col("TX_ID")).over(
                     w_1d.rowsBetween(-30, -1)).as_("CNT_TX_30D")))
```
- Binary flags from timestamps with `IFF(dayofweek(...) IN (0,6), 1, 0)` (weekend/night features).
- Date grid + right-outer join + `ZEROIFNULL` ensures windows include "no transaction" days.
## Binning Continuous Data
| Method | SQL / code | Result |
|---|---|---|
| `WIDTH_BUCKET(expr, min, max, bins)` | `WIDTH_BUCKET(amount, 0, 1000, 10)` | index 1..10, equal-width |
| `NTILE(n)` window | `NTILE(4) OVER (ORDER BY amount)` | equal-count bins |
| `CASE` / `IFF` ranges | `CASE WHEN amount < 50 THEN 'low' ... END` | custom/labeled buckets (e.g. low/med/high) |
- Discretization removes assumptions of linearity; equal-width vs equal-frequency matters for skewed data.
## Label vs One-Hot Encoding
| Approach | Mapping | Use when | Snowpark |
|---|---|---|---|
| Label / ordinal | one integer per category (0,1,2…) | intrinsic order (low/med/high, day-of-week) | `IFF`/`CASE`, or `snowflake.ml.modeling.preprocessing.OrdinalEncoder` |
| One-hot | one binary column per category | unordered categories (city, product) | `OneHotEncoder`, or manual `F.iff(F.col("CITY")=="SF", 1, 0)` per value |
> [!warning]
> One-hot on a column with high cardinality explodes feature count → consider label encoding with a cap, feature hashing, or target mean encoding instead. Do NOT one-hot orderable columns — that discards ordering information; do NOT label-encode unordered columns — that invents a false order.
```python
SF   = F.iff(F.col("CITY") == "SF",  F.lit(1), F.lit(0))
NYC  = F.iff(F.col("CITY") == "NYC", F.lit(1), F.lit(0))   # manual one-hot via functions
df = df.with_columns(["IS_SF", "IS_NYC"], [SF, NYC])
```
## Imbalanced Data (supplementary)
> [!note]
> No downloaded vault source; general knowledge for the exam. SMOTE is not a Snowflake-native function — usually applied in a Python library after `to_pandas()`, or approximated with over/undersampling bookkeeping in SQL.
- **Oversampling**: SMOTE (synthesize minority samples via interpolation) — python packages `imbalanced-learn`; note Snowflake ML does not ship a SMOTE transformer.
- **Undersampling**: drop majority rows (random undersampling) to rebalance; risks losing information.
- Alternative: keep as-is and evaluate with precision/recall, class weights, or anomaly-style framing (e.g. fraud is ~0.9% of rows).
## Exam Patterns
| Scenario/Keyword | Answer |
|---|---|
| "Scale features to zero mean/unit variance" | `StandardScaler` (z-score = `(x−μ)/σ`) |
| "Scale features into a fixed range such as [0,1]" | Min-MaxScaler |
| "Convert unordered categories to model input" | one-hot encoding (or `OneHotEncoder`) |
| "Encode an ordinal column without expansion" | label/ordinal encoding |
| "Turn a continuous column into buckets" | `WIDTH_BUCKET` / `NTILE` / `CASE` |
| "Derive average customer spend over 30d" | window `AVG` over partition + frame |
| "Run existing pandas code on large Snowflake data" | pandas on Snowflake (`snowflake.snowpark.modin`, transpiles to SQL) |
| "Distributed preprocessing at scale" | `snowflake.ml.modeling.preprocessing` + Pipeline (pushdown to warehouse) |
## Related Notes
- [data-preparation-with-snowpark](data-preparation-with-snowpark.md)
- [exploratory-data-analysis](exploratory-data-analysis.md)
- [feature-store-and-notebooks](feature-store-and-notebooks.md)
## Source Documents
- [Snowpark Python Demo: Feature Engineering](../../sources/downloads/domain-2-data-preparation-and-feature-engineering/snowpark-python-demo-feature-engineering.md)
- [Engineer features (Snowflake ML)](../../sources/downloads/domain-2-data-preparation-and-feature-engineering/engineer-features-snowflake-documentation.md)
- [pandas on Snowflake](../../sources/downloads/domain-2-data-preparation-and-feature-engineering/pandas-on-snowflake-snowflake-documentation.md)
- [snowflake.ml.modeling reference](../../sources/downloads/domain-2-data-preparation-and-feature-engineering/engineer-features-snowflake-documentation/snowflake-ml-modeling-snowflake-documentation.md)