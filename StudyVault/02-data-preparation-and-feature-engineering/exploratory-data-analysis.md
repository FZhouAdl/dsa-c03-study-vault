---
source_pdf: domain-2-data-preparation-and-feature-engineering/approx-count-distinct-snowflake-documentation.md
part: "2.0"
keywords: eda, descriptive statistics, window functions, approximation, linear regression, data profiling
---
# Exploratory Data Analysis (★★★)
#domain-2 #eda
## Overview Table
| Item | Key Point |
|---|---|
| Data profiling | Identify initial patterns: distributions, nulls, cardinality, outliers (Objective 2.2) |
| Descriptive stats | `MIN/MAX/AVG/STDEV/VARIANCE`, `MEDIAN`, `MODE`, `COUNT_DISTINCT`; Snowpark `df.describe()` |
| Window functions | Per-row analytics over a partition without collapsing rows; `NTILE` for equal buckets |
| Approximation | Fast, deterministic estimates on big data: `APPROX_COUNT_DISTINCT`, `APPROX_TOP_K`, `APPROX_PERCENTILE` |
| Linear regression (SQL) | `REGR_SLOPE`, `REGR_INTERCEPT`, `REGR_R2` — slope, intercept, dependency |
| External tooling | Jupyter/IDE connect via Snowpark client; notebooks run Python inside Snowflake |
## Data Profiling
- Initial exploration: null counts per column (`COUNT(*) vs COUNT(col)`), distinct counts, min/max ranges, distribution shape, target class balance (e.g. `RATIO_TO_REPORT` for fraud %).
- Snowpark: `df.describe().show()` → count/mean/stddev/min/max per numeric column. For the full curses: pull to pandas and use `pandas_profiling`/`ydata-profiling`.
- Frequency check by group: `group_by().agg(count)` then `F.call_function("RATIO_TO_REPORT", col).over()` for percentages (demo pattern).
## Descriptive Statistics
```sql
SELECT MIN(amount), MAX(amount), AVG(amount),
       STDDEV(amount), VARIANCE(amount), MEDIAN(amount),
       CORR(spend, income)
FROM customer_metrics;
```
| Function | Returns |
|---|---|
| `MIN` / `MAX` / `AVG` | Range and center of a column |
| `STDDEV` / `VARIANCE` | Spread; sample variants by default (`_POP` variants exist) |
| `MEDIAN` / `MODE` | Robust center / most frequent value |
| `COUNT(col)` vs `COUNT(*)` | Non-null count vs total rows — null-gap diagnostic |
| `CORR(x,y)` | Pearson correlation between two columns |
## Window Functions
Window = `OVER (PARTITION BY ... ORDER BY ...)`; rows keep their identity; each input row gets a result.
| Pattern | Function |
|---|---|
| Share of group total | `RATIO_TO_REPORT(count) OVER (PARTITION BY grp)` |
| Previous/next value | `LAG(x, 1)` / `LEAD(x, 1) OVER (PARTITION BY c ORDER BY d)` |
| Rolling/cumulative totals | `SUM(x) OVER (... ROWS BETWEEN 7 PRECEDING AND CURRENT ROW)` |
| Rank within a group | `ROW_NUMBER()`, `RANK()`, `DENSE_RANK()` |
| Equal buckets by order | `NTILE(4) OVER (ORDER BY shares)` → buckets 1..4 of equal size |
```python
from snowflake.snowpark import Window
w7 = Window.partition_by("CUSTOMER_ID").order_by("TX_DATE").rowsBetween(-7, -1)
df = df.with_column("AVG_AMT_PREV_7D", F.avg("TX_AMOUNT").over(w7))
```
> [!warning]
> GROUP BY collapses each group to one row; a window function returns one value **per input row**. Scenarios like "per-row running average" or "previous day's value" require windows (`AVG/LAG OVER`), not GROUP BY. Approach functions used as windows do **not** support ORDER BY inside `OVER` or explicit frames.
## Approximation / High-Performance Functions
| Function | Algorithm | Estimates… |
|---|---|---|
| `APPROX_COUNT_DISTINCT(x)` (alias `HLL`) | HyperLogLog | **Number of distinct values** (cardinality) |
| `APPROX_TOP_K(x, k)` | Space-Saving | **Which values appear most often** (frequency/TOPn) |
| `APPROX_PERCENTILE(x, p)` | t-Digest | Value below which p% of data falls (e.g. p95) |
```sql
SELECT APPROX_COUNT_DISTINCT(customer_id),   -- ~unique customers
       APPROX_TOP_K(page_url, 10),           -- ~top 10 most visited pages
       APPROX_PERCENTILE(response_ms, 0.95); -- ~p95 latency
FROM events;
```
- Exam gold rule: "approximate **number of times a value appears** / most frequent values" = **APPROX_TOP_K**; "approximate **number of distinct values**" = **APPROX_COUNT_DISTINCT** / **HLL**.
- Approximations are **deterministic**: same input ⇒ same output; but `APPROX_COUNT_DISTINCT(i)` may not equal `COUNT(DISTINCT i)` (doc example: 1007 vs 1024). Exact alternatives (`COUNT(DISTINCT)`, `PERCENTILE_CONT`, `MODE`) are slower.
## Linear Regression in SQL (Objective 2.2)
| Goal | Function |
|---|---|
| Slope | `REGR_SLOPE(y, x)` = `COVAR_POP(x,y) / VAR_POP(x)` |
| Intercept | `REGR_INTERCEPT(y, x)` = `AVG(y) − SLOPE × AVG(x)` |
| Dependency strength | `REGR_R2(y, x)` → R², proportion of y's variance explained by x |
| Diagnostics | `REGR_COUNT`, `REGR_AVGX/Y`, `REGR_SXX/SXY/SYY`, `CORR(x,y)` |
```sql
SELECT REGR_SLOPE(sales, ad_spend)   AS slope,
       REGR_INTERCEPT(sales, ad_spend) AS intercept,
       REGR_R2(sales, ad_spend)      AS r_squared
FROM monthly_metrics;
```
> [!warning]
> Argument order: **dependent variable first (y), independent second (x)**. `REGR_SLOPE(sales, ad_spend)` ≠ `REGR_SLOPE(ad_spend, sales)`.
## Connecting External ML Platforms / Notebooks
- Jupyter / any IDE: install `snowflake-snowpark-python`, build a `Session`, work via pushdown; local Python never sees raw data.
- Snowsight: Snowflake Notebooks and Python worksheets run Python (Snowpark) inside Snowflake on the warehouse.
- External tools use standard drivers/connectors; governance/security stay in Snowflake (data never leaves the account).
- `snowflake.ml` modeling and preprocessors run distributed on the warehouse — see feature engineering note.
## Exam Patterns
| Scenario/Keyword | Answer |
|---|---|
| "Approximate number of times a value appears / most frequent values" | `APPROX_TOP_K` |
| "Approximate number of distinct values" | `APPROX_COUNT_DISTINCT` / `HLL` (HyperLogLog) |
| "Approximate p95 / median at scale" | `APPROX_PERCENTILE` |
| "Per-row previous value / rolling sum" | `LAG` / window frame with `OVER` |
| "Split ordered rows into N equal buckets" | `NTILE(N) OVER (ORDER BY ...)` |
| "Slope and intercept from SQL" | `REGR_SLOPE(y,x)` + `REGR_INTERCEPT(y,x)` |
| "Verify dependency between dependent & independent variables" | `REGR_R2(y,x)` (or `CORR`) |
| "Quick profile of numeric columns" | `df.describe()` / MIN-MAX-AVG-STDDEV set |
## Related Notes
- [data-preparation-with-snowpark](data-preparation-with-snowpark.md)
- [visualization-in-snowsight](visualization-in-snowsight.md)
- [feature-engineering-techniques](feature-engineering-techniques.md)
## Source Documents
- [APPROX_COUNT_DISTINCT](../../sources/downloads/domain-2-data-preparation-and-feature-engineering/approx-count-distinct-snowflake-documentation.md)
- [NTILE](../../sources/downloads/domain-2-data-preparation-and-feature-engineering/ntile-snowflake-documentation.md)
- [Snowpark API: REGR_SLOPE / REGR_INTERCEPT / APPROX_* / WIDTH_BUCKET](../../sources/downloads/domain-2-data-preparation-and-feature-engineering/functions-snowflake-documentation.md)
- [Snowpark Python Demo: Feature Engineering](../../sources/downloads/domain-2-data-preparation-and-feature-engineering/snowpark-python-demo-feature-engineering.md)
- [Snowsight: Visualizing worksheet data](../../sources/downloads/domain-2-data-preparation-and-feature-engineering/visualizing-worksheet-data-snowflake-documentation.md)