---
title: "snowflake.snowpark.DataFrame | Snowflake Documentation"
source: https://docs.snowflake.com/en/developer-guide/snowpark/reference/python/latest/snowpark/api/snowflake.snowpark.DataFrame.html
cert_domain: domain-2-data-preparation-and-feature-engineering
crawl_depth: 1
crawled: 2026-08-23
---

# snowflake.snowpark.DataFrame¶

_class _snowflake.snowpark.DataFrame(_session : Optional[[Session](snowflake.snowpark.Session.html#snowflake.snowpark.Session "snowflake.snowpark.session.Session")] = None_, _plan : Optional[LogicalPlan] = None_, _is_cached : bool = False_)[[source]](https://github.com/snowflakedb/snowpark-python/blob/v1.54.0/src/snowflake/snowpark/dataframe.py#L419-L7107)¶
    

Bases: `object`

Represents a lazily-evaluated relational dataset that contains a collection of [`Row`](snowflake.snowpark.Row.html#snowflake.snowpark.Row "snowflake.snowpark.Row") objects with columns defined by a schema (column name and type).

A DataFrame is considered lazy because it encapsulates the computation or query required to produce a relational dataset. The computation is not performed until you call a method that performs an action (e.g. [`collect()`](snowflake.snowpark.DataFrame.collect.html#snowflake.snowpark.DataFrame.collect "snowflake.snowpark.DataFrame.collect")).

**Creating a DataFrame**

You can create a DataFrame in a number of different ways, as shown in the examples below.

Creating tables and data to run the sample code:
    
[code]
    >>> session.sql("create or replace temp table prices(product_id varchar, amount number(10, 2))").collect()
    [Row(status='Table PRICES successfully created.')]
    >>> session.sql("insert into prices values ('id1', 10.0), ('id2', 20.0)").collect()
    [Row(number of rows inserted=2)]
    >>> # Create a CSV file to demo load
    >>> import tempfile
    >>> with tempfile.NamedTemporaryFile(mode="w+t") as t:
    ...     t.writelines(["id1, Product A", "\n" "id2, Product B"])
    ...     t.flush()
    ...     create_stage_result = session.sql("create or replace temp stage test_stage").collect()
    ...     put_result = session.file.put(t.name, "@test_stage/test_dir")
    
[/code]

Example 1
    

Creating a DataFrame by reading a table in Snowflake:
[code] 
    >>> df_prices = session.table("prices")
    
[/code]

Example 2
    

Creating a DataFrame by reading files from a stage:
[code] 
    >>> from snowflake.snowpark.types import StructType, StructField, IntegerType, StringType
    >>> df_catalog = session.read.schema(StructType([StructField("id", StringType()), StructField("name", StringType())])).csv("@test_stage/test_dir")
    >>> df_catalog.show()
    ---------------------
    |"ID"  |"NAME"      |
    ---------------------
    |id1   | Product A  |
    |id2   | Product B  |
    ---------------------
    
[/code]

Example 3
    

Creating a DataFrame by specifying a sequence or a range:
[code] 
    >>> session.create_dataframe([(1, "one"), (2, "two")], schema=["col_a", "col_b"]).show()
    ---------------------
    |"COL_A"  |"COL_B"  |
    ---------------------
    |1        |one      |
    |2        |two      |
    ---------------------
    
    >>> session.range(1, 10, 2).to_df("col1").sort("col1").show()
    ----------
    |"COL1"  |
    ----------
    |1       |
    |3       |
    |5       |
    |7       |
    |9       |
    ----------
    
[/code]

Example 4
    

Create a new DataFrame by applying transformations to other existing DataFrames:
[code] 
    >>> df_merged_data = df_catalog.join(df_prices, df_catalog["id"] == df_prices["product_id"])
    
[/code]

**Performing operations on a DataFrame**

Broadly, the operations on DataFrame can be divided into two types:

  * **Transformations** produce a new DataFrame from one or more existing DataFrames. Note that transformations are lazy and don’t cause the DataFrame to be evaluated. If the API does not provide a method to express the SQL that you want to use, you can use `functions.sqlExpr()` as a workaround.

  * **Actions** cause the DataFrame to be evaluated. When you call a method that performs an action, Snowpark sends the SQL query for the DataFrame to the server for evaluation.




**Transforming a DataFrame**

The following examples demonstrate how you can transform a DataFrame.

Example 5
    

Using the [`select()`](snowflake.snowpark.DataFrame.select.html#snowflake.snowpark.DataFrame.select "snowflake.snowpark.DataFrame.select") method to select the columns that should be in the DataFrame (similar to adding a `SELECT` clause):
[code] 
    >>> # Return a new DataFrame containing the product_id and amount columns of the prices table.
    >>> # This is equivalent to: SELECT PRODUCT_ID, AMOUNT FROM PRICES;
    >>> df_price_ids_and_amounts = df_prices.select(col("product_id"), col("amount"))
    
[/code]

Example 6
    

Using the [`Column.as_()`](snowflake.snowpark.Column.as_.html#snowflake.snowpark.Column.as_ "snowflake.snowpark.Column.as_") method to rename a column in a DataFrame (similar to using `SELECT col AS alias`):
[code] 
    >>> # Return a new DataFrame containing the product_id column of the prices table as a column named
    >>> # item_id. This is equivalent to: SELECT PRODUCT_ID AS ITEM_ID FROM PRICES;
    >>> df_price_item_ids = df_prices.select(col("product_id").as_("item_id"))
    
[/code]

Example 7
    

Using the [`filter()`](snowflake.snowpark.DataFrame.filter.html#snowflake.snowpark.DataFrame.filter "snowflake.snowpark.DataFrame.filter") method to filter data (similar to adding a `WHERE` clause):
[code] 
    >>> # Return a new DataFrame containing the row from the prices table with the ID 1.
    >>> # This is equivalent to:
    >>> # SELECT * FROM PRICES WHERE PRODUCT_ID = 1;
    >>> df_price1 = df_prices.filter((col("product_id") == 1))
    
[/code]

Example 8
    

Using the [`sort()`](snowflake.snowpark.DataFrame.sort.html#snowflake.snowpark.DataFrame.sort "snowflake.snowpark.DataFrame.sort") method to specify the sort order of the data (similar to adding an `ORDER BY` clause):
[code] 
    >>> # Return a new DataFrame for the prices table with the rows sorted by product_id.
    >>> # This is equivalent to: SELECT * FROM PRICES ORDER BY PRODUCT_ID;
    >>> df_sorted_prices = df_prices.sort(col("product_id"))
    
[/code]

Example 9
    

Using [`agg()`](snowflake.snowpark.DataFrame.agg.html#snowflake.snowpark.DataFrame.agg "snowflake.snowpark.DataFrame.agg") method to aggregate results.
[code] 
    >>> import snowflake.snowpark.functions as f
    >>> df_prices.agg(("amount", "sum")).collect()
    [Row(SUM(AMOUNT)=Decimal('30.00'))]
    >>> df_prices.agg(f.sum("amount")).collect()
    [Row(SUM(AMOUNT)=Decimal('30.00'))]
    >>> # rename the aggregation column name
    >>> df_prices.agg(f.sum("amount").alias("total_amount"), f.max("amount").alias("max_amount")).collect()
    [Row(TOTAL_AMOUNT=Decimal('30.00'), MAX_AMOUNT=Decimal('20.00'))]
    
[/code]

Example 10
    

Using the [`group_by()`](snowflake.snowpark.DataFrame.group_by.html#snowflake.snowpark.DataFrame.group_by "snowflake.snowpark.DataFrame.group_by") method to return a [`RelationalGroupedDataFrame`](snowflake.snowpark.RelationalGroupedDataFrame.html#snowflake.snowpark.RelationalGroupedDataFrame "snowflake.snowpark.RelationalGroupedDataFrame") that you can use to group and aggregate results (similar to adding a `GROUP BY` clause).

[`RelationalGroupedDataFrame`](snowflake.snowpark.RelationalGroupedDataFrame.html#snowflake.snowpark.RelationalGroupedDataFrame "snowflake.snowpark.RelationalGroupedDataFrame") provides methods for aggregating results, including:

  * [`RelationalGroupedDataFrame.avg()`](snowflake.snowpark.RelationalGroupedDataFrame.avg.html#snowflake.snowpark.RelationalGroupedDataFrame.avg "snowflake.snowpark.RelationalGroupedDataFrame.avg") (equivalent to AVG(column))

  * [`RelationalGroupedDataFrame.count()`](snowflake.snowpark.RelationalGroupedDataFrame.count.html#snowflake.snowpark.RelationalGroupedDataFrame.count "snowflake.snowpark.RelationalGroupedDataFrame.count") (equivalent to COUNT())

  * [`RelationalGroupedDataFrame.max()`](snowflake.snowpark.RelationalGroupedDataFrame.max.html#snowflake.snowpark.RelationalGroupedDataFrame.max "snowflake.snowpark.RelationalGroupedDataFrame.max") (equivalent to MAX(column))

  * [`RelationalGroupedDataFrame.median()`](snowflake.snowpark.RelationalGroupedDataFrame.median.html#snowflake.snowpark.RelationalGroupedDataFrame.median "snowflake.snowpark.RelationalGroupedDataFrame.median") (equivalent to MEDIAN(column))

  * [`RelationalGroupedDataFrame.min()`](snowflake.snowpark.RelationalGroupedDataFrame.min.html#snowflake.snowpark.RelationalGroupedDataFrame.min "snowflake.snowpark.RelationalGroupedDataFrame.min") (equivalent to MIN(column))

  * [`RelationalGroupedDataFrame.sum()`](snowflake.snowpark.RelationalGroupedDataFrame.sum.html#snowflake.snowpark.RelationalGroupedDataFrame.sum "snowflake.snowpark.RelationalGroupedDataFrame.sum") (equivalent to SUM(column))



[code] 
    >>> # Return a new DataFrame for the prices table that computes the sum of the prices by
    >>> # category. This is equivalent to:
    >>> #  SELECT CATEGORY, SUM(AMOUNT) FROM PRICES GROUP BY CATEGORY
    >>> df_total_price_per_category = df_prices.group_by(col("product_id")).sum(col("amount"))
    >>> # Have multiple aggregation values with the group by
    >>> import snowflake.snowpark.functions as f
    >>> df_summary = df_prices.group_by(col("product_id")).agg(f.sum(col("amount")).alias("total_amount"), f.avg("amount")).sort(col("product_id"))
    >>> df_summary.show()
    -------------------------------------------------
    |"PRODUCT_ID"  |"TOTAL_AMOUNT"  |"AVG(AMOUNT)"  |
    -------------------------------------------------
    |id1           |10.00           |10.00000000    |
    |id2           |20.00           |20.00000000    |
    -------------------------------------------------
    
[/code]

Example 11
    

Using windowing functions. Refer to [`Window`](snowflake.snowpark.Window.html#snowflake.snowpark.Window "snowflake.snowpark.Window") for more details.
[code] 
    >>> from snowflake.snowpark import Window
    >>> from snowflake.snowpark.functions import row_number
    >>> df_prices.with_column("price_rank",  row_number().over(Window.order_by(col("amount").desc()))).show()
    ------------------------------------------
    |"PRODUCT_ID"  |"AMOUNT"  |"PRICE_RANK"  |
    ------------------------------------------
    |id2           |20.00     |1             |
    |id1           |10.00     |2             |
    ------------------------------------------
    
[/code]

Example 12
    

Handling missing values. Refer to [`DataFrameNaFunctions`](snowflake.snowpark.DataFrameNaFunctions.html#snowflake.snowpark.DataFrameNaFunctions "snowflake.snowpark.DataFrameNaFunctions") for more details.
[code] 
    >>> df = session.create_dataframe([[1, None, 3], [4, 5, None]], schema=["a", "b", "c"])
    >>> df.na.fill({"b": 2, "c": 6}).show()
    -------------------
    |"A"  |"B"  |"C"  |
    -------------------
    |1    |2    |3    |
    |4    |5    |6    |
    -------------------
    
[/code]

**Performing an action on a DataFrame**

The following examples demonstrate how you can perform an action on a DataFrame.

Example 13
    

Performing a query and returning an array of Rows:
[code] 
    >>> df_prices.collect()
    [Row(PRODUCT_ID='id1', AMOUNT=Decimal('10.00')), Row(PRODUCT_ID='id2', AMOUNT=Decimal('20.00'))]
    
[/code]

Example 14
    

Performing a query and print the results:
[code] 
    >>> df_prices.show()
    ---------------------------
    |"PRODUCT_ID"  |"AMOUNT"  |
    ---------------------------
    |id1           |10.00     |
    |id2           |20.00     |
    ---------------------------
    
[/code]

Example 15
    

Calculating statistics values. Refer to [`DataFrameStatFunctions`](snowflake.snowpark.DataFrameStatFunctions.html#snowflake.snowpark.DataFrameStatFunctions "snowflake.snowpark.DataFrameStatFunctions") for more details.
[code] 
    >>> df = session.create_dataframe([[1, 2], [3, 4], [5, -1]], schema=["a", "b"])
    >>> df.stat.corr("a", "b")
    -0.5960395606792697
    
[/code]

Example 16
    

Performing a query asynchronously and returning a list of [`Row`](snowflake.snowpark.Row.html#snowflake.snowpark.Row "snowflake.snowpark.Row") objects:
[code] 
    >>> df = session.create_dataframe([[float(4), 3, 5], [2.0, -4, 7], [3.0, 5, 6], [4.0, 6, 8]], schema=["a", "b", "c"])
    >>> async_job = df.collect_nowait()
    >>> async_job.result()
    [Row(A=4.0, B=3, C=5), Row(A=2.0, B=-4, C=7), Row(A=3.0, B=5, C=6), Row(A=4.0, B=6, C=8)]
    
[/code]

Example 17
    

Performing a query and transforming it into `pandas.DataFrame` asynchronously:
[code] 
    >>> async_job = df.to_pandas(block=False)
    >>> async_job.result()
         A  B  C
    0  4.0  3  5
    1  2.0 -4  7
    2  3.0  5  6
    3  4.0  6  8
    
[/code]

Methods

[`agg`](snowflake.snowpark.DataFrame.agg.html#snowflake.snowpark.DataFrame.agg "snowflake.snowpark.DataFrame.agg")(*exprs) | Aggregate the data in the DataFrame.  
---|---  
`alias`(name) | Returns an aliased dataframe in which the columns can now be referenced to using col(<df alias>, <column name>).  
[`approxQuantile`](snowflake.snowpark.DataFrame.approxQuantile.html#snowflake.snowpark.DataFrame.approxQuantile "snowflake.snowpark.DataFrame.approxQuantile")(col, percentile, *[, ...]) | For a specified numeric column and a list of desired quantiles, returns an approximate value for the column at each of the desired quantiles.  
[`approx_quantile`](snowflake.snowpark.DataFrame.approx_quantile.html#snowflake.snowpark.DataFrame.approx_quantile "snowflake.snowpark.DataFrame.approx_quantile")(col, percentile, *[, ...]) | For a specified numeric column and a list of desired quantiles, returns an approximate value for the column at each of the desired quantiles.  
[`cache_result`](snowflake.snowpark.DataFrame.cache_result.html#snowflake.snowpark.DataFrame.cache_result "snowflake.snowpark.DataFrame.cache_result")(*[, statement_params]) | Caches the content of this DataFrame to create a new cached Table DataFrame.  
[`col`](snowflake.snowpark.DataFrame.col.html#snowflake.snowpark.DataFrame.col "snowflake.snowpark.DataFrame.col")(col_name) | Returns a reference to a column in the DataFrame.  
[`col_ilike`](snowflake.snowpark.DataFrame.col_ilike.html#snowflake.snowpark.DataFrame.col_ilike "snowflake.snowpark.DataFrame.col_ilike")(pattern) | Returns a new DataFrame with only the columns whose names match the specified pattern using case-insensitive ILIKE matching (similar to SELECT * ILIKE 'pattern' in SQL).  
[`collect`](snowflake.snowpark.DataFrame.collect.html#snowflake.snowpark.DataFrame.collect "snowflake.snowpark.DataFrame.collect")() | Executes the query representing this DataFrame and returns the result as a list of [`Row`](snowflake.snowpark.Row.html#snowflake.snowpark.Row "snowflake.snowpark.Row") objects.  
[`collect_nowait`](snowflake.snowpark.DataFrame.collect_nowait.html#snowflake.snowpark.DataFrame.collect_nowait "snowflake.snowpark.DataFrame.collect_nowait")(*[, statement_params, ...]) | Executes the query representing this DataFrame asynchronously and returns: class:AsyncJob.  
[`copy_into_table`](snowflake.snowpark.DataFrame.copy_into_table.html#snowflake.snowpark.DataFrame.copy_into_table "snowflake.snowpark.DataFrame.copy_into_table")(table_name, *[, files, ...]) | Executes a [COPY INTO <table>](https://docs.snowflake.com/en/sql-reference/sql/copy-into-table.html) command to load data from files in a stage location into a specified table.  
[`corr`](snowflake.snowpark.DataFrame.corr.html#snowflake.snowpark.DataFrame.corr "snowflake.snowpark.DataFrame.corr")(col1, col2, *[, statement_params]) | Calculates the correlation coefficient for non-null pairs in two numeric columns.  
[`count`](snowflake.snowpark.DataFrame.count.html#snowflake.snowpark.DataFrame.count "snowflake.snowpark.DataFrame.count")() | Executes the query representing this DataFrame and returns the number of rows in the result (similar to the COUNT function in SQL).  
[`cov`](snowflake.snowpark.DataFrame.cov.html#snowflake.snowpark.DataFrame.cov "snowflake.snowpark.DataFrame.cov")(col1, col2, *[, statement_params]) | Calculates the sample covariance for non-null pairs in two numeric columns.  
[`createOrReplaceTempView`](snowflake.snowpark.DataFrame.createOrReplaceTempView.html#snowflake.snowpark.DataFrame.createOrReplaceTempView "snowflake.snowpark.DataFrame.createOrReplaceTempView")(name, *[, comment, ...]) | Creates or replace a temporary view that returns the same results as this DataFrame.  
[`createOrReplaceView`](snowflake.snowpark.DataFrame.createOrReplaceView.html#snowflake.snowpark.DataFrame.createOrReplaceView "snowflake.snowpark.DataFrame.createOrReplaceView")(name, *[, comment, ...]) | Creates a view that captures the computation expressed by this DataFrame.  
[`createTempView`](snowflake.snowpark.DataFrame.createTempView.html#snowflake.snowpark.DataFrame.createTempView "snowflake.snowpark.DataFrame.createTempView")(name, *[, comment, ...]) | Creates a temporary view that returns the same results as this DataFrame.  
[`create_or_replace_dynamic_table`](snowflake.snowpark.DataFrame.create_or_replace_dynamic_table.html#snowflake.snowpark.DataFrame.create_or_replace_dynamic_table "snowflake.snowpark.DataFrame.create_or_replace_dynamic_table")(name, *, ...) | Creates a dynamic table that captures the computation expressed by this DataFrame.  
[`create_or_replace_temp_view`](snowflake.snowpark.DataFrame.create_or_replace_temp_view.html#snowflake.snowpark.DataFrame.create_or_replace_temp_view "snowflake.snowpark.DataFrame.create_or_replace_temp_view")(name, *[, ...]) | Creates or replace a temporary view that returns the same results as this DataFrame.  
[`create_or_replace_view`](snowflake.snowpark.DataFrame.create_or_replace_view.html#snowflake.snowpark.DataFrame.create_or_replace_view "snowflake.snowpark.DataFrame.create_or_replace_view")(name, *[, comment, ...]) | Creates a view that captures the computation expressed by this DataFrame.  
[`create_temp_view`](snowflake.snowpark.DataFrame.create_temp_view.html#snowflake.snowpark.DataFrame.create_temp_view "snowflake.snowpark.DataFrame.create_temp_view")(name, *[, comment, ...]) | Creates a temporary view that returns the same results as this DataFrame.  
[`crossJoin`](snowflake.snowpark.DataFrame.crossJoin.html#snowflake.snowpark.DataFrame.crossJoin "snowflake.snowpark.DataFrame.crossJoin")(right, *[, lsuffix, rsuffix, directed]) | Performs a cross join, which returns the Cartesian product of the current `DataFrame` and another `DataFrame` (`right`).  
[`cross_join`](snowflake.snowpark.DataFrame.cross_join.html#snowflake.snowpark.DataFrame.cross_join "snowflake.snowpark.DataFrame.cross_join")(right, *[, lsuffix, rsuffix, ...]) | Performs a cross join, which returns the Cartesian product of the current `DataFrame` and another `DataFrame` (`right`).  
[`crosstab`](snowflake.snowpark.DataFrame.crosstab.html#snowflake.snowpark.DataFrame.crosstab "snowflake.snowpark.DataFrame.crosstab")(col1, col2, *[, statement_params]) | Computes a pair-wise frequency table (a `contingency table`) for the specified columns.  
[`cube`](snowflake.snowpark.DataFrame.cube.html#snowflake.snowpark.DataFrame.cube "snowflake.snowpark.DataFrame.cube")(*cols) | Performs a SQL [GROUP BY CUBE](https://docs.snowflake.com/en/sql-reference/constructs/group-by-cube.html).  
[`describe`](snowflake.snowpark.DataFrame.describe.html#snowflake.snowpark.DataFrame.describe "snowflake.snowpark.DataFrame.describe")(*cols[, strings_include_math_stats]) | Computes basic statistics for numeric columns, which includes `count`, `mean`, `stddev`, `min`, and `max`.  
[`distinct`](snowflake.snowpark.DataFrame.distinct.html#snowflake.snowpark.DataFrame.distinct "snowflake.snowpark.DataFrame.distinct")() | Returns a new DataFrame that contains only the rows with distinct values from the current DataFrame.  
[`drop`](snowflake.snowpark.DataFrame.drop.html#snowflake.snowpark.DataFrame.drop "snowflake.snowpark.DataFrame.drop")(*cols) | Returns a new DataFrame that excludes the columns with the specified names from the output.  
[`dropDuplicates`](snowflake.snowpark.DataFrame.dropDuplicates.html#snowflake.snowpark.DataFrame.dropDuplicates "snowflake.snowpark.DataFrame.dropDuplicates")(*subset) | Creates a new DataFrame by removing duplicated rows on given subset of columns.  
[`drop_duplicates`](snowflake.snowpark.DataFrame.drop_duplicates.html#snowflake.snowpark.DataFrame.drop_duplicates "snowflake.snowpark.DataFrame.drop_duplicates")(*subset) | Creates a new DataFrame by removing duplicated rows on given subset of columns.  
[`dropna`](snowflake.snowpark.DataFrame.dropna.html#snowflake.snowpark.DataFrame.dropna "snowflake.snowpark.DataFrame.dropna")([how, thresh, subset]) | Returns a new DataFrame that excludes all rows containing fewer than a specified number of non-null and non-NaN values in the specified columns.  
[`except_`](snowflake.snowpark.DataFrame.except_.html#snowflake.snowpark.DataFrame.except_ "snowflake.snowpark.DataFrame.except_")(other) | Returns a new DataFrame that contains all the rows from the current DataFrame except for the rows that also appear in the `other` DataFrame.  
[`explain`](snowflake.snowpark.DataFrame.explain.html#snowflake.snowpark.DataFrame.explain "snowflake.snowpark.DataFrame.explain")() | Prints the list of queries that will be executed to evaluate this DataFrame.  
[`fillna`](snowflake.snowpark.DataFrame.fillna.html#snowflake.snowpark.DataFrame.fillna "snowflake.snowpark.DataFrame.fillna")(value[, subset, include_decimal]) | Returns a new DataFrame that replaces all null and NaN values in the specified columns with the values provided.  
[`filter`](snowflake.snowpark.DataFrame.filter.html#snowflake.snowpark.DataFrame.filter "snowflake.snowpark.DataFrame.filter")(expr) | Filters rows based on the specified conditional expression (similar to WHERE in SQL).  
[`first`](snowflake.snowpark.DataFrame.first.html#snowflake.snowpark.DataFrame.first "snowflake.snowpark.DataFrame.first")() | Executes the query representing this DataFrame and returns the first `n` rows of the results.  
[`flatten`](snowflake.snowpark.DataFrame.flatten.html#snowflake.snowpark.DataFrame.flatten "snowflake.snowpark.DataFrame.flatten")(input[, path, outer, recursive, mode]) | Flattens (explodes) compound values into multiple rows.  
`get_execution_profile`([output_file, verbose]) | Get the execution profile of the dataframe.  
[`groupBy`](snowflake.snowpark.DataFrame.groupBy.html#snowflake.snowpark.DataFrame.groupBy "snowflake.snowpark.DataFrame.groupBy")(*cols) | Groups rows by the columns specified by expressions (similar to GROUP BY in SQL).  
[`group_by`](snowflake.snowpark.DataFrame.group_by.html#snowflake.snowpark.DataFrame.group_by "snowflake.snowpark.DataFrame.group_by")(*cols) | Groups rows by the columns specified by expressions (similar to GROUP BY in SQL).  
[`group_by_grouping_sets`](snowflake.snowpark.DataFrame.group_by_grouping_sets.html#snowflake.snowpark.DataFrame.group_by_grouping_sets "snowflake.snowpark.DataFrame.group_by_grouping_sets")(*grouping_sets) | Performs a SQL [GROUP BY GROUPING SETS](https://docs.snowflake.com/en/sql-reference/constructs/group-by-grouping-sets.html).  
[`intersect`](snowflake.snowpark.DataFrame.intersect.html#snowflake.snowpark.DataFrame.intersect "snowflake.snowpark.DataFrame.intersect")(other) | Returns a new DataFrame that contains the intersection of rows from the current DataFrame and another DataFrame (`other`).  
[`join`](snowflake.snowpark.DataFrame.join.html#snowflake.snowpark.DataFrame.join "snowflake.snowpark.DataFrame.join")(right[, on, how, lsuffix, rsuffix, ...]) | Performs a join of the specified type (`how`) with the current DataFrame and another DataFrame (`right`) on a list of columns (`on`).  
[`join_table_function`](snowflake.snowpark.DataFrame.join_table_function.html#snowflake.snowpark.DataFrame.join_table_function "snowflake.snowpark.DataFrame.join_table_function")(func, *func_arguments, ...) | Lateral joins the current DataFrame with the output of the specified table function.  
[`lateral_join`](snowflake.snowpark.DataFrame.lateral_join.html#snowflake.snowpark.DataFrame.lateral_join "snowflake.snowpark.DataFrame.lateral_join")(right[, on, lsuffix, rsuffix]) | Performs an inner lateral join with the current DataFrame and another DataFrame (`right`).  
[`limit`](snowflake.snowpark.DataFrame.limit.html#snowflake.snowpark.DataFrame.limit "snowflake.snowpark.DataFrame.limit")(n[, offset]) | Returns a new DataFrame that contains at most `n` rows from the current DataFrame, skipping `offset` rows from the beginning (similar to LIMIT and OFFSET in SQL).  
[`minus`](snowflake.snowpark.DataFrame.minus.html#snowflake.snowpark.DataFrame.minus "snowflake.snowpark.DataFrame.minus")(other) | Returns a new DataFrame that contains all the rows from the current DataFrame except for the rows that also appear in the `other` DataFrame.  
[`natural_join`](snowflake.snowpark.DataFrame.natural_join.html#snowflake.snowpark.DataFrame.natural_join "snowflake.snowpark.DataFrame.natural_join")(right[, how, directed]) | Performs a natural join of the specified type (`how`) with the current DataFrame and another DataFrame (`right`).  
[`orderBy`](snowflake.snowpark.DataFrame.orderBy.html#snowflake.snowpark.DataFrame.orderBy "snowflake.snowpark.DataFrame.orderBy")(*cols[, ascending]) | Sorts a DataFrame by the specified expressions (similar to ORDER BY in SQL).  
[`order_by`](snowflake.snowpark.DataFrame.order_by.html#snowflake.snowpark.DataFrame.order_by "snowflake.snowpark.DataFrame.order_by")(*cols[, ascending]) | Sorts a DataFrame by the specified expressions (similar to ORDER BY in SQL).  
[`pivot`](snowflake.snowpark.DataFrame.pivot.html#snowflake.snowpark.DataFrame.pivot "snowflake.snowpark.DataFrame.pivot")(pivot_col[, values, default_on_null]) | Rotates this DataFrame by turning the unique values from one column in the input expression into multiple columns and aggregating results where required on any remaining column values.  
[`printSchema`](snowflake.snowpark.DataFrame.printSchema.html#snowflake.snowpark.DataFrame.printSchema "snowflake.snowpark.DataFrame.printSchema")([level]) | Prints the schema of a dataframe in tree format.  
[`print_schema`](snowflake.snowpark.DataFrame.print_schema.html#snowflake.snowpark.DataFrame.print_schema "snowflake.snowpark.DataFrame.print_schema")([level]) | Prints the schema of a dataframe in tree format.  
[`randomSplit`](snowflake.snowpark.DataFrame.randomSplit.html#snowflake.snowpark.DataFrame.randomSplit "snowflake.snowpark.DataFrame.randomSplit")(weights[, seed, statement_params]) | Randomly splits the current DataFrame into separate DataFrames, using the specified weights.  
[`random_split`](snowflake.snowpark.DataFrame.random_split.html#snowflake.snowpark.DataFrame.random_split "snowflake.snowpark.DataFrame.random_split")(weights[, seed, statement_params]) | Randomly splits the current DataFrame into separate DataFrames, using the specified weights.  
[`rename`](snowflake.snowpark.DataFrame.rename.html#snowflake.snowpark.DataFrame.rename "snowflake.snowpark.DataFrame.rename")(col_or_mapper[, new_column]) | Returns a DataFrame with the specified column `col_or_mapper` renamed as `new_column`.  
[`replace`](snowflake.snowpark.DataFrame.replace.html#snowflake.snowpark.DataFrame.replace "snowflake.snowpark.DataFrame.replace")(to_replace[, value, subset, ...]) | Returns a new DataFrame that replaces values in the specified columns.  
[`rollup`](snowflake.snowpark.DataFrame.rollup.html#snowflake.snowpark.DataFrame.rollup "snowflake.snowpark.DataFrame.rollup")(*cols) | Performs a SQL [GROUP BY ROLLUP](https://docs.snowflake.com/en/sql-reference/constructs/group-by-rollup.html).  
[`sample`](snowflake.snowpark.DataFrame.sample.html#snowflake.snowpark.DataFrame.sample "snowflake.snowpark.DataFrame.sample")([frac, n]) | Samples rows based on either the number of rows to be returned or a percentage of rows to be returned.  
[`sampleBy`](snowflake.snowpark.DataFrame.sampleBy.html#snowflake.snowpark.DataFrame.sampleBy "snowflake.snowpark.DataFrame.sampleBy")(col, fractions[, seed]) | Returns a DataFrame containing a stratified sample without replacement, based on a `dict` that specifies the fraction for each stratum.  
[`sample_by`](snowflake.snowpark.DataFrame.sample_by.html#snowflake.snowpark.DataFrame.sample_by "snowflake.snowpark.DataFrame.sample_by")(col, fractions[, seed]) | Returns a DataFrame containing a stratified sample without replacement, based on a `dict` that specifies the fraction for each stratum.  
[`select`](snowflake.snowpark.DataFrame.select.html#snowflake.snowpark.DataFrame.select "snowflake.snowpark.DataFrame.select")(*cols) | Returns a new DataFrame with the specified Column expressions as output (similar to SELECT in SQL).  
[`selectExpr`](snowflake.snowpark.DataFrame.selectExpr.html#snowflake.snowpark.DataFrame.selectExpr "snowflake.snowpark.DataFrame.selectExpr")(*exprs) | Projects a set of SQL expressions and returns a new `DataFrame`.  
[`select_expr`](snowflake.snowpark.DataFrame.select_expr.html#snowflake.snowpark.DataFrame.select_expr "snowflake.snowpark.DataFrame.select_expr")(*exprs) | Projects a set of SQL expressions and returns a new `DataFrame`.  
[`show`](snowflake.snowpark.DataFrame.show.html#snowflake.snowpark.DataFrame.show "snowflake.snowpark.DataFrame.show")([n, max_width, statement_params]) | Evaluates this DataFrame and prints out the first `n` rows with the specified maximum number of characters per column.  
[`sort`](snowflake.snowpark.DataFrame.sort.html#snowflake.snowpark.DataFrame.sort "snowflake.snowpark.DataFrame.sort")(*cols[, ascending]) | Sorts a DataFrame by the specified expressions (similar to ORDER BY in SQL).  
[`subtract`](snowflake.snowpark.DataFrame.subtract.html#snowflake.snowpark.DataFrame.subtract "snowflake.snowpark.DataFrame.subtract")(other) | Returns a new DataFrame that contains all the rows from the current DataFrame except for the rows that also appear in the `other` DataFrame.  
[`take`](snowflake.snowpark.DataFrame.take.html#snowflake.snowpark.DataFrame.take "snowflake.snowpark.DataFrame.take")([n, statement_params, block]) | Executes the query representing this DataFrame and returns the first `n` rows of the results.  
[`toDF`](snowflake.snowpark.DataFrame.toDF.html#snowflake.snowpark.DataFrame.toDF "snowflake.snowpark.DataFrame.toDF")(*names) | Creates a new DataFrame containing columns with the specified names.  
[`toLocalIterator`](snowflake.snowpark.DataFrame.toLocalIterator.html#snowflake.snowpark.DataFrame.toLocalIterator "snowflake.snowpark.DataFrame.toLocalIterator")(*[, statement_params, ...]) | Executes the query representing this DataFrame and returns an iterator of [`Row`](snowflake.snowpark.Row.html#snowflake.snowpark.Row "snowflake.snowpark.Row") objects that you can use to retrieve the results.  
[`toPandas`](snowflake.snowpark.DataFrame.toPandas.html#snowflake.snowpark.DataFrame.toPandas "snowflake.snowpark.DataFrame.toPandas")(*[, statement_params, block]) | Executes the query representing this DataFrame and returns the result as a [pandas DataFrame](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.html).  
`to_arrow`(*[, statement_params, block]) | Executes the query representing this DataFrame and returns the result as a pyarrow Table <https://arrow.apache.org/docs/python/generated/pyarrow.Table.html>.  
`to_arrow_batches`(*[, statement_params, block]) | Executes the query representing this DataFrame and returns an iterator of pyarrow Tables (containing a subset of rows) that you can use to retrieve the results.  
[`to_df`](snowflake.snowpark.DataFrame.to_df.html#snowflake.snowpark.DataFrame.to_df "snowflake.snowpark.DataFrame.to_df")(*names) | Creates a new DataFrame containing columns with the specified names.  
[`to_local_iterator`](snowflake.snowpark.DataFrame.to_local_iterator.html#snowflake.snowpark.DataFrame.to_local_iterator "snowflake.snowpark.DataFrame.to_local_iterator")() | Executes the query representing this DataFrame and returns an iterator of [`Row`](snowflake.snowpark.Row.html#snowflake.snowpark.Row "snowflake.snowpark.Row") objects that you can use to retrieve the results.  
[`to_pandas`](snowflake.snowpark.DataFrame.to_pandas.html#snowflake.snowpark.DataFrame.to_pandas "snowflake.snowpark.DataFrame.to_pandas")() | Executes the query representing this DataFrame and returns the result as a [pandas DataFrame](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.html).  
[`to_pandas_batches`](snowflake.snowpark.DataFrame.to_pandas_batches.html#snowflake.snowpark.DataFrame.to_pandas_batches "snowflake.snowpark.DataFrame.to_pandas_batches")() | Executes the query representing this DataFrame and returns an iterator of pandas dataframes (containing a subset of rows) that you can use to retrieve the results.  
[`to_snowpark_pandas`](snowflake.snowpark.DataFrame.to_snowpark_pandas.html#snowflake.snowpark.DataFrame.to_snowpark_pandas "snowflake.snowpark.DataFrame.to_snowpark_pandas")([index_col, columns, ...]) | Convert the Snowpark DataFrame to Snowpark pandas DataFrame.  
[`union`](snowflake.snowpark.DataFrame.union.html#snowflake.snowpark.DataFrame.union "snowflake.snowpark.DataFrame.union")(other) | Returns a new DataFrame that contains all the rows in the current DataFrame and another DataFrame (`other`), excluding any duplicate rows.  
[`unionAll`](snowflake.snowpark.DataFrame.unionAll.html#snowflake.snowpark.DataFrame.unionAll "snowflake.snowpark.DataFrame.unionAll")(other) | Returns a new DataFrame that contains all the rows in the current DataFrame and another DataFrame (`other`), including any duplicate rows.  
[`unionAllByName`](snowflake.snowpark.DataFrame.unionAllByName.html#snowflake.snowpark.DataFrame.unionAllByName "snowflake.snowpark.DataFrame.unionAllByName")(other[, allow_missing_columns]) | Returns a new DataFrame that contains all the rows in the current DataFrame and another DataFrame (`other`), including any duplicate rows.  
[`unionByName`](snowflake.snowpark.DataFrame.unionByName.html#snowflake.snowpark.DataFrame.unionByName "snowflake.snowpark.DataFrame.unionByName")(other[, allow_missing_columns]) | Returns a new DataFrame that contains all the rows in the current DataFrame and another DataFrame (`other`), excluding any duplicate rows.  
[`union_all`](snowflake.snowpark.DataFrame.union_all.html#snowflake.snowpark.DataFrame.union_all "snowflake.snowpark.DataFrame.union_all")(other) | Returns a new DataFrame that contains all the rows in the current DataFrame and another DataFrame (`other`), including any duplicate rows.  
[`union_all_by_name`](snowflake.snowpark.DataFrame.union_all_by_name.html#snowflake.snowpark.DataFrame.union_all_by_name "snowflake.snowpark.DataFrame.union_all_by_name")(other[, allow_missing_columns]) | Returns a new DataFrame that contains all the rows in the current DataFrame and another DataFrame (`other`), including any duplicate rows.  
[`union_by_name`](snowflake.snowpark.DataFrame.union_by_name.html#snowflake.snowpark.DataFrame.union_by_name "snowflake.snowpark.DataFrame.union_by_name")(other[, allow_missing_columns]) | Returns a new DataFrame that contains all the rows in the current DataFrame and another DataFrame (`other`), excluding any duplicate rows.  
[`unpivot`](snowflake.snowpark.DataFrame.unpivot.html#snowflake.snowpark.DataFrame.unpivot "snowflake.snowpark.DataFrame.unpivot")(value_column, name_column, column_list) | Rotates a table by transforming columns into rows.  
[`where`](snowflake.snowpark.DataFrame.where.html#snowflake.snowpark.DataFrame.where "snowflake.snowpark.DataFrame.where")(expr) | Filters rows based on the specified conditional expression (similar to WHERE in SQL).  
[`withColumn`](snowflake.snowpark.DataFrame.withColumn.html#snowflake.snowpark.DataFrame.withColumn "snowflake.snowpark.DataFrame.withColumn")(col_name, col, *[, ...]) | Returns a DataFrame with an additional column with the specified name `col_name`.  
[`withColumnRenamed`](snowflake.snowpark.DataFrame.withColumnRenamed.html#snowflake.snowpark.DataFrame.withColumnRenamed "snowflake.snowpark.DataFrame.withColumnRenamed")(existing, new) | Returns a DataFrame with the specified column `existing` renamed as `new`.  
[`with_column`](snowflake.snowpark.DataFrame.with_column.html#snowflake.snowpark.DataFrame.with_column "snowflake.snowpark.DataFrame.with_column")(col_name, col, *[, ...]) | Returns a DataFrame with an additional column with the specified name `col_name`.  
[`with_column_renamed`](snowflake.snowpark.DataFrame.with_column_renamed.html#snowflake.snowpark.DataFrame.with_column_renamed "snowflake.snowpark.DataFrame.with_column_renamed")(existing, new) | Returns a DataFrame with the specified column `existing` renamed as `new`.  
[`with_columns`](snowflake.snowpark.DataFrame.with_columns.html#snowflake.snowpark.DataFrame.with_columns "snowflake.snowpark.DataFrame.with_columns")(col_names, values, *[, ...]) | Returns a DataFrame with additional columns with the specified names `col_names`.  
  
Attributes

[`ai`](snowflake.snowpark.DataFrame.ai.html#snowflake.snowpark.DataFrame.ai "snowflake.snowpark.DataFrame.ai") | Returns a [`DataFrameAIFunctions`](snowflake.snowpark.DataFrameAIFunctions.html#snowflake.snowpark.DataFrameAIFunctions "snowflake.snowpark.DataFrameAIFunctions") object that provides AI-powered functions for the DataFrame.  
---|---  
`analytics` |   
[`columns`](snowflake.snowpark.DataFrame.columns.html#snowflake.snowpark.DataFrame.columns "snowflake.snowpark.DataFrame.columns") | Returns all column names as a list.  
`dtypes` |   
[`na`](snowflake.snowpark.DataFrame.na.html#snowflake.snowpark.DataFrame.na "snowflake.snowpark.DataFrame.na") | Returns a [`DataFrameNaFunctions`](snowflake.snowpark.DataFrameNaFunctions.html#snowflake.snowpark.DataFrameNaFunctions "snowflake.snowpark.DataFrameNaFunctions") object that provides functions for handling missing values in the DataFrame.  
[`queries`](snowflake.snowpark.DataFrame.queries.html#snowflake.snowpark.DataFrame.queries "snowflake.snowpark.DataFrame.queries") | Returns a `dict` that contains a list of queries that will be executed to evaluate this DataFrame with the key queries, and a list of post-execution actions (e.g., queries to clean up temporary objects) with the key post_actions.  
[`schema`](snowflake.snowpark.DataFrame.schema.html#snowflake.snowpark.DataFrame.schema "snowflake.snowpark.DataFrame.schema") | The definition of the columns in this DataFrame (the "relational schema" for the DataFrame).  
[`session`](snowflake.snowpark.DataFrame.session.html#snowflake.snowpark.DataFrame.session "snowflake.snowpark.DataFrame.session") | Returns a [`snowflake.snowpark.Session`](snowflake.snowpark.Session.html#snowflake.snowpark.Session "snowflake.snowpark.Session") object that provides access to the session the current DataFrame is relying on.  
[`stat`](snowflake.snowpark.DataFrame.stat.html#snowflake.snowpark.DataFrame.stat "snowflake.snowpark.DataFrame.stat") |   
[`write`](snowflake.snowpark.DataFrame.write.html#snowflake.snowpark.DataFrame.write "snowflake.snowpark.DataFrame.write") | Returns a new [`DataFrameWriter`](snowflake.snowpark.DataFrameWriter.html#snowflake.snowpark.DataFrameWriter "snowflake.snowpark.DataFrameWriter") object that you can use to write the data in the `DataFrame` to a Snowflake database or a stage location  
[`is_cached`](snowflake.snowpark.DataFrame.is_cached.html#snowflake.snowpark.DataFrame.is_cached "snowflake.snowpark.DataFrame.is_cached") | Whether the dataframe is cached.
