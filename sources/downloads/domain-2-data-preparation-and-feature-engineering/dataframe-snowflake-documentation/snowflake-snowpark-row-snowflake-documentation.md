---
title: "snowflake.snowpark.Row | Snowflake Documentation"
source: https://docs.snowflake.com/en/developer-guide/snowpark/reference/python/latest/snowpark/api/snowflake.snowpark.Row.html
cert_domain: domain-2-data-preparation-and-feature-engineering
crawl_depth: 1
crawled: 2026-08-23
---

# snowflake.snowpark.Row¶

_class _snowflake.snowpark.Row(_* values: Any_, _** named_values: Any_)[[source]](https://github.com/snowflakedb/snowpark-python/blob/v1.54.0/src/snowflake/snowpark/row.py#L34-L312)¶
    

Bases: `tuple`

Represents a row in [`DataFrame`](snowflake.snowpark.DataFrame.html#snowflake.snowpark.DataFrame "snowflake.snowpark.DataFrame").

It is immutable and works like a tuple or a named tuple.
[code] 
    >>> row = Row(1, 2)
    >>> row
    Row(1, 2)
    >>> row[0]
    1
    >>> len(row)
    2
    >>> row[0:1]
    Row(1)
    >>> named_row = Row(name1=1, name2=2)
    >>> named_row
    Row(name1=1, name2=2)
    >>> named_row["name1"]
    1
    >>> named_row.name1
    1
    >>> row == named_row
    True
    
[/code]

A `Row` object is callable. You can use it to create other `Row` objects:
[code] 
    >>> Employee = Row("name", "salary")
    >>> emp1 = Employee("John", 10000)
    >>> emp1
    Row(name='John', salary=10000)
    >>> emp2 = Employee("James", 20000)
    >>> emp2
    Row(name='James', salary=20000)
    
[/code]

Methods

[`asDict`](snowflake.snowpark.Row.asDict.html#snowflake.snowpark.Row.asDict "snowflake.snowpark.Row.asDict")([recursive]) | Convert to a dict if this row object has both keys and values.  
---|---  
[`as_dict`](snowflake.snowpark.Row.as_dict.html#snowflake.snowpark.Row.as_dict "snowflake.snowpark.Row.as_dict")([recursive]) | Convert to a dict if this row object has both keys and values.
