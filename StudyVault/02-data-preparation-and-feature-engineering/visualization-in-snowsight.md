---
source_pdf: domain-2-data-preparation-and-feature-engineering/visualizing-worksheet-data-snowflake-documentation.md
part: "2.0"
keywords: snowsight, visualization, outliers, statistical summaries, charting
---
# Visualization in Snowsight (★★◎)
#domain-2 #visualization
## Overview Table
| Item | Key Point |
|---|---|
| Objective 2.4 | Statistical summaries, interpret charts, identify outliers, present a business case |
| Snowsight charting | Charts on worksheet query results: bar, line, scatterplot, heat grid, scorecard |
| Chart data features | Column selection/bucketing + aggregations (average, count, min, max, median, mode, sum) |
| Open-source graph libs | matplotlib, plotly, altair, seaborn — run on data pulled via `to_pandas()` |
| Outlier detection | SQL stats (STDDEV/percentiles/NTILE/IQR), scatter/heat visualization; confirm before dropping |
| Snowflake Notebooks | Streamlit, altair, matplotlib, seaborn inline for interactive EDA |
## Snowsight Charts from SQL (statistical summaries)
| Chart type | Best for | Example query output |
|---|---|---|
| Bar | counts by category (TOPn) | fraud counts by weekday |
| Line | trends over time | transactions per day |
| Scatterplot | two-variable relationships / outliers | amount vs time, spend vs income |
| Heat grid | matrix density / correlation | hour × weekday activity |
| Scorecard | single KPI with delta | total revenue, fraud rate |
- Dropdown menu path: run worksheet → **Chart** tab above results. Column inspector controls:
  - **Data**: add/remove/re-label columns; change what is plotted.
  - **Bucket**: group date/timestamp by **day/week/month/year**; group numeric columns by integer buckets.
  - **Aggregate**: average, count, minimum, maximum, median, mode, sum (choose per column).
- Chart updates automatically while the columns it uses stay in the query result; charts cost compute (warehouse) — mind cost for big queries.
```sql
SELECT COUNT(O_ORDERDATE) AS orders, :datebucket(O_ORDERDATE), O_ORDERDATE AS date
FROM SNOWFLAKE_SAMPLE_DATA.TPCH_SF1.ORDERS
GROUP BY :datebucket(O_ORDERDATE), O_ORDERDATE
ORDER BY O_ORDERDATE LIMIT 10;
```
## Identifying Data Outliers
| Technique | How |
|---|---|
| z-score | `ABS((x - AVG(x)) / STDDEV(x)) > 3` — flag extreme points |
| IQR rule | `Q1 − 1.5*IQR` / `Q3 + 1.5*IQR` via `PERCENTILE_CONT` |
| Decile/percentile view | `NTILE(10) OVER (ORDER BY x)` to inspect the tail buckets |
| Statistical summary | `MIN/MAX/AVG/STDDEV`, `APPROX_PERCENTILE(x, 0.99)` (see EDA note) |
| Scatterplot / box-plot | Snowsight scatter or matplotlib/pandas box plot on `to_pandas()` |
```sql
WITH stats AS (
  SELECT AVG(amount) m, STDDEV(amount) sd FROM payments WHERE status = 'VALID'
)
SELECT * FROM payments p, stats
WHERE p.status='VALID' AND ABS((p.amount - stats.m)/stats.sd) > 3;
```
> [!warning]
> Outliers are not always errors — investigate a subset before removing; in fraud/threat data the outliers are the signal. Don't silently drop them from the model dataset.
## Interpreting Open-Source Graph Libraries
- Snowpark DataFrames → `to_pandas()` → matplotlib/plotly/altair/seaborn; data stays small after aggregation, so pulls are cheap.
- What to look for: distribution shape (skew), class imbalance, correlation direction/strength, spikes/spikes and gaps, tail behavior for fraud/threshold features.
```python
import matplotlib.pyplot as plt
daily = df.group_by(F.to_date(F.col("TX_DATETIME"))).count().to_pandas()
daily.plot(x="TO_DATE(TX_DATETIME)", y="COUNT")
plt.show()
```
- Notebooks (Objective 2.4): run Streamlit, altair, matplotlib, seaborn inline in Snowflake Notebooks; pair SQL cells (source of truth) with Python visualization cells.
## Statistical Summaries to Include in a Business Case
| Summary | Message it supports |
|---|---|
| Row counts, time range, class balance (%) | Data coverage statement |
| MIN/MEDIAN/MAX, STDEV on KPIs | Spread + representative value (median for skewed) |
| Trend + seasonality (weekday/night flags) | Behavioral narrative |
| Correlation / regression R² | Explainable driver of the business metric |
- Cite the model metrics back to the feature that matters (e.g. "transactions at night correlate with fraud rate") so the story maps to features you built.
## Exam Patterns
| Scenario/Keyword | Answer |
|---|---|
| "Chart query results with minimal effort" | Snowsight **Chart** tab on worksheet results |
| "Group daily numbers into monthly buckets without rewriting SQL" | Chart bucketing by month |
| "Single-value KPI showing delta vs prior period" | Scorecard chart |
| "Spot relationship + outliers between two numeric columns" | Scatterplot (Snowsight or plotly) |
| "Plot with matplotlib in a notebook" | `to_pandas()` first, then plot |
| "Objectively flag extreme data points in SQL" | z-score / IQR / `NTILE` tail inspection |
## Related Notes
- [exploratory-data-analysis](exploratory-data-analysis.md)
- [data-preparation-with-snowpark](data-preparation-with-snowpark.md)
- [feature-store-and-notebooks](feature-store-and-notebooks.md)
## Source Documents
- [Visualizing worksheet data](../../sources/downloads/domain-2-data-preparation-and-feature-engineering/visualizing-worksheet-data-snowflake-documentation.md)
- [About Snowflake Notebooks](../../sources/downloads/domain-2-data-preparation-and-feature-engineering/about-legacy-snowflake-notebooks-snowflake-documentation.md)
- [Snowpark Python Demo: Feature Engineering](../../sources/downloads/domain-2-data-preparation-and-feature-engineering/snowpark-python-demo-feature-engineering.md)
- [Snowflake Notebooks in Workspaces](../../sources/downloads/domain-2-data-preparation-and-feature-engineering/about-legacy-snowflake-notebooks-snowflake-documentation/snowflake-notebooks-in-workspaces-snowflake-documentation.md)