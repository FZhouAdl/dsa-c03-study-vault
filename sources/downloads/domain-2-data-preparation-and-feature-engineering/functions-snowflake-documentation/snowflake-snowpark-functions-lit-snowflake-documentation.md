---
title: "snowflake.snowpark.functions.lit | Snowflake Documentation"
source: https://docs.snowflake.com/en/developer-guide/snowpark/reference/python/latest/snowpark/api/snowflake.snowpark.functions.lit.html
cert_domain: domain-2-data-preparation-and-feature-engineering
crawl_depth: 1
crawled: 2026-08-23
---

# snowflake.snowpark.functions.lit¶

snowflake.snowpark.functions.lit(_literal : Union[[Column](snowflake.snowpark.Column.html#snowflake.snowpark.Column "snowflake.snowpark.column.Column"), None, bool, int, float, str, bytearray, Decimal, date, datetime, time, bytes, NaTType, float64, list, tuple, dict]_, _datatype : Optional[[DataType](snowflake.snowpark.types.DataType.html#snowflake.snowpark.types.DataType "snowflake.snowpark.types.DataType")] = None_) → [Column](snowflake.snowpark.Column.html#snowflake.snowpark.Column "snowflake.snowpark.column.Column")[[source]](https://github.com/snowflakedb/snowpark-python/blob/v1.54.0/src/snowflake/snowpark/functions.py#L367-L396)¶
    

Creates a [`Column`](snowflake.snowpark.Column.html#snowflake.snowpark.Column "snowflake.snowpark.Column") expression for a literal value. It supports basic Python data types, including `int`, `float`, `str`, `bool`, `bytes`, `bytearray`, `datetime.time`, `datetime.date`, `datetime.datetime` and `decimal.Decimal`. Also, it supports Python structured data types, including `list`, `tuple` and `dict`, but this container must be JSON serializable. If a `Column` object is passed, it is returned as is.

Example:
[code] 
    >>> import datetime
    >>> columns = [lit(1), lit("1"), lit(1.0), lit(True), lit(b'snow'), lit(datetime.date(2023, 2, 2)), lit([1, 2]), lit({"snow": "flake"}), lit(lit(1))]
    >>> session.create_dataframe([[]]).select([c.as_(str(i)) for i, c in enumerate(columns)]).show()
    ---------------------------------------------------------------------------------------------
    |"0"  |"1"  |"2"  |"3"   |"4"                 |"5"         |"6"   |"7"                |"8"  |
    ---------------------------------------------------------------------------------------------
    |1    |1    |1.0  |True  |bytearray(b'snow')  |2023-02-02  |[     |{                  |1    |
    |     |     |     |      |                    |            |  1,  |  "snow": "flake"  |     |
    |     |     |     |      |                    |            |  2   |}                  |     |
    |     |     |     |      |                    |            |]     |                   |     |
    ---------------------------------------------------------------------------------------------
    
[/code]
