---
source_pdf: sources/SnowProDataScientistStudyGuide.pdf
part: "2.0"
keywords: practice, snowpark, eda, feature engineering, feature store
---
# Domain 2.0 Practice: Data Preparation & Feature Engineering (#practice #domain-2)
## Related Concepts
Before starting, review: [data-preparation-with-snowpark](data-preparation-with-snowpark.md), [exploratory-data-analysis](exploratory-data-analysis.md), [feature-engineering-techniques](feature-engineering-techniques.md), [feature-store-and-notebooks](feature-store-and-notebooks.md), [visualization-in-snowsight](visualization-in-snowsight.md).
> [!hint]- Key patterns (click to expand)
> | Keyword | Answer |
> | Approx number of times a value appears | APPROX_TOP_K |
> | Approx distinct values (cardinality) | APPROX_COUNT_DISTINCT |
> | Continuous → intervals | WIDTH_BUCKET / NTILE / CASE |
> | Slope / intercept / fit in SQL | REGR_SLOPE / REGR_INTERCEPT / REGR_R2 |
> | Missing values filling | fillna / na.drop / imputation |
> | Row-ordered stats over groups | Window functions with PARTITION/ORDER BY |
> | categorical, no order | one-hot |
> | categorical, ordinal | label encode |


## Recall Questions
### Q1
Which Snowflake estimating function identifies the **approximate number of times a value appears** (the most frequent values) in a dataset?

> [!hint]- Hint
> Cardinality vs frequency: which one lists "top" values?

> [!answer]- Show answer
> `APPROX_TOP_K(column, k)`. It estimates the most frequent (top-k) values using the Space-Saving algorithm. `APPROX_COUNT_DISTINCT` estimates the number of distinct values (cardinality); `APPROX_PERCENTILE` estimates quantiles. This question mirrors an official sample question.
### Q2
A Snowpark DataFrame `df = session.table("TXNS")` is defined. At what point is the SQL actually sent to Snowflake?

> [!hint]- Hint
> Lazy vs eager evaluation.

> [!answer]- Show answer
> Only when an **action** runs: `collect()`, `count()`, `show()`, or `write...save_as_table()`. Transformations like `select`, `filter`, `group_by` only build the query plan (lazy evaluation, pushed down to the warehouse).
### Q3
Which Snowpark Python method removes rows that are duplicates on a subset of columns?

> [!hint]- Hint
> Data cleaning, Objective 2.1.

> [!answer]- Show answer
> `df.drop_duplicates(["CUST_ID", "TXN_ID"])`. SQL equivalent: `SELECT DISTINCT` or the `ROW_NUMBER()` dedup pattern.
### Q4
In `REGR_SLOPE(y, x)` and `REGR_INTERCEPT(y, x)`, what does the documentation require about argument order, and which function verifies dependency between the variables?

> [!hint]- Hint
> Dependent vs independent; look at the R² function.

> [!answer]- Show answer
> **Dependent variable first (y), independent second (x)**. `REGR_SLOPE(sales, ad_spend)` is not the same as reversed order. `REGR_R2(y, x)` verifies the dependency (proportion of variance in y explained by x). `CORR(x, y)` is a related correlation check.
### Q5
Which algorithm powers `APPROX_COUNT_DISTINCT` (alias `HLL`)?

> [!hint]- Hint
> Two letters: H…

> [!answer]- Show answer
> HyperLogLog. It returns an approximation of `COUNT(DISTINCT col)`. Approximations are deterministic — the same input always gives the same result, but it may not exactly equal the exact count (doc example: 1007 vs 1024).
### Q6
Which window function splits ordered rows into buckets **of equal count**, and what is its SQL form?

> [!hint]- Hint
> Quartiles/deciles use it.

> [!answer]- Show answer
> `NTILE(n) OVER (PARTITION BY ... ORDER BY ...)` — e.g. `NTILE(4)` gives quartile buckets 1..4. Contrast with `WIDTH_BUCKET(expr, min, max, bins)` which makes equal-width intervals.
### Q7
You need an approximate **p95** latency value for millions of rows, computed quickly and deterministically. Which function?

> [!hint]- Hint
> Quantile estimation at scale.

> [!answer]- Show answer
> `APPROX_PERCENTILE(response_ms, 0.95)` (t-Digest). `APPROX_COUNT_DISTINCT` gives cardinality, `APPROX_TOP_K` gives most-frequent values — neither answers "what value is below p% of the data".
### Q8
Which Snowpark method replaces NULL values in a column, versus dropping rows?

> [!hint]- Hint
> Two cleaning verbs: fill or drop.

> [!answer]- Show answer
> `df.fillna(value={"AMOUNT": 0})` fills; `df.dropna(subset=["KEY"])` drops rows. SQL twins: `NVL`/`COALESCE`/`ZEROIFNULL` and `WHERE col IS NOT NULL`. Choose by business meaning — amounts/counts often `fillna(0)`, key columns that must exist get dropped.
## Application Questions
### Q9
A dataset contains a `PRODUCT_CATEGORY` column with 4 unordered categories and an ordinal `SATISFACTION` column (Low/Medium/High). You are about to train logistic regression. How should you encode each, and why?

> [!hint]- Hint
> Unordered → no false order; ordinal → keep the order without column explosion.

> [!answer]- Show answer
> - `PRODUCT_CATEGORY`: **one-hot encoding** (one binary column per category) — no intrinsic order, so labels would invent a false ordering.
> - `SATISFACTION`: **label/ordinal encoding** (0/1/2) — preserves order and avoids expansion.
> Use `snowflake.ml.modeling.preprocessing.OneHotEncoder` / `OrdinalEncoder`, or manual `F.iff(...)` columns in Snowpark. One-hot is wrong for high-cardinality columns (column explosion) and for ordinal features (loses ordering).
### Q10
You need a **per-transaction feature**: the average amount a customer spent in the previous 30 days (excluding the current transaction). Should you use a `GROUP BY` aggregation or a window function? Why?

> [!hint]- Hint
> Does each input row keep its identity?

> [!answer]- Show answer
> **Window function**: `AVG(TX_AMOUNT) OVER (PARTITION BY CUSTOMER_ID ORDER BY TX_DATETIME ROWS BETWEEN 30 PRECEDING AND 1 PRECEDING)` (or `rowsBetween(-30, -1)`). Only windows return one value **per input row** — `GROUP BY` collapses each customer to a single row and cannot attach a per-transaction value back. Same reason `LAG`/'previous day' features need windows.
### Q11 (Analysis)
Your team has thousands of lines of working pandas code that runs on a laptop over a 2 GB CSV. You must now run it over a 50 TB table governed in Snowflake with minimal rewriting. Which approach best fits, and what are the trade-offs?

> [!hint]- Hint
> Three DataFrame options: pandas, Snowpark, Snowpark pandas (modin).

> [!answer]- Show answer
> **pandas on Snowflake** (`import modin.pandas as pd` + `import snowflake.snowpark.modin.plugin`). The data stays under Snowflake governance; operations transpile to SQL and execute in the warehouse (hybrid execution — in-memory for small data, distributed/Snowflake engine for large). Trade-offs: requires an active Snowpark session; pandas type mapping is constrained by Snowflake's SQL types; not all pandas APIs have a distributed implementation (e.g. unsupported APIs raise `NotImplementedError`); avoid `for`/`iterrows` loops (query complexity); convert `to_pandas()` before third-party libraries unless using Snowpark ML. Pure Snowpark DataFrames give the most snowflake-native control but require rewriting code; native pandas would hit the memory ceiling and lose governance.
### Q12 (Analysis)
Two teams both build a "customer 30-day spend" feature — one for churn, one for fraud — computing it independently with slightly different definitions. Predictions for the same customer disagree, and retraining models produced features from the past that leak into training. What does the Snowflake Feature Store provide to fix this, and how does it guarantee feature timing?

> [!hint]- Hint
> One source of truth + no future leakage in training data.

> [!answer]- Show answer
> The **Feature Store** fixes consistency: features are defined **once** in a `FeatureView` (`register_feature_view`), reusable across models, refreshed automatically by Snowflake (managed, `refresh_freq`) or via external pipelines — a single source of truth with ML Lineage. Point-in-time correctness comes from building **datasets** from a **spine** (entity keys + timestamps); retrieval uses SQL **ASOF JOIN**, so each training row gets the feature value as of its own timestamp — never future values — eliminating leakage/train-serve skew. Models registered in the Model Registry automatically retrieve the correct feature values at inference.
## Summary Fold
> [!summary]- Quick recap of Domain 2.0
> - Clean with `drop_duplicates`, `fillna/dropna`, `cast`, `sample`; everything pushes to the warehouse and runs lazily until an action.
> - Profile with `describe()` + MIN/MAX/AVG/STDEV/VARIANCE, window functions (`LAG`, `NTILE`, frames), and approximations (`APPROX_TOP_K`=frequency, `APPROX_COUNT_DISTINCT`/HLL=cardinality, `APPROX_PERCENTILE`=quantile).
> - Regression in SQL: `REGR_SLOPE(y,x)`, `REGR_INTERCEPT(y,x)`, `REGR_R2(y,x)`.
> - Engineer: StandardScaler vs MinMax vs Normalizer; label vs one-hot; bin with `WIDTH_BUCKET`/`NTILE`/`CASE`; derived features via windows.
> - Centralize reusable features in the Feature Store (Entity/FeatureView/Dataset, ASOF JOIN for point-in-time); present findings with Snowsight charts + `to_pandas()` plots.
> - Exam traps: argument order in REGR_*; APPROX_TOP_K vs APPROX_COUNT_DISTINCT; window vs GROUP BY; lazy vs eager.
## Source Documents
- [SnowPro Data Scientist Study Guide (Domain 2.0)](../../sources/snowpro-data-scientist-study-guide.txt)