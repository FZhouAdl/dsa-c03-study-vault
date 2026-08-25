---
title: "snowflake-python-recipes/pandas on Snowflake 101/pandas on Snowflake 101.ipynb at main · Snowflake-Labs/snowflake-python-recipes · GitHub"
source: https://github.com/Snowflake-Labs/snowflake-python-recipes/blob/main/pandas%20on%20Snowflake%20101/pandas%20on%20Snowflake%20101.ipynb
cert_domain: domain-2-data-preparation-and-feature-engineering
crawl_depth: 1
crawled: 2026-08-23
note: fetched from raw.githubusercontent
---

# pandas on Snowflake 101

[pandas on Snowflake](https://docs.snowflake.com/developer-guide/snowpark/python/snowpark-pandas) gives Python developers the flexibility and convenience of pandas together with the power of Snowflake via a simple, unified, and familiar interface. Benefits of using pandas on Snowflake includes: 

- **Connected**: Easily work with Snowflake data, bring in data from files, and save back results
- **Robust**: Develop pandas pipeline at all data scales from prototype to production
- **Flexible**: Unlock powerful Snowflake analytics with familiar, flexible pandas API

In this quickstart, we'll show how you can get started with using pandas on Snowflake. We'll also see that the Snowpark pandas API is very similar to the native pandas API and enables you to scale up your traditional pandas pipelines with just a few lines of change. You can run this notebook in a Snowflake Notebook. 

## Import Required Packages

The Snowpark pandas API is available as part of the Snowpark Python package. Snowpark Python comes pre-installed with the Snowflake Notebooks environment. Additionally, you will need to add the `modin` package in the `Packages` dropdown.

- To install Modin, select `modin` from `Packages`.

```python
# Import the Snowpark pandas plugin for modin
import snowflake.snowpark.modin.plugin
import modin.pandas as pd
```

## Connecting to Snowflake 

To work with your data in Snowflake, you need to first get a session variable to connect to Snowflake. Since you are already logged in to Snowflake Notebook, you can get your session variable directly through the active notebook session. The session variable is the entrypoint that gives you access to using Python in Snowflake including pandas on Snowflake.

```python
# Access current Snowpark session
from snowflake.snowpark.context import get_active_session
session = get_active_session()
```

## Generate Data Tables
First let's generate synthetic data. Note that this will take about a minute but only needs to be run once. 

```python
CREATE OR REPLACE TABLE REVENUE_TRANSACTIONS_50M (Transaction_ID TEXT, Date DATE, Revenue FLOAT) AS
SELECT
  UUID_STRING() AS Transaction_ID,
  DATEADD(DAY,UNIFORM(0, 10000, RANDOM()),'1998-01-01') AS Date,
  UNIFORM(10, 1000, RANDOM()) * UNIFORM(10, 1000, RANDOM()) AS Revenue
FROM
  TABLE(GENERATOR(ROWCOUNT => 50000000));
```

## Reading Data From Snowflake


### 🐌 The Naive approach: Load data into in-memory pandas

There are two common approaches to reading the data to vanilla pandas. However, both of these can be inefficient on large datasets.

1) Create a [Snowpark DataFrame](https://docs.snowflake.com/en/developer-guide/snowpark/python/working-with-dataframes#return-the-contents-of-a-dataframe-as-a-pandas-dataframe) and calling [`to_pandas`](https://docs.snowflake.com/developer-guide/snowpark/reference/python/latest/snowpark/api/snowflake.snowpark.DataFrame.to_pandas) to export results into a pandas DataFrame
```python
snowpark_df = session.table("REVENUE_TRANSACTIONS_50M")
native_pd_df = snowpark_df.to_pandas()
```

2) Use the [Snowflake Connector for Python](https://docs.snowflake.com/en/developer-guide/python-connector/python-connector-pandas) to query and export results from Snowflake into a pandas DataFrame using [`fetch_pandas_all`](https://docs.snowflake.com/en/developer-guide/python-connector/python-connector-api#fetch_pandas_all)

```python
# Create a cursor object
cur = session.connection.cursor()
# Execute a statement that will generate a result set
cur.execute("select * from REVENUE_TRANSACTIONS_50M")
# Fetch all the rows in a cursor and load them into a pandas DataFrame
native_pd_df = cur.fetch_pandas_all()
```

We will use the first approach below to demonstrate the time it takes to pull data into pandas in-memory.

```python
from time import perf_counter
start_time = perf_counter()
table = session.table("REVENUE_TRANSACTIONS_50M")
pandas_df = table.to_pandas()
end_time = perf_counter()
time = end_time-start_time
print(f"Read to pandas dataframes takes {time} seconds")
```

### 🚀 The Better Approach: `pd.read_snowflake`


Now let's try this with pandas on Snowflake. We can read the table directly using Snowpark pandas's [`read_snowflake`](https://docs.snowflake.com/en/developer-guide/snowpark/reference/python/latest/modin/pandas_api/modin.pandas.read_snowflake) command, which reads in the table by creating a reference to the underlying table, rather than pulling all the data into memory. 

```python
df = pd.read_snowflake("REVENUE_TRANSACTIONS_50M")
```

```python
from time import perf_counter
start_time = perf_counter()
df = pd.read_snowflake("REVENUE_TRANSACTIONS_50M")
end_time = perf_counter()
time = end_time-start_time
print(f"Read to Snowpark pandas dataframe takes {time} seconds")
```

As you can see, calling `read_snowflake` on any sized data takes no more than a few seconds  This scales even as we increase the row size to billions of rows, while this would almost lead to out of memory errors with in-memory pandas.

## The Power of `read_snowflake`
`read_snowflake` doesn't only support reading in data from Snowflake tables, it also supports reading from Snowflake views, dynamic tables, iceberg tables, and more. Here you can see how read_snowflake can even take in a SQL query as an input and return a Snowpark pandas dataframe.

```python
summary_df = pd.read_snowflake("SELECT DATE_TRUNC ('MONTH', DATE) AS MONTH_DATE, SUM(REVENUE) AS TOTAL_REVENUE, COUNT(TRANSACTION_ID) AS TRANSACTION_COUNT FROM REVENUE_TRANSACTIONS_50M GROUP BY MONTH_DATE")
summary_df
```

You can even read from a view using `pd.read_snowflake`. Let's say that the SQL query we had earlier was used to define a view.

```python
CREATE OR REPLACE VIEW SUMMARY_VIEW AS SELECT DATE_TRUNC ('MONTH', DATE) AS MONTH_DATE, SUM(REVENUE) AS TOTAL_REVENUE, COUNT(TRANSACTION_ID) AS TRANSACTION_COUNT FROM REVENUE_TRANSACTIONS_50M GROUP BY MONTH_DATE;
```

```python
summary_df = pd.read_snowflake("SUMMARY_VIEW")
summary_df
```

In summary,`pd.read_snowflake` is a convenient way for you to work with your Snowflake objects and intermix Python and SQL queries.

## Examine and Profile Data
Let's take a look at the data we're going to be working with. We will inspect the dataframe by printing out the first few rows.

```python
df.head()
```

We can look at the size and overall descriptive statistics of our dataframe.

```python
df.shape
```

```python
df.describe()
```

## Data Transformations
Let's take a look at some common data transformations.

```python
df["DATE"] = pd.to_datetime(df["DATE"])
```

Filter to data only in the last 7 days based on the max date in the dataset.

```python
# Get the max date from the dataset
max_date = df["DATE"].max()
# Filter for last 7 days from the max date
filtered_df = df[(df["DATE"] >= max_date - pd.Timedelta('7 days')) & (df["DATE"] <= max_date)]
```

```python
print(f"Before filtering, dataset size: {len(df)} rows. After filtering, dataset size: {len(filtered_df)} rows")
```

The best part about this is that pandas on Snowflake automatically translates your pandas code into SQL and executed directly on Snowflake's engine, leading to significantly faster performance when working with large data. 

To show this in action, you can verify this by checking the `Query History` page to inspect the SQL query generated from your pandas operation.

pandas on Snowflake supports a wide range of operations—like data cleaning, transformation, reshaping, using the familiar pandas API. You can see the list of currently supported APIs in Snowpark pandas [here](https://docs.snowflake.com/en/developer-guide/snowpark/reference/python/latest/modin/supported/index).



## Saving back to Snowflake

Once you have developed your workflow, you can either save your results back to a table, view, files, dynamic table or iceberg table. We will show how you can save to a table and view in this demo. If you are interested in saving to a dynamic table to automatically refresh your pipeline as new data come in or saving it to a Iceberg table to leverage open table format, you can check out the example notebook [here](https://github.com/Snowflake-Labs/snowflake-python-recipes/blob/main/pandas%20pipeline%20with%20dynamic%20Iceberg%20tables/pandas%20pipeline%20with%20dynamic%20Iceberg%20tables.ipynb). 

You can use [to_snowflake](https://docs.snowflake.com/en/developer-guide/snowpark/reference/python/latest/modin/pandas_api/modin.pandas.DataFrame.to_snowflake) to save your Snowpark pandas dataframe back to Snowflake as a table.

```python
filtered_df.to_snowflake("FILTERED_REVENUE_TRANSACTIONS", if_exists="replace")
```

To verify that the table has been created, we can run a simple SQL query to inspect the table.

```python
SELECT * FROM FILTERED_REVENUE_TRANSACTIONS LIMIT 5;
```

You can also save your pandas workflow as a view. 

```python
filtered_df.to_view("FILTERED_REVENUE_VIEW", index = None)
```

Here you can see the view definition SQL statement that is generated by Snowpark pandas.

```python
SELECT GET_DDL('VIEW', 'FILTERED_REVENUE_VIEW');
```

This saves your Snowpark pandas operations as a pipeline that is then triggered when you access the view. 

```python
# Read the view into a pandas DataFrame
view_df = pd.read_snowflake("FILTERED_REVENUE_VIEW")
view_df.head()
```

## 🎁 Bonus: File read and write operations with pandas

You can use pandas on Snowflake to load in [CSV](https://docs.snowflake.com/en/developer-guide/snowpark/reference/python/latest/modin/pandas_api/modin.pandas.read_csv#modin.pandas.read_csv), [Parquet](https://docs.snowflake.com/en/developer-guide/snowpark/reference/python/latest/modin/pandas_api/modin.pandas.read_parquet#modin.pandas.read_parquet), and [Excel](https://docs.snowflake.com/en/developer-guide/snowpark/reference/python/latest/modin/pandas_api/modin.pandas.read_excel#modin.pandas.read_excel) from stage or local file location. Here is the full list of [I/O functionalities supported](https://docs.snowflake.com/en/developer-guide/snowpark/reference/python/latest/modin/io).

Here's how you can read CSV files from an S3 bucket



```python
-- First let's create a external stage and upload the CSV file. 
CREATE OR REPLACE STAGE FROSTBYTES
    URL = 's3://sfquickstarts/frostbyte_tastybytes/';
```

```python
menu_df = pd.read_csv("@frostbytes/analytics/menu_item_aggregate_v.csv")
menu_df
```

### Conclusion

In this quickstart, you saw how easy it is to get started with pandas on Snowflake. With minimal code changes, your existing pandas workflows can scale to larger datasets and run directly in Snowflake’s engine. 
pandas on Snowflake brings the flexibility and familiarity of the pandas API to the power and scale of Snowflake. It provides a simple, unified experience for Python developers to work efficiently with large datasets—all without moving data out of Snowflake.

Key benefits include:

- Connected – Easily access Snowflake data, bring in files, and write results back
- Robust – Build pipelines that scale seamlessly from development to production
- Flexible – Use the familiar pandas API to unlock powerful Snowflake analytics

To learn more, see [Snowflake Documentation](https://docs.snowflake.com/developer-guide/snowpark/python/snowpark-pandas). For a more advanced example, check out [this quickstart](https://quickstarts.snowflake.com/guide/data_engineering_pipelines_with_snowpark_pandas/) on how you can build a data engineering pipeline with Snowpark pandas.

