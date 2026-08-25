---
title: "snowflake.snowpark.Column | Snowflake Documentation"
source: https://docs.snowflake.com/en/developer-guide/snowpark/reference/python/latest/snowpark/api/snowflake.snowpark.Column.html
cert_domain: domain-2-data-preparation-and-feature-engineering
crawl_depth: 1
crawled: 2026-08-23
---

# snowflake.snowpark.Column¶

_class _snowflake.snowpark.Column(_expr1 : Union[str, Expression]_, _expr2 : Optional[str] = None_, __caller_name : Optional[str] = 'Column'_, _*_ , __is_qualified_name : bool = False_)[[source]](https://github.com/snowflakedb/snowpark-python/blob/v1.54.0/src/snowflake/snowpark/column.py#L153-L1514)¶
    

Bases: `object`

Represents a column or an expression in a [`DataFrame`](snowflake.snowpark.DataFrame.html#snowflake.snowpark.DataFrame "snowflake.snowpark.DataFrame").

To access a Column object that refers a column in a [`DataFrame`](snowflake.snowpark.DataFrame.html#snowflake.snowpark.DataFrame "snowflake.snowpark.DataFrame"), you can:

>   * Use the column name.
> 
>   * Use the [`functions.col()`](snowflake.snowpark.functions.col.html#snowflake.snowpark.functions.col "snowflake.snowpark.functions.col") function.
> 
>   * Use the [`DataFrame.col()`](snowflake.snowpark.DataFrame.col.html#snowflake.snowpark.DataFrame.col "snowflake.snowpark.DataFrame.col") method.
> 
>   * Use the index operator `[]` on a dataframe object with a column name.
> 
>   * Use the dot operator `.` on a dataframe object with a column name.
> 
> 

[code] 
>     >>> from snowflake.snowpark.functions import col
>     >>> df = session.create_dataframe([["John", 1], ["Mike", 11]], schema=["name", "age"])
>     >>> df.select("name").collect()
>     [Row(NAME='John'), Row(NAME='Mike')]
>     >>> df.select(col("name")).collect()
>     [Row(NAME='John'), Row(NAME='Mike')]
>     >>> df.select(df.col("name")).collect()
>     [Row(NAME='John'), Row(NAME='Mike')]
>     >>> df.select(df["name"]).collect()
>     [Row(NAME='John'), Row(NAME='Mike')]
>     >>> df.select(df.name).collect()
>     [Row(NAME='John'), Row(NAME='Mike')]
>     
[/code]
> 
> Snowflake object identifiers, including column names, may or may not be case sensitive depending on a set of rules. Refer to [Snowflake Object Identifier Requirements](https://docs.snowflake.com/en/sql-reference/identifiers-syntax.html) for details. When you use column names with a DataFrame, you should follow these rules.
> 
> The returned column names after a DataFrame is evaluated follow the case-sensitivity rules too. The above `df` was created with column name “name” while the returned column name after `collect()` was called became “NAME”. It’s because the column is regarded as ignore-case so the Snowflake database returns the upper case.

To create a Column object that represents a constant value, use [`snowflake.snowpark.functions.lit()`](snowflake.snowpark.functions.lit.html#snowflake.snowpark.functions.lit "snowflake.snowpark.functions.lit"):
[code] 
    >>> from snowflake.snowpark.functions import lit
    >>> df.select(col("name"), lit("const value").alias("literal_column")).collect()
    [Row(NAME='John', LITERAL_COLUMN='const value'), Row(NAME='Mike', LITERAL_COLUMN='const value')]
    
[/code]

This class also defines utility functions for constructing expressions with Columns. Column objects can be built with the operators, summarized by operator precedence, in the following table:

Operator | Description  
---|---  
`x[index]` | Index operator to get an item out of a Snowflake ARRAY or OBJECT  
`**` | Power  
`-x`, `~x` | Unary minus, unary not  
`*`, `/`, `%` | Multiply, divide, remainder  
`+`, `-` | Plus, minus  
`&` | And  
`|` | Or  
`==`, `!=`, `<`, `<=`, `>`, `>=` | Equal to, not equal to, less than, less than or equal to, greater than, greater than or equal to  
  
> The following examples demonstrate how to use Column objects in expressions:
[code] 
>     >>> df = session.create_dataframe([[20, 5], [1, 2]], schema=["a", "b"])
>     >>> df.filter((col("a") == 20) | (col("b") <= 10)).collect()  # use parentheses before and after the | operator.
>     [Row(A=20, B=5), Row(A=1, B=2)]
>     >>> df.filter((df["a"] + df.b) < 10).collect()
>     [Row(A=1, B=2)]
>     >>> df.select((col("b") * 10).alias("c")).collect()
>     [Row(C=50), Row(C=20)]
>     
[/code]
> 
> When you use `|`, `&`, and `~` as logical operators on columns, you must always enclose column expressions with parentheses as illustrated in the above example, because their order precedence is higher than `==`, `<`, etc.
> 
> Do not use `and`, `or`, and `not` logical operators on column objects, for instance, `(df.col1 > 1) and (df.col2 > 2)` is wrong. The reason is Python doesn’t have a magic method, or dunder method for them. It will raise an error and tell you to use `|`, `&` or `~`, for which Python has magic methods. A side effect is `if column:` will raise an error because it has a hidden call to `bool(a_column)`, like using the `and` operator. Use `if a_column is None:` instead.

To access elements of a semi-structured Object and Array, use `[]` on a Column object:
[code] 
    >>> from snowflake.snowpark.types import StringType, IntegerType
    >>> df_with_semi_data = session.create_dataframe([[{"k1": "v1", "k2": "v2"}, ["a0", 1, "a2"]]], schema=["object_column", "array_column"])
    >>> df_with_semi_data.select(df_with_semi_data["object_column"]["k1"].alias("k1_value"), df_with_semi_data["array_column"][0].alias("a0_value"), df_with_semi_data["array_column"][1].alias("a1_value")).collect()
    [Row(K1_VALUE='"v1"', A0_VALUE='"a0"', A1_VALUE='1')]
    >>> # The above two returned string columns have JSON literal values because children of semi-structured data are semi-structured.
    >>> # The next line converts JSON literal to a string
    >>> df_with_semi_data.select(df_with_semi_data["object_column"]["k1"].cast(StringType()).alias("k1_value"), df_with_semi_data["array_column"][0].cast(StringType()).alias("a0_value"), df_with_semi_data["array_column"][1].cast(IntegerType()).alias("a1_value")).collect()
    [Row(K1_VALUE='v1', A0_VALUE='a0', A1_VALUE=1)]
    
[/code]

This class has methods for the most frequently used column transformations and operators. Module `snowflake.snowpark.functions` defines many functions to transform columns.

Methods

[`alias`](snowflake.snowpark.Column.alias.html#snowflake.snowpark.Column.alias "snowflake.snowpark.Column.alias")(alias) | Returns a new renamed Column.  
---|---  
[`as_`](snowflake.snowpark.Column.as_.html#snowflake.snowpark.Column.as_ "snowflake.snowpark.Column.as_")(alias) | Returns a new renamed Column.  
[`asc`](snowflake.snowpark.Column.asc.html#snowflake.snowpark.Column.asc "snowflake.snowpark.Column.asc")() | Returns a Column expression with values sorted in ascending order.  
[`asc_nulls_first`](snowflake.snowpark.Column.asc_nulls_first.html#snowflake.snowpark.Column.asc_nulls_first "snowflake.snowpark.Column.asc_nulls_first")() | Returns a Column expression with values sorted in ascending order (null values sorted before non-null values).  
[`asc_nulls_last`](snowflake.snowpark.Column.asc_nulls_last.html#snowflake.snowpark.Column.asc_nulls_last "snowflake.snowpark.Column.asc_nulls_last")() | Returns a Column expression with values sorted in ascending order (null values sorted after non-null values).  
[`astype`](snowflake.snowpark.Column.astype.html#snowflake.snowpark.Column.astype "snowflake.snowpark.Column.astype")(to[, rename_fields, add_fields]) | Casts the value of the Column to the specified data type.  
[`between`](snowflake.snowpark.Column.between.html#snowflake.snowpark.Column.between "snowflake.snowpark.Column.between")(lower_bound, upper_bound) | Between lower bound and upper bound.  
[`bitand`](snowflake.snowpark.Column.bitand.html#snowflake.snowpark.Column.bitand "snowflake.snowpark.Column.bitand")(other) | Bitwise and.  
[`bitor`](snowflake.snowpark.Column.bitor.html#snowflake.snowpark.Column.bitor "snowflake.snowpark.Column.bitor")(other) | Bitwise or.  
[`bitwiseAnd`](snowflake.snowpark.Column.bitwiseAnd.html#snowflake.snowpark.Column.bitwiseAnd "snowflake.snowpark.Column.bitwiseAnd")(other) | Bitwise and.  
[`bitwiseOR`](snowflake.snowpark.Column.bitwiseOR.html#snowflake.snowpark.Column.bitwiseOR "snowflake.snowpark.Column.bitwiseOR")(other) | Bitwise or.  
[`bitwiseXOR`](snowflake.snowpark.Column.bitwiseXOR.html#snowflake.snowpark.Column.bitwiseXOR "snowflake.snowpark.Column.bitwiseXOR")(other) | Bitwise xor.  
[`bitxor`](snowflake.snowpark.Column.bitxor.html#snowflake.snowpark.Column.bitxor "snowflake.snowpark.Column.bitxor")(other) | Bitwise xor.  
[`cast`](snowflake.snowpark.Column.cast.html#snowflake.snowpark.Column.cast "snowflake.snowpark.Column.cast")(to[, rename_fields, add_fields]) | Casts the value of the Column to the specified data type.  
[`collate`](snowflake.snowpark.Column.collate.html#snowflake.snowpark.Column.collate "snowflake.snowpark.Column.collate")(collation_spec) | Returns a copy of the original `Column` with the specified `collation_spec` property, rather than the original collation specification property.  
`contains`(string) | Returns true if the column contains string for each row.  
[`desc`](snowflake.snowpark.Column.desc.html#snowflake.snowpark.Column.desc "snowflake.snowpark.Column.desc")() | Returns a Column expression with values sorted in descending order.  
[`desc_nulls_first`](snowflake.snowpark.Column.desc_nulls_first.html#snowflake.snowpark.Column.desc_nulls_first "snowflake.snowpark.Column.desc_nulls_first")() | Returns a Column expression with values sorted in descending order (null values sorted before non-null values).  
[`desc_nulls_last`](snowflake.snowpark.Column.desc_nulls_last.html#snowflake.snowpark.Column.desc_nulls_last "snowflake.snowpark.Column.desc_nulls_last")() | Returns a Column expression with values sorted in descending order (null values sorted after non-null values).  
[`endswith`](snowflake.snowpark.Column.endswith.html#snowflake.snowpark.Column.endswith "snowflake.snowpark.Column.endswith")(other) | Returns true if this Column ends with another string.  
[`eqNullSafe`](snowflake.snowpark.Column.eqNullSafe.html#snowflake.snowpark.Column.eqNullSafe "snowflake.snowpark.Column.eqNullSafe")(other) | Equal to.  
[`equal_nan`](snowflake.snowpark.Column.equal_nan.html#snowflake.snowpark.Column.equal_nan "snowflake.snowpark.Column.equal_nan")() | Is NaN.  
[`equal_null`](snowflake.snowpark.Column.equal_null.html#snowflake.snowpark.Column.equal_null "snowflake.snowpark.Column.equal_null")(other) | Equal to.  
`getField`(field) | Accesses an element of ARRAY column by ordinal position, or an element of OBJECT column by key.  
[`getItem`](snowflake.snowpark.Column.getItem.html#snowflake.snowpark.Column.getItem "snowflake.snowpark.Column.getItem")(field) | Accesses an element of ARRAY column by ordinal position, or an element of OBJECT column by key.  
[`getName`](snowflake.snowpark.Column.getName.html#snowflake.snowpark.Column.getName "snowflake.snowpark.Column.getName")() | Returns the column name (if the column has a name).  
[`get_name`](snowflake.snowpark.Column.get_name.html#snowflake.snowpark.Column.get_name "snowflake.snowpark.Column.get_name")() | Returns the column name (if the column has a name).  
[`in_`](snowflake.snowpark.Column.in_.html#snowflake.snowpark.Column.in_ "snowflake.snowpark.Column.in_")(*vals) | Returns a conditional expression that you can pass to the [`DataFrame.filter()`](snowflake.snowpark.DataFrame.filter.html#snowflake.snowpark.DataFrame.filter "snowflake.snowpark.DataFrame.filter") or where [`DataFrame.where()`](snowflake.snowpark.DataFrame.where.html#snowflake.snowpark.DataFrame.where "snowflake.snowpark.DataFrame.where") to perform the equivalent of a WHERE .  
[`isNotNull`](snowflake.snowpark.Column.isNotNull.html#snowflake.snowpark.Column.isNotNull "snowflake.snowpark.Column.isNotNull")() | Is not null.  
[`isNull`](snowflake.snowpark.Column.isNull.html#snowflake.snowpark.Column.isNull "snowflake.snowpark.Column.isNull")() | Is null.  
[`is_not_null`](snowflake.snowpark.Column.is_not_null.html#snowflake.snowpark.Column.is_not_null "snowflake.snowpark.Column.is_not_null")() | Is not null.  
[`is_null`](snowflake.snowpark.Column.is_null.html#snowflake.snowpark.Column.is_null "snowflake.snowpark.Column.is_null")() | Is null.  
[`isin`](snowflake.snowpark.Column.isin.html#snowflake.snowpark.Column.isin "snowflake.snowpark.Column.isin")(*vals) | Returns a conditional expression that you can pass to the [`DataFrame.filter()`](snowflake.snowpark.DataFrame.filter.html#snowflake.snowpark.DataFrame.filter "snowflake.snowpark.DataFrame.filter") or where [`DataFrame.where()`](snowflake.snowpark.DataFrame.where.html#snowflake.snowpark.DataFrame.where "snowflake.snowpark.DataFrame.where") to perform the equivalent of a WHERE .  
[`like`](snowflake.snowpark.Column.like.html#snowflake.snowpark.Column.like "snowflake.snowpark.Column.like")(pattern) | Allows case-sensitive matching of strings based on comparison with a pattern.  
[`name`](snowflake.snowpark.Column.name.html#snowflake.snowpark.Column.name "snowflake.snowpark.Column.name")(alias[, variant]) | Returns a new renamed Column.  
[`over`](snowflake.snowpark.Column.over.html#snowflake.snowpark.Column.over "snowflake.snowpark.Column.over")([window]) | Returns a window frame, based on the specified `WindowSpec`.  
[`regexp`](snowflake.snowpark.Column.regexp.html#snowflake.snowpark.Column.regexp "snowflake.snowpark.Column.regexp")(pattern[, parameters]) | Returns true if this Column matches the specified regular expression.  
[`rlike`](snowflake.snowpark.Column.rlike.html#snowflake.snowpark.Column.rlike "snowflake.snowpark.Column.rlike")(pattern[, parameters]) | Returns true if this Column matches the specified regular expression.  
[`startswith`](snowflake.snowpark.Column.startswith.html#snowflake.snowpark.Column.startswith "snowflake.snowpark.Column.startswith")(other) | Returns true if this Column starts with another string.  
[`substr`](snowflake.snowpark.Column.substr.html#snowflake.snowpark.Column.substr "snowflake.snowpark.Column.substr")(start_pos, length) | Returns a substring of this string column.  
[`substring`](snowflake.snowpark.Column.substring.html#snowflake.snowpark.Column.substring "snowflake.snowpark.Column.substring")(start_pos, length) | Returns a substring of this string column.  
[`try_cast`](snowflake.snowpark.Column.try_cast.html#snowflake.snowpark.Column.try_cast "snowflake.snowpark.Column.try_cast")(to[, rename_fields, add_fields, ...]) | Tries to cast the value of the Column to the specified data type.  
[`within_group`](snowflake.snowpark.Column.within_group.html#snowflake.snowpark.Column.within_group "snowflake.snowpark.Column.within_group")(*cols) | Returns a Column expression that adds a WITHIN GROUP clause to sort the rows by the specified columns.
