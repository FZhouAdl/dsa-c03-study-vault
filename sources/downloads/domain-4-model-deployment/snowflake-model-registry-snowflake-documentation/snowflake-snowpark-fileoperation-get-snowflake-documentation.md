---
title: "snowflake.snowpark.FileOperation.get | Snowflake Documentation"
source: https://docs.snowflake.com/en/developer-guide/snowpark/reference/python/latest/api/snowflake.snowpark.FileOperation.get
cert_domain: domain-4-model-deployment
crawl_depth: 1
crawled: 2026-08-23
---

# snowflake.snowpark.FileOperation.get¶

FileOperation.get(_stage_location : str_, _target_directory : str_, _*_ , _parallel : int = 10_, _pattern : Optional[str] = None_, _statement_params : Optional[Dict[str, str]] = None_) → List[[GetResult](snowflake.snowpark.GetResult.html#snowflake.snowpark.GetResult "snowflake.snowpark.file_operation.GetResult")][[source]](https://github.com/snowflakedb/snowpark-python/blob/v1.54.0/src/snowflake/snowpark/file_operation.py#L153-L231)¶
    

Downloads the specified files from a path in a stage to a local directory.

References: [Snowflake GET command](https://docs.snowflake.com/en/sql-reference/sql/get.html).

Examples
[code] 
    >>> # Create a temp stage.
    >>> _ = session.sql("create or replace temp stage mystage").collect()
    >>> # Upload a file to a stage.
    >>> _ = session.file.put("tests/resources/t*.csv", "@mystage/prefix1")
    >>> # Download one file from a stage.
    >>> get_result1 = session.file.get("@myStage/prefix1/test2CSV.csv", "tests/downloaded/target1")
    >>> assert len(get_result1) == 1
    >>> # Download all the files from @myStage/prefix.
    >>> get_result2 = session.file.get("@myStage/prefix1", "tests/downloaded/target2")
    >>> assert len(get_result2) > 1
    >>> # Download files with names that match a regular expression pattern.
    >>> get_result3 = session.file.get("@myStage/prefix1", "tests/downloaded/target3", pattern=".*test.*.csv.gz")
    >>> assert len(get_result3) > 1
    
[/code]

Parameters:
    

  * **stage_location** – A directory or filename on a stage, from which you want to download the files.

  * **target_directory** – The path to the local directory where the files should be downloaded. If `target_directory` does not already exist, the method creates the directory.

  * **parallel** – Specifies the number of threads to use for downloading the files. The granularity unit for downloading is one file. Increasing the number of threads might improve performance when downloading large files. Supported values: Any integer value from 1 (no parallelism) to 99 (use 99 threads for downloading files).

  * **pattern** – Specifies a regular expression pattern for filtering files to download. The command lists all files in the specified path and applies the regular expression pattern on each of the files found. Default: `None` (all files in the specified stage are downloaded).

  * **statement_params** – Dictionary of statement level parameters to be set while executing this action.



Returns:
    

A `list` of [`GetResult`](snowflake.snowpark.GetResult.html#snowflake.snowpark.GetResult "snowflake.snowpark.GetResult") instances, each of which represents the result of a downloaded file.
