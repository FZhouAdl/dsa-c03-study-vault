---
title: "Snowpark Scala API Reference 1.18.0 - com.snowflake.snowpark"
source: https://docs.snowflake.com/developer-guide/snowpark/reference/scala/2.12/com/snowflake/snowpark/index.html
cert_domain: domain-2-data-preparation-and-feature-engineering
crawl_depth: 1
crawled: 2026-08-23
---

Snowpark Scala API Reference  1.18.0  [ Developer Guide ](https://docs.snowflake.com/en/developer-guide/snowpark/scala/index.html) < Back 

__ __

#  Packages 

  * [ __ ](../../../index.html "Permalink") package  [ root  ](../../../index.html)

Definition Classes 
     [ root ](../../../index.html)

  * [ __ ](../../../com/index.html "Permalink") package  [ com  ](../../index.html)

Definition Classes 
     [ root ](../../../index.html)

  * [ __ ](../../../com/snowflake/index.html "Permalink") package  [ snowflake  ](../index.html)

Definition Classes 
     [ com ](../../index.html)

  * [ __ ](../../../com/snowflake/snowpark/index.html "Permalink") package  snowpark 

Definition Classes 
     [ snowflake ](../index.html)

  * [ __ ](../../../com/snowflake/snowpark/types/index.html "Permalink") package  [ types  ](types/index.html "This package contains all Snowpark logical types.")

This package contains all Snowpark logical types. 

This package contains all Snowpark logical types. 

Since 
    

0.1.0 

  * [ __ ](../../../com/snowflake/snowpark/udtf/index.html "Permalink") package  [ udtf  ](udtf/index.html)
  * [ ](AsyncJob.html "Provides a way to track an asynchronous query in Snowflake.") [ AsyncJob ](AsyncJob.html "Provides a way to track an asynchronous query in Snowflake.")
  * [ ](CaseExpr.html "Represents a CASE expression.") [ CaseExpr ](CaseExpr.html "Represents a CASE expression.")
  * [ ](Column.html "Represents a column or an expression in a DataFrame.") [ Column ](Column.html "Represents a column or an expression in a DataFrame.")
  * [ ](CopyableDataFrame.html "DataFrame for loading data from files in a stage to a table.") [ CopyableDataFrame ](CopyableDataFrame.html "DataFrame for loading data from files in a stage to a table.")
  * [ ](CopyableDataFrameAsyncActor.html "Provides APIs to execute CopyableDataFrame actions asynchronously.") [ CopyableDataFrameAsyncActor ](CopyableDataFrameAsyncActor.html "Provides APIs to execute CopyableDataFrame actions asynchronously.")
  * [ ](DataFrame.html "Represents a lazily-evaluated relational dataset that contains a collection of Row objects with columns defined by a schema \(column name and type\).") [ DataFrame ](DataFrame.html "Represents a lazily-evaluated relational dataset that contains a collection of Row objects with columns defined by a schema \(column name and type\).")
  * [ ](DataFrameAsyncActor.html "Provides APIs to execute DataFrame actions asynchronously.") [ DataFrameAsyncActor ](DataFrameAsyncActor.html "Provides APIs to execute DataFrame actions asynchronously.")
  * [ ](DataFrameNaFunctions.html "Provides functions for handling missing values in a DataFrame.") [ DataFrameNaFunctions ](DataFrameNaFunctions.html "Provides functions for handling missing values in a DataFrame.")
  * [ ](DataFrameReader.html "Provides methods to load data in various supported formats from a Snowflake stage to a DataFrame.") [ DataFrameReader ](DataFrameReader.html "Provides methods to load data in various supported formats from a Snowflake stage to a DataFrame.")
  * [ ](DataFrameStatFunctions.html "Provides eagerly computed statistical functions for DataFrames.") [ DataFrameStatFunctions ](DataFrameStatFunctions.html "Provides eagerly computed statistical functions for DataFrames.")
  * [ ](DataFrameWriter.html "Provides methods for writing data from a DataFrame to supported output destinations.") [ DataFrameWriter ](DataFrameWriter.html "Provides methods for writing data from a DataFrame to supported output destinations.")
  * [ ](DataFrameWriterAsyncActor.html "Provides APIs to execute DataFrameWriter actions asynchronously.") [ DataFrameWriterAsyncActor ](DataFrameWriterAsyncActor.html "Provides APIs to execute DataFrameWriter actions asynchronously.")
  * [ ](DeleteResult.html "Result of deleting rows in an Updatable") [ DeleteResult ](DeleteResult.html "Result of deleting rows in an Updatable")
  * [ ](FileOperation.html "Provides methods for working on files in a stage.") [ FileOperation ](FileOperation.html "Provides methods for working on files in a stage.")
  * [ ](GetResult.html "Represents the results of downloading a file from a stage location to the local file system.") [ GetResult ](GetResult.html "Represents the results of downloading a file from a stage location to the local file system.")
  * [ ](GroupingSets$.html "Constructors of GroupingSets object.") [ ](GroupingSets.html "A Container of grouping sets that you pass to DataFrame.groupByGroupingSets.") [ GroupingSets ](GroupingSets.html "A Container of grouping sets that you pass to DataFrame.groupByGroupingSets.")
  * [ ](HasCachedResult.html "A DataFrame that returns cached data.") [ HasCachedResult ](HasCachedResult.html "A DataFrame that returns cached data.")
  * [ ](MatchedClauseBuilder.html "Builder for a matched clause.") [ MatchedClauseBuilder ](MatchedClauseBuilder.html "Builder for a matched clause.")
  * [ ](MergeBuilder.html "Builder for a merge action.") [ MergeBuilder ](MergeBuilder.html "Builder for a merge action.")
  * [ ](MergeBuilderAsyncActor.html "Provides APIs to execute MergeBuilder actions asynchronously.") [ MergeBuilderAsyncActor ](MergeBuilderAsyncActor.html "Provides APIs to execute MergeBuilder actions asynchronously.")
  * [ ](MergeResult.html "Result of merging a DataFrame into an Updatable DataFrame") [ MergeResult ](MergeResult.html "Result of merging a DataFrame into an Updatable DataFrame")
  * [ ](MergeTypedAsyncJob.html "Provides a way to track an asynchronously executed action in a MergeBuilder.") [ MergeTypedAsyncJob ](MergeTypedAsyncJob.html "Provides a way to track an asynchronously executed action in a MergeBuilder.")
  * [ ](NotMatchedClauseBuilder.html "Builder for a not matched clause.") [ NotMatchedClauseBuilder ](NotMatchedClauseBuilder.html "Builder for a not matched clause.")
  * [ ](PublicPreview.html) [ PublicPreview ](PublicPreview.html)
  * [ ](PutResult.html "Represents the results of uploading a local file to a stage location.") [ PutResult ](PutResult.html "Represents the results of uploading a local file to a stage location.")
  * [ ](RelationalGroupedDataFrame.html "Represents an underlying DataFrame with rows that are grouped by common values.") [ RelationalGroupedDataFrame ](RelationalGroupedDataFrame.html "Represents an underlying DataFrame with rows that are grouped by common values.")
  * [ ](Row$.html) [ ](Row.html "Represents a row returned by the evaluation of a DataFrame.") [ Row ](Row.html "Represents a row returned by the evaluation of a DataFrame.")
  * [ ](SProcRegistration.html "Provides methods to register a SProc \(Stored Procedure\) in the Snowflake database.") [ SProcRegistration ](SProcRegistration.html "Provides methods to register a SProc \(Stored Procedure\) in the Snowflake database.")
  * [ ](SaveMode$.html "SaveMode configures the behavior when data is written from a DataFrame to a data source using a DataFrameWriter instance.") [ ](SaveMode.html "Please refer to the companion SaveMode$ object.") [ SaveMode ](SaveMode.html "Please refer to the companion SaveMode$ object.")
  * [ ](Session$.html "Companion object to Session that you use to build and create a session.") [ ](Session.html "Establishes a connection with a Snowflake database and provides methods for creating DataFrames and accessing objects for working with files in stages.") [ Session ](Session.html "Establishes a connection with a Snowflake database and provides methods for creating DataFrames and accessing objects for working with files in stages.")
  * [ ](SnowparkClientException.html "Represents a Snowpark client side exception.") [ SnowparkClientException ](SnowparkClientException.html "Represents a Snowpark client side exception.")
  * [ ](StoredProcedure.html "The reference to a Stored Procedure which can be created by Session.sproc.register methods, and used in Session.storedProcedure method.") [ StoredProcedure ](StoredProcedure.html "The reference to a Stored Procedure which can be created by Session.sproc.register methods, and used in Session.storedProcedure method.")
  * [ ](TableFunction.html "Looks up table functions by funcName and returns tableFunction object which can be used in DataFrame.join and Session.tableFunction methods.") [ TableFunction ](TableFunction.html "Looks up table functions by funcName and returns tableFunction object which can be used in DataFrame.join and Session.tableFunction methods.")
  * [ ](TypedAsyncJob.html "Provides a way to track an asynchronously executed action in a DataFrame.") [ TypedAsyncJob ](TypedAsyncJob.html "Provides a way to track an asynchronously executed action in a DataFrame.")
  * [ ](UDFRegistration.html "Provides methods to register lambdas and functions as UDFs in the Snowflake database.") [ UDFRegistration ](UDFRegistration.html "Provides methods to register lambdas and functions as UDFs in the Snowflake database.")
  * [ ](UDTFRegistration.html "Provides methods to register a UDTF \(user-defined table function\) in the Snowflake database.") [ UDTFRegistration ](UDTFRegistration.html "Provides methods to register a UDTF \(user-defined table function\) in the Snowflake database.")
  * [ ](Updatable.html "Represents a lazily-evaluated Updatable.") [ Updatable ](Updatable.html "Represents a lazily-evaluated Updatable.")
  * [ ](UpdatableAsyncActor.html "Provides APIs to execute Updatable actions asynchronously.") [ UpdatableAsyncActor ](UpdatableAsyncActor.html "Provides APIs to execute Updatable actions asynchronously.")
  * [ ](UpdateResult.html "Result of updating rows in an Updatable") [ UpdateResult ](UpdateResult.html "Result of updating rows in an Updatable")
  * [ ](UserDefinedFunction.html "Encapsulates a user defined lambda or function that is returned by UDFRegistration.registerTemporary or by com.snowflake.snowpark.functions.udf.") [ UserDefinedFunction ](UserDefinedFunction.html "Encapsulates a user defined lambda or function that is returned by UDFRegistration.registerTemporary or by com.snowflake.snowpark.functions.udf.")
  * [ ](Window$.html "Contains functions to form WindowSpec.") [ Window ](Window$.html "Contains functions to form WindowSpec.")
  * [ ](WindowSpec.html "Represents a window frame clause.") [ WindowSpec ](WindowSpec.html "Represents a window frame clause.")
  * [ ](WriteFileResult.html "Represents the results of writing data from a DataFrame to a file in a stage.") [ WriteFileResult ](WriteFileResult.html "Represents the results of writing data from a DataFrame to a file in a stage.")
  * [ ](functions$.html "Provides utility functions that generate Column expressions that you can pass to DataFrame transformation methods.") [ functions ](functions$.html "Provides utility functions that generate Column expressions that you can pass to DataFrame transformation methods.")
  * [ ](tableFunctions$.html "Provides utility functions that generate table function expressions that can be passed to DataFrame join method and Session tableFunction method.") [ tableFunctions ](tableFunctions$.html "Provides utility functions that generate table function expressions that can be passed to DataFrame join method and Session tableFunction method.")



p 

[ com ](../../index.html) . [ snowflake ](../index.html)

#  snowpark  [ __ ](../../../com/snowflake/snowpark/index.html "Permalink")

####  package  snowpark 

__ __

Ordering 

  1. Alphabetic 



Visibility 

  1. Public 
  2. All 



###  Type Members 

  1. [ __ ](../../../com/snowflake/snowpark/AsyncJob.html "Permalink") class  [ AsyncJob  ](AsyncJob.html "Provides a way to track an asynchronous query in Snowflake.") extends  AnyRef 

Provides a way to track an asynchronous query in Snowflake. 

Provides a way to track an asynchronous query in Snowflake. 

You can use this object to check the status of an asynchronous query and retrieve the results. 

To check the status of an asynchronous query that you submitted earlier, call [ Session.createAsyncJob ](Session.html#createAsyncJob\(queryID:String\):com.snowflake.snowpark.AsyncJob) , and pass in the query ID. This returns an ` AsyncJob ` object that you can use to check the status of the query and retrieve the query results. 

Example 1: Create an AsyncJob by specifying a valid ` <query_id> ` , check whether the query is running or not, and get the result rows. 
[code] val asyncJob = session.createAsyncJob(<query_id>)
         println(s"Is query ${asyncJob.getQueryId()} running? ${asyncJob.isRunning()}")
         val rows = asyncJob.getRows()
[/code]

Example 2: Create an AsyncJob by specifying a valid ` <query_id> ` and cancel the query if it is still running. 
[code] session.createAsyncJob(<query_id>).cancel()
[/code]

Since 
    

0.11.0 

  2. [ __ ](../../../com/snowflake/snowpark/CaseExpr.html "Permalink") class  [ CaseExpr  ](CaseExpr.html "Represents a CASE expression.") extends [ Column ](Column.html)

Represents a [ CASE ](https://docs.snowflake.com/en/sql-reference/functions/case.html) expression. 

Represents a [ CASE ](https://docs.snowflake.com/en/sql-reference/functions/case.html) expression. 

To construct this object for a CASE expression, call the [ functions.when ](functions$.html#when\(condition:com.snowflake.snowpark.Column,value:Any\):com.snowflake.snowpark.CaseExpr) . specifying a condition and the corresponding result for that condition. Then, call the [ when ](CaseExpr.html#when\(condition:com.snowflake.snowpark.Column,value:Any\):com.snowflake.snowpark.CaseExpr) and [ otherwise ](CaseExpr.html#otherwise\(value:Any\):com.snowflake.snowpark.Column) methods to specify additional conditions and results. 

For example: 
[code] import com.snowflake.snowpark.functions._
         df.select(
           when(col("col").is_null, lit(1))
             .when(col("col") === 1, lit(2))
             .otherwise(lit(3))
         )
[/code]

Since 
    

0.2.0 

  3. [ __ ](../../../com/snowflake/snowpark/Column.html "Permalink") case class  [ Column  ](Column.html "Represents a column or an expression in a DataFrame.") extends  Logging  with  Product  with  Serializable 

Represents a column or an expression in a DataFrame. 

Represents a column or an expression in a DataFrame. 

To create a Column object to refer to a column in a DataFrame, you can: 

     * Use the [ functions.col ](functions$.html#col\(colName:String\):com.snowflake.snowpark.Column) function. 
     * Use the [ DataFrame.col ](DataFrame.html#col\(colName:String\):com.snowflake.snowpark.Column) method. 
     * Use the shorthand for the [ DataFrame.apply ](DataFrame.html#apply\(colName:String\):com.snowflake.snowpark.Column) method ( ` <dataframe> ` ("<col_name>") ` ). `

For example: 
[code] import com.snowflake.snowpark.functions.col
    df.select(col("name"))
    df.select(df.col("name"))
    dfLeft.select(dfRight, dfLeft("name") === dfRight("name"))
[/code]

This class also defines utility functions for constructing expressions with Columns. 

The following examples demonstrate how to use Column objects in expressions: 
[code] df
     .filter(col("id") === 20)
     .filter((col("a") + col("b")) < 10)
     .select((col("b") * 10) as "c")
[/code]

Since 
    

0.1.0 

  4. [ __ ](../../../com/snowflake/snowpark/CopyableDataFrame.html "Permalink") class  [ CopyableDataFrame  ](CopyableDataFrame.html "DataFrame for loading data from files in a stage to a table.") extends [ DataFrame ](DataFrame.html)

DataFrame for loading data from files in a stage to a table. 

DataFrame for loading data from files in a stage to a table. Objects of this type are returned by the [ DataFrameReader ](DataFrameReader.html) methods that load data from files (e.g. [ csv ](DataFrameReader.html#csv\(path:String\):com.snowflake.snowpark.CopyableDataFrame) ). 

To save the data from the staged files to a table, call the ` copyInto() ` methods. This method uses the COPY INTO ` <table_name> ` command to copy the data to a specified table. 

Since 
    

0.9.0 

  5. [ __ ](../../../com/snowflake/snowpark/CopyableDataFrameAsyncActor.html "Permalink") class  [ CopyableDataFrameAsyncActor  ](CopyableDataFrameAsyncActor.html "Provides APIs to execute CopyableDataFrame actions asynchronously.") extends [ DataFrameAsyncActor ](DataFrameAsyncActor.html)

Provides APIs to execute CopyableDataFrame actions asynchronously. 

Provides APIs to execute CopyableDataFrame actions asynchronously. 

Since 
    

0.11.0 

  6. [ __ ](../../../com/snowflake/snowpark/DataFrame.html "Permalink") class  [ DataFrame  ](DataFrame.html "Represents a lazily-evaluated relational dataset that contains a collection of Row objects with columns defined by a schema \(column name and type\).") extends  Logging 

Represents a lazily-evaluated relational dataset that contains a collection of [ Row ](Row.html) objects with columns defined by a schema (column name and type). 

Represents a lazily-evaluated relational dataset that contains a collection of [ Row ](Row.html) objects with columns defined by a schema (column name and type). 

A DataFrame is considered lazy because it encapsulates the computation or query required to produce a relational dataset. The computation is not performed until you call a method that performs an action (e.g. [ collect ](DataFrame.html#collect\(\):Array\[com.snowflake.snowpark.Row\]) ). 

**Creating a DataFrame**

You can create a DataFrame in a number of different ways, as shown in the examples below. 

Example 1: Creating a DataFrame by reading a table. 
[code] val dfPrices = session.table("itemsdb.publicschema.prices")
[/code]

Example 2: Creating a DataFrame by reading files from a stage. 
[code] val dfCatalog = session.read.csv("@stage/some_dir")
[/code]

Example 3: Creating a DataFrame by specifying a sequence or a range. 
[code] val df = session.createDataFrame(Seq((1, "one"), (2, "two")))
[/code]
[code] val df = session.range(1, 10, 2)
[/code]

Example 4: Create a new DataFrame by applying transformations to other existing DataFrames. 
[code] val dfMergedData = dfCatalog.join(dfPrices, dfCatalog("itemId") === dfPrices("ID"))
[/code]

**Performing operations on a DataFrame**

Broadly, the operations on DataFrame can be divided into two types: 

     * **Transformations** produce a new DataFrame from one or more existing DataFrames. Note that tranformations are lazy and don't cause the DataFrame to be evaluated. If the API does not provide a method to express the SQL that you want to use, you can use [ functions.sqlExpr ](functions$.html#sqlExpr\(sqlText:String\):com.snowflake.snowpark.Column) as a workaround. 
     * **Actions** cause the DataFrame to be evaluated. When you call a method that performs an action, Snowpark sends the SQL query for the DataFrame to the server for evaluation. 

**Transforming a DataFrame**

The following examples demonstrate how you can transform a DataFrame. 

Example 5. Using the [ select ](DataFrame.html#select\(first:com.snowflake.snowpark.Column,remaining:com.snowflake.snowpark.Column*\):com.snowflake.snowpark.DataFrame) method to select the columns that should be in the DataFrame (similar to adding a ` SELECT ` clause). 
[code] // Return a new DataFrame containing the ID and amount columns of the prices table. This is
    // equivalent to:
    //   SELECT ID, AMOUNT FROM PRICES;
    val dfPriceIdsAndAmounts = dfPrices.select(col("ID"), col("amount"))
[/code]

Example 6. Using the [ Column.as ](Column.html#as\(alias:String\):com.snowflake.snowpark.Column) method to rename a column in a DataFrame (similar to using ` SELECT col AS alias ` ). 
[code] // Return a new DataFrame containing the ID column of the prices table as a column named
    // itemId. This is equivalent to:
    //   SELECT ID AS itemId FROM PRICES;
    val dfPriceItemIds = dfPrices.select(col("ID").as("itemId"))
[/code]

Example 7. Using the [ filter ](DataFrame.html#filter\(condition:com.snowflake.snowpark.Column\):com.snowflake.snowpark.DataFrame) method to filter data (similar to adding a ` WHERE ` clause). 
[code] // Return a new DataFrame containing the row from the prices table with the ID 1. This is
    // equivalent to:
    //   SELECT * FROM PRICES WHERE ID = 1;
    val dfPrice1 = dfPrices.filter((col("ID") === 1))
[/code]

Example 8. Using the [ sort ](DataFrame.html#sort\(first:com.snowflake.snowpark.Column,remaining:com.snowflake.snowpark.Column*\):com.snowflake.snowpark.DataFrame) method to specify the sort order of the data (similar to adding an ` ORDER BY ` clause). 
[code] // Return a new DataFrame for the prices table with the rows sorted by ID. This is equivalent
    // to:
    //   SELECT * FROM PRICES ORDER BY ID;
    val dfSortedPrices = dfPrices.sort(col("ID"))
[/code]

Example 9. Using the [ groupBy ](DataFrame.html#groupBy\(first:com.snowflake.snowpark.Column,remaining:com.snowflake.snowpark.Column*\):com.snowflake.snowpark.RelationalGroupedDataFrame) method to return a [ RelationalGroupedDataFrame ](RelationalGroupedDataFrame.html) that you can use to group and aggregate results (similar to adding a ` GROUP BY ` clause). 

[ RelationalGroupedDataFrame ](RelationalGroupedDataFrame.html) provides methods for aggregating results, including: 

     * [ avg ](RelationalGroupedDataFrame.html#avg\(cols:com.snowflake.snowpark.Column*\):com.snowflake.snowpark.DataFrame) (equivalent to AVG(column)) 
     * [ count ](RelationalGroupedDataFrame.html#count\(\):com.snowflake.snowpark.DataFrame) (equivalent to COUNT()) 
     * [ max ](RelationalGroupedDataFrame.html#max\(cols:com.snowflake.snowpark.Column*\):com.snowflake.snowpark.DataFrame) (equivalent to MAX(column)) 
     * [ median ](RelationalGroupedDataFrame.html#median\(cols:com.snowflake.snowpark.Column*\):com.snowflake.snowpark.DataFrame) (equivalent to MEDIAN(column)) 
     * [ min ](RelationalGroupedDataFrame.html#min\(cols:com.snowflake.snowpark.Column*\):com.snowflake.snowpark.DataFrame) (equivalent to MIN(column)) 
     * [ sum ](RelationalGroupedDataFrame.html#sum\(cols:com.snowflake.snowpark.Column*\):com.snowflake.snowpark.DataFrame) (equivalent to SUM(column)) 
[code] // Return a new DataFrame for the prices table that computes the sum of the prices by
    // category. This is equivalent to:
    //   SELECT CATEGORY, SUM(AMOUNT) FROM PRICES GROUP BY CATEGORY;
    val dfTotalPricePerCategory = dfPrices.groupBy(col("category")).sum(col("amount"))
[/code]

Example 10. Using a [ Window ](Window$.html) to build a [ WindowSpec ](WindowSpec.html) object that you can use for [ windowing functions ](https://docs.snowflake.com/en/user-guide/functions-window-using.html) (similar to using '<function> OVER ... PARTITION BY ... ORDER BY'). 
[code] // Define a window that partitions prices by category and sorts the prices by date within the
    // partition.
    val window = Window.partitionBy(col("category")).orderBy(col("price_date"))
    // Calculate the running sum of prices over this window. This is equivalent to:
    //   SELECT CATEGORY, PRICE_DATE, SUM(AMOUNT) OVER
    //       (PARTITION BY CATEGORY ORDER BY PRICE_DATE)
    //       FROM PRICES ORDER BY PRICE_DATE;
    val dfCumulativePrices = dfPrices.select(
        col("category"), col("price_date"),
        sum(col("amount")).over(window)).sort(col("price_date"))
[/code]

**Performing an action on a DataFrame**

The following examples demonstrate how you can perform an action on a DataFrame. 

Example 11: Performing a query and returning an array of Rows. 
[code] val results = dfPrices.collect()
[/code]

Example 12: Performing a query and print the results. 
[code] dfPrices.show()
[/code]

Since 
    

0.1.0 

  7. [ __ ](../../../com/snowflake/snowpark/DataFrameAsyncActor.html "Permalink") class  [ DataFrameAsyncActor  ](DataFrameAsyncActor.html "Provides APIs to execute DataFrame actions asynchronously.") extends  AnyRef 

Provides APIs to execute DataFrame actions asynchronously. 

Provides APIs to execute DataFrame actions asynchronously. 

Since 
    

0.11.0 

  8. [ __ ](../../../com/snowflake/snowpark/DataFrameNaFunctions.html "Permalink") final  class  [ DataFrameNaFunctions  ](DataFrameNaFunctions.html "Provides functions for handling missing values in a DataFrame.") extends  Logging 

Provides functions for handling missing values in a DataFrame. 

Provides functions for handling missing values in a DataFrame. 

Since 
    

0.2.0 

  9. [ __ ](../../../com/snowflake/snowpark/DataFrameReader.html "Permalink") class  [ DataFrameReader  ](DataFrameReader.html "Provides methods to load data in various supported formats from a Snowflake stage to a DataFrame.") extends  AnyRef 

Provides methods to load data in various supported formats from a Snowflake stage to a DataFrame. 

Provides methods to load data in various supported formats from a Snowflake stage to a DataFrame. The paths provided to the DataFrameReader must refer to Snowflake stages. 

To use this object: 

     1. Access an instance of a DataFrameReader by calling the [ Session.read ](Session.html#read:com.snowflake.snowpark.DataFrameReader) method. 
     2. Specify any [ format-specific options ](https://docs.snowflake.com/en/sql-reference/sql/create-file-format.html#format-type-options-formattypeoptions) and [ copy options ](https://docs.snowflake.com/en/sql-reference/sql/copy-into-table.html#copy-options-copyoptions) by calling the [ option ](DataFrameReader.html#option\(key:String,value:Any\):com.snowflake.snowpark.DataFrameReader) or [ options ](DataFrameReader.html#options\(configs:Map\[String,Any\]\):com.snowflake.snowpark.DataFrameReader) method. These methods return a DataFrameReader that is configured with these options. (Note that although specifying copy options can make error handling more robust during the reading process, it may have an effect on performance.) 
     3. Specify the schema of the data that you plan to load by constructing a [ types.StructType ](types/StructType.html) object and passing it to the [ schema ](DataFrameReader.html#schema\(schema:com.snowflake.snowpark.types.StructType\):com.snowflake.snowpark.DataFrameReader) method. This method returns a DataFrameReader that is configured to read data that uses the specified schema. 
     4. Specify the format of the data by calling the method named after the format (e.g. [ csv ](DataFrameReader.html#csv\(path:String\):com.snowflake.snowpark.CopyableDataFrame) , [ json ](DataFrameReader.html#json\(path:String\):com.snowflake.snowpark.CopyableDataFrame) , etc.). These methods return a [ DataFrame ](DataFrame.html) that is configured to load data in the specified format. 
     5. Call a [ DataFrame ](DataFrame.html) method that performs an action. 
        * For example, to load the data from the file, call [ DataFrame.collect ](DataFrame.html#collect\(\):Array\[com.snowflake.snowpark.Row\]) . 
        * As another example, to save the data from the file to a table, call [ CopyableDataFrame.copyInto(tableName:String)* ](CopyableDataFrame.html#copyInto\(tableName:String\):Unit) . This uses the COPY INTO ` <table_name> ` command. 

The following examples demonstrate how to use a DataFrameReader. 

**Example 1:** Loading the first two columns of a CSV file and skipping the first header line. 
[code] // Import the package for StructType.
    import com.snowflake.snowpark.types._
    val filePath = "@mystage1"
    // Define the schema for the data in the CSV file.
    val userSchema = StructType(Seq(StructField("a", IntegerType), StructField("b", StringType)))
    // Create a DataFrame that is configured to load data from the CSV file.
    val csvDF = session.read.option("skip_header", 1).schema(userSchema).csv(filePath)
    // Load the data into the DataFrame and return an Array of Rows containing the results.
    val results = csvDF.collect()
[/code]

**Example 2:** Loading a gzip compressed json file. 
[code] val filePath = "@mystage2/data.json.gz"
    // Create a DataFrame that is configured to load data from the gzipped JSON file.
    val jsonDF = session.read.option("compression", "gzip").json(filePath)
    // Load the data into the DataFrame and return an Array of Rows containing the results.
    val results = jsonDF.collect()
[/code]

If you want to load only a subset of files from the stage, you can use the [ pattern ](https://docs.snowflake.com/en/sql-reference/sql/copy-into-table.html#loading-using-pattern-matching) option to specify a regular expression that matches the files that you want to load. 

**Example 3:** Loading only the CSV files from a stage location. 
[code] import com.snowflake.snowpark.types._
    // Define the schema for the data in the CSV files.
    val userSchema: StructType = StructType(Seq(StructField("a", IntegerType),StructField("b", StringType)))
    // Create a DataFrame that is configured to load data from the CSV files in the stage.
    val csvDF = session.read.option("pattern", ".*[.]csv").schema(userSchema).csv("@stage_location")
    // Load the data into the DataFrame and return an Array of Rows containing the results.
    val results = csvDF.collect()
[/code]

In addition, if you want to load the files from the stage into a specified table with COPY INTO ` <table_name> ` command, you can use a ` copyInto() ` method e.g. [ CopyableDataFrame.copyInto(tableName:String)* ](CopyableDataFrame.html#copyInto\(tableName:String\):Unit) . 

**Example 4:** Loading data from a JSON file in a stage to a table by using COPY INTO ` <table_name> ` . 
[code] val filePath = "@mystage1"
    // Create a DataFrame that is configured to load data from the JSON file.
    val jsonDF = session.read.json(filePath)
    // Load the data into the specified table `T1`.
    // The table "T1" should exist before calling copyInto().
    jsonDF.copyInto("T1")
[/code]

Since 
    

0.1.0 

  10. [ __ ](../../../com/snowflake/snowpark/DataFrameStatFunctions.html "Permalink") final  class  [ DataFrameStatFunctions  ](DataFrameStatFunctions.html "Provides eagerly computed statistical functions for DataFrames.") extends  Logging 

Provides eagerly computed statistical functions for DataFrames. 

Provides eagerly computed statistical functions for DataFrames. 

To access an object of this class, use [ DataFrame.stat ](DataFrame.html#stat:com.snowflake.snowpark.DataFrameStatFunctions) . 

Since 
    

0.2.0 

  11. [ __ ](../../../com/snowflake/snowpark/DataFrameWriter.html "Permalink") class  [ DataFrameWriter  ](DataFrameWriter.html "Provides methods for writing data from a DataFrame to supported output destinations.") extends  AnyRef 

Provides methods for writing data from a DataFrame to supported output destinations. 

Provides methods for writing data from a DataFrame to supported output destinations. 

You can write data to the following locations: 

     * A Snowflake table 
     * A file on a stage 

###  Saving Data to a Table 

To use this object to write into a table: 

     1. Access an instance of a DataFrameWriter by calling the [ DataFrame.write ](DataFrame.html#write:com.snowflake.snowpark.DataFrameWriter) method. 
     2. Specify the save mode to use (overwrite or append) by calling the [ mode ](DataFrameWriter.html#mode\(saveMode:com.snowflake.snowpark.SaveMode\):com.snowflake.snowpark.DataFrameWriter) method. This method returns a DataFrameWriter that is configured to save data using the specified mode. The default [ SaveMode ](SaveMode.html) is [ SaveMode.Append ](SaveMode$$Append$.html) . 
     3. (Optional) If you need to set some options for the save operation (e.g. columnOrder), call the [ options ](DataFrameWriter.html#options\(configs:Map\[String,Any\]\):com.snowflake.snowpark.DataFrameWriter) or [ option ](DataFrameWriter.html#option\(key:String,value:Any\):com.snowflake.snowpark.DataFrameWriter) method. 
     4. Call a ` saveAs* ` method to save the data to the specified destination. 

For example: 
[code] df.write.mode("overwrite").saveAsTable("T")
[/code]

###  Saving Data to a File on a Stage 

To save data to a file on a stage: 

     1. Access an instance of a DataFrameWriter by calling the [ DataFrame.write ](DataFrame.html#write:com.snowflake.snowpark.DataFrameWriter) method. 
     2. Specify the save mode to use (Overwrite or ErrorIfExists) by calling the [ mode ](DataFrameWriter.html#mode\(saveMode:com.snowflake.snowpark.SaveMode\):com.snowflake.snowpark.DataFrameWriter) method. This method returns a DataFrameWriter that is configured to save data using the specified mode. The default [ SaveMode ](SaveMode.html) is [ SaveMode.ErrorIfExists ](SaveMode$$ErrorIfExists$.html) for this case. 
     3. (Optional) If you need to set some options for the save operation (e.g. file format options), call the [ options ](DataFrameWriter.html#options\(configs:Map\[String,Any\]\):com.snowflake.snowpark.DataFrameWriter) or [ option ](DataFrameWriter.html#option\(key:String,value:Any\):com.snowflake.snowpark.DataFrameWriter) method. 
     4. Call the method named after a file format to save the data in the specified format: 
        * To save the data in CSV format, call the [ csv ](DataFrameWriter.html#csv\(path:String\):com.snowflake.snowpark.WriteFileResult) method. 
        * To save the data in JSON format, call the [ json ](DataFrameWriter.html#json\(path:String\):com.snowflake.snowpark.WriteFileResult) method. 
        * To save the data in PARQUET format, call the [ parquet ](DataFrameWriter.html#parquet\(path:String\):com.snowflake.snowpark.WriteFileResult) method. 

For example: 

**Example 1:** Write a DataFrame to a CSV file. 
[code] val result = df.write.csv("@myStage/prefix")
[/code]

**Example 2:** Write a DataFrame to a CSV file without compression. 
[code] val result = df.write.option("compression", "none").csv("@myStage/prefix")
[/code]

Since 
    

0.1.0 

  12. [ __ ](../../../com/snowflake/snowpark/DataFrameWriterAsyncActor.html "Permalink") class  [ DataFrameWriterAsyncActor  ](DataFrameWriterAsyncActor.html "Provides APIs to execute DataFrameWriter actions asynchronously.") extends  AnyRef 

Provides APIs to execute DataFrameWriter actions asynchronously. 

Provides APIs to execute DataFrameWriter actions asynchronously. 

Since 
    

0.11.0 

  13. [ __ ](../../../com/snowflake/snowpark/DeleteResult.html "Permalink") case class  [ DeleteResult  ](DeleteResult.html "Result of deleting rows in an Updatable") (  rowsDeleted:  Long  )  extends  Product  with  Serializable 

Result of deleting rows in an Updatable 

Result of deleting rows in an Updatable 

Since 
    

0.7.0 

  14. [ __ ](../../../com/snowflake/snowpark/FileOperation.html "Permalink") final  class  [ FileOperation  ](FileOperation.html "Provides methods for working on files in a stage.") extends  Logging 

Provides methods for working on files in a stage. 

Provides methods for working on files in a stage. 

To access an object of this class, use [ Session.file ](Session.html#file:com.snowflake.snowpark.FileOperation) . 

For example: 
[code] // Upload a file to a stage.
         session.file.put("file:///tmp/file1.csv", "@myStage/prefix1")
         // Download a file from a stage.
         session.file.get("@myStage/prefix1/file1.csv", "file:///tmp")
[/code]

Since 
    

0.4.0 

  15. [ __ ](../../../com/snowflake/snowpark/GetResult.html "Permalink") case class  [ GetResult  ](GetResult.html "Represents the results of downloading a file from a stage location to the local file system.") (  fileName:  String  ,  sizeBytes:  Long  ,  status:  String  ,  encryption:  String  ,  message:  String  )  extends  Product  with  Serializable 

Represents the results of downloading a file from a stage location to the local file system. 

Represents the results of downloading a file from a stage location to the local file system. 

NOTE: ` fileName ` is the relative path to the file on the stage. For example, if you download ` @myStage/prefix1/file1.csv.gz ` , ` fileName ` is ` prefix1/file1.csv.gz ` . 

Since 
    

0.4.0 

  16. [ __ ](../../../com/snowflake/snowpark/GroupingSets.html "Permalink") case class  [ GroupingSets  ](GroupingSets.html "A Container of grouping sets that you pass to DataFrame.groupByGroupingSets.") (  sets:  Seq  [  Set  [ [ Column ](Column.html) ]]  )  extends  Product  with  Serializable 

A Container of grouping sets that you pass to [ DataFrame.groupByGroupingSets ](DataFrame.html#groupByGroupingSets\(groupingSets:Seq\[com.snowflake.snowpark.GroupingSets\]\):com.snowflake.snowpark.RelationalGroupedDataFrame) . 

A Container of grouping sets that you pass to [ DataFrame.groupByGroupingSets ](DataFrame.html#groupByGroupingSets\(groupingSets:Seq\[com.snowflake.snowpark.GroupingSets\]\):com.snowflake.snowpark.RelationalGroupedDataFrame) . 

sets 
    

a list of grouping sets 

Since 
    

0.4.0 

  17. [ __ ](../../../com/snowflake/snowpark/HasCachedResult.html "Permalink") class  [ HasCachedResult  ](HasCachedResult.html "A DataFrame that returns cached data.") extends [ DataFrame ](DataFrame.html)

A DataFrame that returns cached data. 

A DataFrame that returns cached data. Repeated invocations of actions on this type of dataframe are guaranteed to produce the same results. It is returned from ` cacheResult ` functions (e.g. [ DataFrame.cacheResult ](DataFrame.html#cacheResult\(\):com.snowflake.snowpark.HasCachedResult) ). 

Since 
    

0.4.0 

  18. [ __ ](../../../com/snowflake/snowpark/MatchedClauseBuilder.html "Permalink") class  [ MatchedClauseBuilder  ](MatchedClauseBuilder.html "Builder for a matched clause.") extends  AnyRef 

Builder for a matched clause. 

Builder for a matched clause. It provides APIs to build update and delete actions 

Since 
    

0.7.0 

  19. [ __ ](../../../com/snowflake/snowpark/MergeBuilder.html "Permalink") class  [ MergeBuilder  ](MergeBuilder.html "Builder for a merge action.") extends  AnyRef 

Builder for a merge action. 

Builder for a merge action. It provides APIs to build matched and not matched clauses. 

Since 
    

0.7.0 

  20. [ __ ](../../../com/snowflake/snowpark/MergeBuilderAsyncActor.html "Permalink") class  [ MergeBuilderAsyncActor  ](MergeBuilderAsyncActor.html "Provides APIs to execute MergeBuilder actions asynchronously.") extends  AnyRef 

Provides APIs to execute MergeBuilder actions asynchronously. 

Provides APIs to execute MergeBuilder actions asynchronously. 

Since 
    

1.3.0 

  21. [ __ ](../../../com/snowflake/snowpark/MergeResult.html "Permalink") case class  [ MergeResult  ](MergeResult.html "Result of merging a DataFrame into an Updatable DataFrame") (  rowsInserted:  Long  ,  rowsUpdated:  Long  ,  rowsDeleted:  Long  )  extends  Product  with  Serializable 

Result of merging a DataFrame into an Updatable DataFrame 

Result of merging a DataFrame into an Updatable DataFrame 

Since 
    

0.7.0 

  22. [ __ ](../../../com/snowflake/snowpark/MergeTypedAsyncJob.html "Permalink") class  [ MergeTypedAsyncJob  ](MergeTypedAsyncJob.html "Provides a way to track an asynchronously executed action in a MergeBuilder.") extends [ TypedAsyncJob ](TypedAsyncJob.html) [ [ MergeResult ](MergeResult.html) ] 

Provides a way to track an asynchronously executed action in a MergeBuilder. 

Provides a way to track an asynchronously executed action in a MergeBuilder. 

Since 
    

1.3.0 

  23. [ __ ](../../../com/snowflake/snowpark/NotMatchedClauseBuilder.html "Permalink") class  [ NotMatchedClauseBuilder  ](NotMatchedClauseBuilder.html "Builder for a not matched clause.") extends  AnyRef 

Builder for a not matched clause. 

Builder for a not matched clause. It provides APIs to build insert actions 

Since 
    

0.7.0 

  24. [ __ ](../../../com/snowflake/snowpark/PublicPreview.html "Permalink") class  [ PublicPreview  ](PublicPreview.html) extends  Annotation  with  Annotation  with  ClassfileAnnotation 

Annotations 
     @Documented  () 

  25. [ __ ](../../../com/snowflake/snowpark/PutResult.html "Permalink") case class  [ PutResult  ](PutResult.html "Represents the results of uploading a local file to a stage location.") (  sourceFileName:  String  ,  targetFileName:  String  ,  sourceSizeBytes:  Long  ,  targetSizeBytes:  Long  ,  sourceCompression:  String  ,  targetCompression:  String  ,  status:  String  ,  encryption:  String  ,  message:  String  )  extends  Product  with  Serializable 

Represents the results of uploading a local file to a stage location. 

Represents the results of uploading a local file to a stage location. 

Since 
    

0.4.0 

  26. [ __ ](../../../com/snowflake/snowpark/RelationalGroupedDataFrame.html "Permalink") class  [ RelationalGroupedDataFrame  ](RelationalGroupedDataFrame.html "Represents an underlying DataFrame with rows that are grouped by common values.") extends  AnyRef 

Represents an underlying DataFrame with rows that are grouped by common values. 

Represents an underlying DataFrame with rows that are grouped by common values. Can be used to define aggregations on these grouped DataFrames. 

Example: 
[code] val groupedDf: RelationalGroupedDataFrame = df.groupBy("dept")
         val aggDf: DataFrame = groupedDf.agg(groupedDf("salary") -> "mean")
[/code]

The methods [ DataFrame.groupBy ](DataFrame.html#groupBy\(cols:Array\[String\]\):com.snowflake.snowpark.RelationalGroupedDataFrame) , [ DataFrame.cube ](DataFrame.html#cube\(cols:Seq\[String\]\):com.snowflake.snowpark.RelationalGroupedDataFrame) and [ DataFrame.rollup ](DataFrame.html#rollup\(cols:Array\[String\]\):com.snowflake.snowpark.RelationalGroupedDataFrame) return an instance of type [ RelationalGroupedDataFrame ](RelationalGroupedDataFrame.html)

Since 
    

0.1.0 

  27. [ __ ](../../../com/snowflake/snowpark/Row.html "Permalink") class  [ Row  ](Row.html "Represents a row returned by the evaluation of a DataFrame.") extends  Serializable 

Represents a row returned by the evaluation of a [ DataFrame ](DataFrame.html) . 

Represents a row returned by the evaluation of a [ DataFrame ](DataFrame.html) . 

Since 
    

0.1.0 

  28. [ __ ](../../../com/snowflake/snowpark/SProcRegistration.html "Permalink") class  [ SProcRegistration  ](SProcRegistration.html "Provides methods to register a SProc \(Stored Procedure\) in the Snowflake database.") extends  AnyRef 

Provides methods to register a SProc (Stored Procedure) in the Snowflake database. 

Provides methods to register a SProc (Stored Procedure) in the Snowflake database. 

[ Session.sproc ](Session.html#sproc:com.snowflake.snowpark.SProcRegistration) returns an object of this class. 

To register anonymous temporary SProcs which work in the current session: 
[code] val sp = session.sproc.registerTemporary((session: Session, num: Int) => s"num: $num")
         session.storedProcedure(sp, 123)
[/code]

To register named temporary SProcs which work in the current session: 
[code] val name = "sproc"
         val sp = session.sproc.registerTemporary(name,
           (session: Session, num: Int) => s"num: $num")
         session.storedProcedure(sp, 123)
         session.storedProcedure(name, 123)
[/code]

It requires a user stage when registering a permanent SProc. Snowpark will upload all JAR files for the SProc and any dependencies. It is also required to specify Owner or Caller modes via the parameter 'isCallerMode'. 
[code] val name = "sproc"
         val stageName = "<a user stage name>"
         val sp = session.sproc.registerPermanent(name,
           (session: Session, num: Int) => s"num: $num",
           stageName,
           isCallerMode = true)
         session.storedProcedure(sp, 123)
         session.storedProcedure(name, 123)
[/code]

This object also provides a convenient methods to execute SProc lambda functions directly with current session on the client side. The functions are designed for debugging and development only. Since the local and Snowflake server environments are different, the outputs of executing a SP function with these test function and on Snowflake server may be different too. 
[code] // a client side Scala lambda
         val func = (session: Session, num: Int) => s"num: $num"
         // register a server side stored procedure
         val sp = session.sproc.registerTemporary(func)
         // execute the lambda function of this SP from the client side
         val localResult = session.sproc.runLocally(func, 123)
         // execute this SP from the server side
         val resultDF = session.storedProcedure(sp, 123)
[/code]

Since 
    

1.8.0 

  29. [ __ ](../../../com/snowflake/snowpark/SaveMode.html "Permalink") sealed  trait  [ SaveMode  ](SaveMode.html "Please refer to the companion SaveMode$ object.") extends  AnyRef 

Please refer to the companion [ SaveMode$ ](SaveMode$.html) object. 

Please refer to the companion [ SaveMode$ ](SaveMode$.html) object. 

Since 
    

0.1.0 

  30. [ __ ](../../../com/snowflake/snowpark/Session.html "Permalink") class  [ Session  ](Session.html "Establishes a connection with a Snowflake database and provides methods for creating DataFrames and accessing objects for working with files in stages.") extends  Logging 

Establishes a connection with a Snowflake database and provides methods for creating DataFrames and accessing objects for working with files in stages. 

Establishes a connection with a Snowflake database and provides methods for creating DataFrames and accessing objects for working with files in stages. 

When you create a ` Session ` object, you provide configuration settings to establish a connection with a Snowflake database (e.g. the URL for the account, a user name, etc.). You can specify these settings in a configuration file or in a Map that associates configuration setting names with values. 

To create a Session from a file: 
[code] val session = Session.builder.configFile("/path/to/file.properties").create
[/code]

To create a Session from a map of configuration properties: 
[code] val configMap = Map(
         "URL" -> "demo.snowflakecomputing.com",
         "USER" -> "testUser",
         "PASSWORD" -> "******",
         "ROLE" -> "myrole",
         "WAREHOUSE" -> "warehouse1",
         "DB" -> "db1",
         "SCHEMA" -> "schema1"
         )
         Session.builder.configs(configMap).create
[/code]

Session contains functions to construct [ DataFrame ](DataFrame.html) s like [ Session.table ](Session.html#table\(name:String\):com.snowflake.snowpark.Updatable) , [ Session.sql ](Session.html#sql\(query:String,params:Seq\[Any\]\):com.snowflake.snowpark.DataFrame) , and [ Session.read ](Session.html#read:com.snowflake.snowpark.DataFrameReader)

Since 
    

0.1.0 

  31. [ __ ](../../../com/snowflake/snowpark/SnowparkClientException.html "Permalink") class  [ SnowparkClientException  ](SnowparkClientException.html "Represents a Snowpark client side exception.") extends  RuntimeException 

Represents a Snowpark client side exception. 

Represents a Snowpark client side exception. 

Since 
    

0.1.0 

  32. [ __ ](../../../com/snowflake/snowpark/StoredProcedure.html "Permalink") case class  [ StoredProcedure  ](StoredProcedure.html "The reference to a Stored Procedure which can be created by Session.sproc.register methods, and used in Session.storedProcedure method.") extends  Product  with  Serializable 

The reference to a Stored Procedure which can be created by ` Session.sproc.register ` methods, and used in ` Session.storedProcedure ` method. 

The reference to a Stored Procedure which can be created by ` Session.sproc.register ` methods, and used in ` Session.storedProcedure ` method. 

For example: 
[code] val sp = session.sproc.registerTemporary(
           (session: Session, num: Int) => {
             val result = session.sql(s"select $num").collect().head.getInt(0)
             result + 100
           })
         session.storedProcedure(sp, 123).show()
[/code]

Since 
    

1.8.0 

  33. [ __ ](../../../com/snowflake/snowpark/TableFunction.html "Permalink") case class  [ TableFunction  ](TableFunction.html "Looks up table functions by funcName and returns tableFunction object which can be used in DataFrame.join and Session.tableFunction methods.") (  funcName:  String  )  extends  Product  with  Serializable 

Looks up table functions by funcName and returns tableFunction object which can be used in DataFrame.join and Session.tableFunction methods. 

Looks up table functions by funcName and returns tableFunction object which can be used in DataFrame.join and Session.tableFunction methods. 

It can reference both system-defined table function and user-defined table functions. 

Example 
[code] import com.snowflake.snowpark.functions._
         import com.snowflake.snowpark.TableFunction
         
         session.tableFunction(
           TableFunction("flatten"),
           Map("input" -> parse_json(lit("[1,2]")))
         )
         
         df.join(TableFunction("split_to_table"), df("a"), lit(","))
[/code]

funcName 
    

table function name, can be a short name like func or a fully qualified name like database.schema.func 

Since 
    

0.4.0 

  34. [ __ ](../../../com/snowflake/snowpark/TypedAsyncJob.html "Permalink") class  [ TypedAsyncJob  ](TypedAsyncJob.html "Provides a way to track an asynchronously executed action in a DataFrame.") [  T  ]  extends [ AsyncJob ](AsyncJob.html)

Provides a way to track an asynchronously executed action in a DataFrame. 

Provides a way to track an asynchronously executed action in a DataFrame. 

To get the result of the action (e.g. the number of results from a ` count() ` action or an Array of [ Row ](Row.html) objects from the ` collect() ` action), call the [ getResult ](TypedAsyncJob.html#getResult\(maxWaitTimeInSeconds:Int\):T) method. 

To perform an action on a DataFrame asynchronously, call an action method on the [ DataFrameAsyncActor ](DataFrameAsyncActor.html) object returned by [ DataFrame.async ](DataFrame.html#async:com.snowflake.snowpark.DataFrameAsyncActor) . For example: 
[code] val asyncJob1 = df.async.collect()
         val asyncJob2 = df.async.toLocalIterator()
         val asyncJob3 = df.async.count()
[/code]

Each of these methods returns a TypedAsyncJob object that you can use to get the results of the action. 

Since 
    

0.11.0 

  35. [ __ ](../../../com/snowflake/snowpark/UDFRegistration.html "Permalink") class  [ UDFRegistration  ](UDFRegistration.html "Provides methods to register lambdas and functions as UDFs in the Snowflake database.") extends  Logging 

Provides methods to register lambdas and functions as UDFs in the Snowflake database. 

Provides methods to register lambdas and functions as UDFs in the Snowflake database. 

[ Session.udf ](Session.html#udf:com.snowflake.snowpark.UDFRegistration) returns an object of this class. 

You can use this object to register temporary UDFs that you plan to use in the current session: 
[code] session.udf.registerTemporary("mydoubleudf", (x: Int) => x * x)
         session.sql(s"SELECT mydoubleudf(c) from T)
[/code]

You can also register permanent UDFs that you can use in subsequent sessions. When registering a permanent UDF, you must specify a stage where the registration method will upload the JAR files for the UDF and any dependencies. 
[code] session.udf.registerPermanent("mydoubleudf", (x: Int) => x * x, "mystage")
         session.sql(s"SELECT mydoubleudf(c) from T)
[/code]

The methods that register a UDF return a [ UserDefinedFunction ](UserDefinedFunction.html) object, which you can use in [ Column ](Column.html) expressions. 
[code] val myUdf = session.udf.registerTemporary("mydoubleudf", (x: Int) => x * x)
         session.table("T").select(myUdf(col("c")))
[/code]

If you do not need to refer to a UDF by name, use [ com.snowflake.snowpark.functions.udf ](functions$.html#udf\[RT\]\(func:\(\)=>RT\)\(implicitevidence$2:reflect.runtime.universe.TypeTag\[RT\]\):com.snowflake.snowpark.UserDefinedFunction) to create an anonymous UDF instead. 

Snowflake supports the following data types for the parameters for a UDF: 

SQL Type  |  Scala Type  |  Notes   
---|---|---  
NUMBER  |  Short or Option[Short]  |  Supported   
NUMBER  |  Int or Option[Int]  |  Supported   
NUMBER  |  Long or Option[Long]  |  Supported   
FLOAT  |  Float or Option[Float]  |  Supported   
DOUBLE  |  Double or Option[Double]  |  Supported   
NUMBER  |  java.math.BigDecimal  |  Supported   
VARCHAR  |  String or java.lang.String  |  Supported   
BOOL  |  Boolean or Option[Boolean]  |  Supported   
DATE  |  java.sql.Date  |  Supported   
TIMESTAMP  |  java.sql.Timestamp  |  Supported   
BINARY  |  Array[Byte]  |  Supported   
ARRAY  |  Array[String] or Array[Variant]  |  Supported array of type Array[String] or Array[Variant]   
OBJECT  |  Map[String, String] or Map[String, Variant]  |  Supported mutable map of type scala.collection.mutable.Map[String, String] or scala.collection.mutable.Map[String, Variant]   
GEOGRAPHY  |  com.snowflake.snowpark.types.Geography  |  Supported   
VARIANT  |  com.snowflake.snowpark.types.Variant  |  Supported   
  
Since 
    

0.1.0 

  36. [ __ ](../../../com/snowflake/snowpark/UDTFRegistration.html "Permalink") class  [ UDTFRegistration  ](UDTFRegistration.html "Provides methods to register a UDTF \(user-defined table function\) in the Snowflake database.") extends  Logging 

Provides methods to register a UDTF (user-defined table function) in the Snowflake database. 

Provides methods to register a UDTF (user-defined table function) in the Snowflake database. 

[ Session.udtf ](Session.html#udtf:com.snowflake.snowpark.UDTFRegistration) returns an object of this class. 

To register an UDTF, you must: 

     1. Define a UDTF class. 
     2. Create an instance of that class, and register that instance as a UDTF. 

The next sections describe these steps in more detail. 

###  Defining the UDTF Class 

Define a class that inherits from one of the ` UDTF[N] ` classes (e.g. ` UDTF0 ` , ` UDTF1 ` , etc.), where _n_ specifies the number of input arguments for your UDTF. For example, if your UDTF passes in 3 input arguments, extend the ` UDTF3 ` class. 

In your class, override the following three methods: 

     * ` process() ` , which is called once for each row in the input partition. 
     * ` endPartition() ` , which is called once for each partition after all rows have been passed to ` process() ` . 
     * ` outputSchema() ` , which returns a [ types.StructType ](types/StructType.html) object that describes the schema for the returned rows. 

When a UDTF is called, the rows are grouped into partitions before they are passed to the UDTF: 

     * If the statement that calls the UDTF specifies the PARTITION clause (explicit partitions), that clause determines how the rows are partitioned. 
     * If the statement does not specify the PARTITION clause (implicit partitions), Snowflake determines how best to partition the rows. 

For an explanation of partitions, see [ Table Functions and Partitions ](https://docs.snowflake.com/en/developer-guide/udf/java/udf-java-tabular-functions.html#label-udf-java-partitions)

####  Defining the process() Method 

This method is invoked once for each row in the input partition. 

The arguments passed to the registered UDTF are passed to ` process() ` . For each argument passed to the UDTF, you must have a corresponding argument in the signature of the ` process() ` method. Make sure that the type of the argument in the ` process() ` method matches the Snowflake data type of the corresponding argument in the UDTF. 

Snowflake supports the following data types for the parameters for a UDTF: 

SQL Type  |  Scala Type  |  Notes   
---|---|---  
NUMBER  |  Short or Option[Short]  |  Supported   
NUMBER  |  Int or Option[Int]  |  Supported   
NUMBER  |  Long or Option[Long]  |  Supported   
FLOAT  |  Float or Option[Float]  |  Supported   
DOUBLE  |  Double or Option[Double]  |  Supported   
NUMBER  |  java.math.BigDecimal  |  Supported   
VARCHAR  |  String or java.lang.String  |  Supported   
BOOL  |  Boolean or Option[Boolean]  |  Supported   
DATE  |  java.sql.Date  |  Supported   
TIMESTAMP  |  java.sql.Timestamp  |  Supported   
BINARY  |  Array[Byte]  |  Supported   
ARRAY  |  Array[String] or Array[Variant]  |  Supported array of type Array[String] or Array[Variant]   
OBJECT  |  Map[String, String] or Map[String, Variant]  |  Supported mutable map of type scala.collection.mutable.Map[String, String] or scala.collection.mutable.Map[String, Variant]   
VARIANT  |  com.snowflake.snowpark.types.Variant  |  Supported   
  
####  Defining the endPartition() Method 

This method is invoked once for each partition, after all rows in that partition have been passed to the ` process() ` method. 

You can use this method to generate output rows, based on any state information that you aggregate in the ` process() ` method. 

####  Defining the outputSchema() Method 

In this method, define the output schema for the rows returned by the ` process() ` and ` endPartition() ` methods. 

Construct and return a [ types.StructType ](types/StructType.html) object that uses an Array of [ types.StructField ](types/StructField.html) objects to specify the Snowflake data type of each field in a returned row. 

Snowflake supports the following DataTypes for the output schema for a UDTF: 

DataType  |  SQL Type  |  Notes   
---|---|---  
BooleanType  |  Boolean  |  Supported   
ShortType  |  NUMBER  |  Supported   
IntegerType  |  NUMBER  |  Supported   
LongType  |  NUMBER  |  Supported   
DecimalType  |  NUMBER  |  Supported   
FloatType  |  FLOAT  |  Supported   
DoubleType  |  DOUBLE  |  Supported   
StringType  |  VARCHAR  |  Supported   
BinaryType  |  BINARY  |  Supported   
TimeType  |  TIME  |  Supported   
DateType  |  DATE  |  Supported   
TimestampType  |  TIMESTAMP  |  Supported   
VariantType  |  VARIANT  |  Supported   
ArrayType(StringType)  |  ARRAY  |  Supported   
ArrayType(VariantType)  |  ARRAY  |  Supported   
MapType(StringType, StringType)  |  OBJECT  |  Supported   
MapType(StringType, VariantType)  |  OBJECT  |  Supported   
  
####  Example of a UDTF Class 

The following is an example of a UDTF class that generates a range of rows. 

The UDTF passes in 2 arguments, so the class extends ` UDTF2 ` . 

The arguments ` start ` and ` count ` specify the starting number for the row and the number of rows to generate. 
[code] class MyRangeUdtf extends UDTF2[Int, Int] {
        override def process(start: Int, count: Int): Iterable[Row] =
            (start until (start + count)).map(Row(_))
        override def endPartition(): Iterable[Row] = Array.empty[Row]
        override def outputSchema(): StructType = StructType(StructField("C1", IntegerType))
    }
[/code]

###  Registering the UDTF 

Next, create an instance of the new class, and register the class by calling one of the [ UDTFRegistration ](UDTFRegistration.html) methods. You can register a temporary or permanent UDTF by name. If you don't need to call the UDTF by name, you can register an anonymous UDTF. 

####  Registering a Temporary UDTF By Name 

To register a temporary UDTF by name, call ` registerTemporary ` , passing in a name for the UDTF and an instance of the UDTF class. For example: 
[code] // Use the MyRangeUdtf defined in previous example.
    val tableFunction = session.udtf.registerTemporary("myUdtf", new MyRangeUdtf())
    session.tableFunction(tableFunction, lit(10), lit(5)).show
[/code]

####  Registering a Permanent UDTF By Name 

If you need to use the UDTF in subsequent sessions, register a permanent UDTF. 

When registering a permanent UDTF, you must specify a stage where the registration method will upload the JAR files for the UDTF and its dependencies. For example: 
[code] val tableFunction = session.udtf.registerPermanent("myUdtf", new MyRangeUdtf(), "@myStage")
    session.tableFunction(tableFunction, lit(10), lit(5)).show
[/code]

####  Registering an Anonymous Temporary UDTF 

If you do not need to refer to a UDTF by name, use [ UDTF) ](UDTFRegistration.html#registerTemporary\(udtf:com.snowflake.snowpark.udtf.UDTF\):com.snowflake.snowpark.TableFunction) to create an anonymous UDTF instead. 

####  Calling a UDTF 

The methods that register a UDTF return a [ TableFunction ](TableFunction.html) object, which you can use in [ Session.tableFunction ](Session.html#tableFunction\(func:com.snowflake.snowpark.Column\):com.snowflake.snowpark.DataFrame) . 
[code] val tableFunction = session.udtf.registerTemporary("myUdtf", new MyRangeUdtf())
    session.tableFunction(tableFunction, lit(10), lit(5)).show
[/code]

Since 
    

1.2.0 

  37. [ __ ](../../../com/snowflake/snowpark/Updatable.html "Permalink") class  [ Updatable  ](Updatable.html "Represents a lazily-evaluated Updatable.") extends [ DataFrame ](DataFrame.html)

Represents a lazily-evaluated Updatable. 

Represents a lazily-evaluated Updatable. It extends [ DataFrame ](DataFrame.html) so all [ DataFrame ](DataFrame.html) operations can be applied on it. 

**Creating an Updatable**

You can create an Updatable by calling [ session.table ](Session.html#table\(name:String\):com.snowflake.snowpark.Updatable) with the name of the Updatable. 

Example 1: Creating a Updatable by reading a table. 
[code] val dfPrices = session.table("itemsdb.publicschema.prices")
[/code]

Since 
    

0.7.0 

  38. [ __ ](../../../com/snowflake/snowpark/UpdatableAsyncActor.html "Permalink") class  [ UpdatableAsyncActor  ](UpdatableAsyncActor.html "Provides APIs to execute Updatable actions asynchronously.") extends [ DataFrameAsyncActor ](DataFrameAsyncActor.html)

Provides APIs to execute Updatable actions asynchronously. 

Provides APIs to execute Updatable actions asynchronously. 

Since 
    

0.11.0 

  39. [ __ ](../../../com/snowflake/snowpark/UpdateResult.html "Permalink") case class  [ UpdateResult  ](UpdateResult.html "Result of updating rows in an Updatable") (  rowsUpdated:  Long  ,  multiJoinedRowsUpdated:  Long  )  extends  Product  with  Serializable 

Result of updating rows in an Updatable 

Result of updating rows in an Updatable 

Since 
    

0.7.0 

  40. [ __ ](../../../com/snowflake/snowpark/UserDefinedFunction.html "Permalink") case class  [ UserDefinedFunction  ](UserDefinedFunction.html "Encapsulates a user defined lambda or function that is returned by UDFRegistration.registerTemporary or by com.snowflake.snowpark.functions.udf.") extends  Product  with  Serializable 

Encapsulates a user defined lambda or function that is returned by [ UDFRegistration.registerTemporary ](UDFRegistration.html#registerTemporary\[RT\]\(name:String,func:\(\)=>RT\)\(implicitevidence$277:reflect.runtime.universe.TypeTag\[RT\]\):com.snowflake.snowpark.UserDefinedFunction) or by [ com.snowflake.snowpark.functions.udf ](functions$.html#udf\[RT\]\(func:\(\)=>RT\)\(implicitevidence$2:reflect.runtime.universe.TypeTag\[RT\]\):com.snowflake.snowpark.UserDefinedFunction) . 

Encapsulates a user defined lambda or function that is returned by [ UDFRegistration.registerTemporary ](UDFRegistration.html#registerTemporary\[RT\]\(name:String,func:\(\)=>RT\)\(implicitevidence$277:reflect.runtime.universe.TypeTag\[RT\]\):com.snowflake.snowpark.UserDefinedFunction) or by [ com.snowflake.snowpark.functions.udf ](functions$.html#udf\[RT\]\(func:\(\)=>RT\)\(implicitevidence$2:reflect.runtime.universe.TypeTag\[RT\]\):com.snowflake.snowpark.UserDefinedFunction) . 

Use [ UserDefinedFunction.apply ](UserDefinedFunction.html#apply\(exprs:com.snowflake.snowpark.Column*\):com.snowflake.snowpark.Column) to generate [ Column ](Column.html) expressions from an instance. 
[code] import com.snowflake.snowpark.functions._
         val myUdf = udf((x: Int, y: String) => y + x)
         df.select(myUdf(col("i"), col("s")))
[/code]

Since 
    

0.1.0 

  41. [ __ ](../../../com/snowflake/snowpark/WindowSpec.html "Permalink") class  [ WindowSpec  ](WindowSpec.html "Represents a window frame clause.") extends  AnyRef 

Represents a window frame clause. 

Represents a window frame clause. 

Since 
    

0.1.0 

  42. [ __ ](../../../com/snowflake/snowpark/WriteFileResult.html "Permalink") case class  [ WriteFileResult  ](WriteFileResult.html "Represents the results of writing data from a DataFrame to a file in a stage.") (  rows:  Array  [ [ Row ](Row.html) ]  ,  schema: [ StructType ](types/StructType.html) )  extends  Product  with  Serializable 

Represents the results of writing data from a DataFrame to a file in a stage. 

Represents the results of writing data from a DataFrame to a file in a stage. 

To write the data, the DataFrameWriter effectively executes the ` COPY INTO <location> ` command. WriteFileResult encapsulates the output returned by the command: 

     * ` rows ` represents the rows of output from the command. 
     * ` schema ` defines the schema for these rows. 

For example, if the DETAILED_OUTPUT option is TRUE, each row contains a ` file_name ` , ` file_size ` , and ` row_count ` field. ` schema ` defines the names and types of these fields. If the DETAILED_OUTPUT option is not specified (meaning that the option is FALSE), each row contains a ` rows_unloaded ` , ` input_bytes ` , and ` output_bytes ` field. 

rows 
    

The output rows produced by the ` COPY INTO <location> ` command. 

schema 
    

The names and types of the fields in the output rows. 

Since 
    

1.5.0 




###  Value Members 

  1. [ __ ](../../../com/snowflake/snowpark/GroupingSets$.html "Permalink") object  [ GroupingSets  ](GroupingSets$.html "Constructors of GroupingSets object.") extends  Serializable 

Constructors of GroupingSets object. 

Constructors of GroupingSets object. 

Since 
    

0.4.0 

  2. [ __ ](../../../com/snowflake/snowpark/Row$.html "Permalink") object  [ Row  ](Row$.html) extends  Serializable 

Since 
    

0.1.0 

  3. [ __ ](../../../com/snowflake/snowpark/SaveMode$.html "Permalink") object  [ SaveMode  ](SaveMode$.html "SaveMode configures the behavior when data is written from a DataFrame to a data source using a DataFrameWriter instance.")

SaveMode configures the behavior when data is written from a DataFrame to a data source using a [ DataFrameWriter ](DataFrameWriter.html) instance. 

SaveMode configures the behavior when data is written from a DataFrame to a data source using a [ DataFrameWriter ](DataFrameWriter.html) instance. 

Since 
    

0.1.0 

  4. [ __ ](../../../com/snowflake/snowpark/Session$.html "Permalink") object  [ Session  ](Session$.html "Companion object to Session that you use to build and create a session.") extends  Logging 

Companion object to [ Session ](Session.html) that you use to build and create a session. 

Companion object to [ Session ](Session.html) that you use to build and create a session. 

Since 
    

0.1.0 

  5. [ __ ](../../../com/snowflake/snowpark/Window$.html "Permalink") object  [ Window  ](Window$.html "Contains functions to form WindowSpec.")

Contains functions to form [ WindowSpec ](WindowSpec.html) . 

Contains functions to form [ WindowSpec ](WindowSpec.html) . 

Since 
    

0.1.0 

  6. [ __ ](../../../com/snowflake/snowpark/functions$.html "Permalink") object  [ functions  ](functions$.html "Provides utility functions that generate Column expressions that you can pass to DataFrame transformation methods.")

Provides utility functions that generate [ Column ](Column.html) expressions that you can pass to [ DataFrame ](DataFrame.html) transformation methods. 

Provides utility functions that generate [ Column ](Column.html) expressions that you can pass to [ DataFrame ](DataFrame.html) transformation methods. These functions generate references to columns, literals, and SQL expressions (e.g. "c + 1"). 

This object also provides functions that correspond to Snowflake [ system-defined functions ](https://docs.snowflake.com/en/sql-reference-functions.html) (built-in functions), including functions for aggregation and window functions. 

The following examples demonstrate the use of some of these functions: 
[code] // Use columns and literals in expressions.
         df.select(col("c") + lit(1))
         
         // Call system-defined (built-in) functions.
         // This example calls the function that corresponds to the ADD_MONTHS() SQL function.
         df.select(add_months(col("d"), lit(3)))
         
         // Call system-defined functions that have no corresponding function in the functions object.
         // This example calls the RADIANS() SQL function, passing in values from the column "e".
         df.select(callBuiltin("radians", col("e")))
         
         // Call a user-defined function (UDF) by name.
         df.select(callUDF("some_func", col("c")))
         
         // Register and call an anonymous UDF.
         val myudf = udf((x:Int) => x + x)
         df.select(myudf(col("c")))
         
         // Evaluate an SQL expression
         df.select(sqlExpr("c + 1"))
[/code]

For functions that accept scala types, e.g. callUdf, callBuiltin, lit(), the mapping from scala types to Snowflake types is as follows: 
[code] String => String
         Byte => TinyInt
         Int => Int
         Short => SmallInt
         Long => BigInt
         Float => Float
         Double => Double
         Decimal => Number
         Boolean => Boolean
         Array => Array
         Timestamp => Timestamp
         Date => Date
[/code]

Since 
    

0.1.0 

  7. [ __ ](../../../com/snowflake/snowpark/tableFunctions$.html "Permalink") object  [ tableFunctions  ](tableFunctions$.html "Provides utility functions that generate table function expressions that can be passed to DataFrame join method and Session tableFunction method.")

Provides utility functions that generate table function expressions that can be passed to DataFrame join method and Session tableFunction method. 

Provides utility functions that generate table function expressions that can be passed to DataFrame join method and Session tableFunction method. 

This object also provides functions that correspond to Snowflake [ system-defined table functions ](https://docs.snowflake.com/en/sql-reference/functions-table.html) . 

The following examples demonstrate the use of some of these functions: 
[code] import com.snowflake.snowpark.functions.parse_json
         
         // Creates DataFrame from Session.tableFunction
         session.tableFunction(tableFunctions.flatten, Map("input" -> parse_json(lit("[1,2]"))))
         session.tableFunction(tableFunctions.split_to_table, "split by space", " ")
         
         // DataFrame joins table function
         df.join(tableFunctions.flatten, Map("input" -> parse_json(df("a"))))
         df.join(tableFunctions.split_to_table, df("a"), ",")
         
         // Invokes any table function including user-defined table function
          df.join(tableFunctions.tableFunction("flatten"), Map("input" -> parse_json(df("a"))))
          session.tableFunction(tableFunctions.tableFunction("split_to_table"), "split by space", " ")
[/code]

Since 
    

0.4.0 




###  Ungrouped 

© 2025 Snowflake Inc. All Rights Reserved
