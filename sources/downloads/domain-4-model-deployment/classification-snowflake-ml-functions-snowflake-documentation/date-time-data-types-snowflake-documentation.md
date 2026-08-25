---
title: "Date & time data types | Snowflake Documentation"
source: https://docs.snowflake.com/sql-reference/data-types-datetime
cert_domain: domain-4-model-deployment
crawl_depth: 1
crawled: 2026-08-23
---

# Date & time data types¶

Snowflake supports data types for managing dates, times, and timestamps (combined date + time). Snowflake also supports formats for string constants used in manipulating dates, times, and timestamps.

## Data types¶

Snowflake supports the following date and time data types:

  * DATE
  * DATETIME
  * Interval data types
  * TIME
  * TIMESTAMP_LTZ , TIMESTAMP_NTZ , TIMESTAMP_TZ



Note

For DATE and TIMESTAMP data, Snowflake recommends using years between 1582 and 9999. Snowflake accepts some years outside this range, but years prior to 1582 should be avoided due to limitations on the Gregorian Calendar.

### DATE¶

Snowflake supports a single DATE data type for storing dates (with no time elements).

DATE accepts dates in the most common forms (`YYYY-MM-DD`, `DD-MON-YYYY`, and so on).

In addition, all accepted TIMESTAMP values are valid inputs for dates, but the TIME information is truncated.

### DATETIME¶

DATETIME is synonymous with TIMESTAMP_NTZ.

### Interval data types¶

Interval data types store values that represent a duration of time. You can calculate an interval as the difference between two dates or times. An interval only defines a duration, so it doesn’t have a start or end point in time. For example, you might define an interval as three years and seven months.

Snowflake supports the following year-month variations of interval data types:

Data type| Description  
---|---  
INTERVAL YEAR| Represents a duration of time in years.  
INTERVAL YEAR TO MONTH| Represents a duration of time in years and months.  
INTERVAL MONTH| Represents a duration of time in months.  
  
Snowflake supports the following day-time variations of interval data types:

Data type| Description  
---|---  
INTERVAL DAY| Represents a duration of time in days.  
INTERVAL DAY TO HOUR| Represents a duration of time in days and hours.  
INTERVAL DAY TO MINUTE| Represents a duration of time in days, hours, and minutes.  
INTERVAL DAY TO SECOND| Represents a duration of time in days, hours, minutes, seconds, and fractional seconds.  
INTERVAL HOUR| Represents a duration of time in hours.  
INTERVAL HOUR TO MINUTE| Represents a duration of time in hours and minutes.  
INTERVAL HOUR TO SECOND| Represents a duration of time in hours, minutes, seconds, and fractional seconds.  
INTERVAL MINUTE| Represents a duration of time in minutes.  
INTERVAL MINUTE TO SECOND| Represents a duration of time in minutes, seconds, and fractional seconds.  
INTERVAL SECOND| Represents a duration of time in seconds and fractional seconds.  
  
The following sections describe interval data types in more detail:

  * Benefits of interval data types
  * Syntax of interval data types
  * Representing interval values
  * Interval formats
  * Examples of interval values
  * Operations that involve date and time values
  * Functions that accept interval values as arguments
  * Examples for interval data types
  * Limitations for interval data types



Note

You can also use interval constants for date and time arithmetic. However, interval constants don’t support interval storage as a column type.

#### Benefits of interval data types¶

Interval data types provide the following benefits:

  * Ensure accurate date arithmetic without ambiguity.
  * Eliminate the need for manual [conversion and casting](/sql-reference/data-type-conversion) from integer-based durations.
  * Optimize storage for data that represents intervals of time.
  * Optimize query execution for duration data.
  * Simplify the migration of data from third-party databases, such as Databricks, Oracle, and Teradata.
  * Comply fully with ANSI standards.



#### Syntax of interval data types¶

To specify an interval data type, use the following syntax:
[code] 
    INTERVAL { yearMonthQualifier | dayTimeQualifier }
    
[/code]

Where:

> 
[code]
>     yearMonthQualifier ::=
>       {
>         YEAR [ (<precision>) ] [ TO MONTH ]
>         | MONTH [ (<precision>) ]
>       }
>     
[/code]
[code]
>     dayTimeQualifier ::=
>       {
>         DAY [ (<precision>) ] [ TO { HOUR | MINUTE | SECOND [ (<fractional_seconds_precision>) ] } ]
>         | HOUR [ (<precision>) ] [ TO { MINUTE | SECOND [ (<fractional_seconds_precision>) ] } ]
>         | MINUTE [ (<precision>) ] [ TO SECOND [ (<fractional_seconds_precision>) ] ]
>         | SECOND [ (<precision>) [ , (<fractional_seconds_precision>) ] ]
>       }
>     
[/code]

Properties:

  * `_precision_` is the total number of digits that is allowed. Precision can range from `1` to `9`.

Default: `9`

  * `_fractional_seconds_precision_` is the number of digits in the fractional part of a second. Time precision can range from `0` (seconds) to `9` (nanoseconds).

Default: `9`




Use this syntax when you are specifying an interval data type. For example, the following table has a `duration` column of INTERVAL YEAR TO MONTH type:
[code] 
    CREATE OR REPLACE TEMPORARY TABLE sample_table_with_interval (
      id VARCHAR,
      duration INTERVAL YEAR(2) TO MONTH);
    
[/code]

#### Representing interval values¶

You can represent an interval value by using an interval literal or an interval format:

  * Interval literals
  * Interval formats
  * Examples of interval values



##### Interval literals¶

An interval literal is an expression that specifies a duration of time in a string literal. Use the following syntax to specify an interval literal:
[code] 
    INTERVAL '[ <sign> ] <string>' { <yearMonthQualifier> | <dayTimeQualifier> }
    
[/code]

Where:

  * `_sign_` is an optional symbol that specifies a positive (`+`) or negative (`-`) duration of time.

Default: `+`.

  * `_string_` is a value that represents a time duration.

  * `_yearMonthQualifier_` is a qualifier that is defined in Syntax of interval data types.

  * `_dayTimeQualifier_` is a qualifier that is defined in Syntax of interval data types.




##### Interval formats¶

String literals in specific formats can represent interval values.

To specify values for years and months, use the following format:
[code] 
    '<sign><Y>-<MM>'
    
[/code]

Where:

  * `_sign_` is a required symbol that specifies a positive (`+`) or negative (`-`) duration of time.

Default: `+`.

  * `_Y_` is the number of years. The number of digits that is allowed (precision) depends on the data type of the value.

  * `_MM_` is two digits for the number of months, from `00` to `11`.




To specify values for days, hours, seconds, and fractional seconds, use the following format:
[code] 
    '<sign>[<D>] [<HH24>]:[<MI>]:[<SS>].[<F>]'
    
[/code]

Where:

  * `_sign_` is a required symbol that specifies a positive (`+`) or negative (`-`) duration of time.

Default: `+`.

  * `_D_` is the number of days. The number of digits that is allowed (precision) depends on the data type of the value.

Omit `_D_` for values of the following types:

    * INTERVAL HOUR
    * INTERVAL HOUR TO MINUTE
    * INTERVAL HOUR TO SECOND
    * INTERVAL MINUTE
    * INTERVAL MINUTE TO SECOND
    * INTERVAL SECOND
  * `_HH24_` is two digits for the number of hours, from `00` to `23`.

Omit `_HH24_` for values of the following types:

    * INTERVAL DAY
    * INTERVAL MINUTE
    * INTERVAL MINUTE TO SECOND
    * INTERVAL SECOND
  * `_MI_` is two digits for the number of minutes, from `00` through `59`.

Omit `_MI_` for values of the following types:

    * INTERVAL DAY TO HOUR
    * INTERVAL DAY
    * INTERVAL HOUR
    * INTERVAL SECOND
  * `_SS_` is two digits for the number of seconds, from `00` through `59`.

Omit `_SS_` for values of the following types:

    * INTERVAL DAY
    * INTERVAL DAY TO HOUR
    * INTERVAL DAY TO MINUTE
    * INTERVAL HOUR
    * INTERVAL HOUR TO MINUTE
    * INTERVAL MINUTE
  * `_F_` is the number of fractional seconds for the data types that include seconds. The number of digits that is allowed (precision) depends on the data type of the value.




The following usage notes apply to string literals in interval format:

  * The string literal representation applies when you use the CAST or TO_CHAR function to cast intervals explicitly to text strings.
  * Leading zeros in a field specify precision.



##### Examples of interval values¶

The following table shows how to represent various interval values. The values shown in the table conform to the following rules for interval values:

  * For positive values, the plus sign `+` is optional for interval literal values but required for interval format values.
  * In the interval literal values, the value in parentheses specifies the precision, which is the number of digits that is allowed. For example, `YEAR(3)` specifies that three digits are allowed in the year.
  * In the interval format values, the primary field (the leading field) does not include leading zeros. Subordinate fields use a fixed number of digits. For example, in a YEAR TO MONTH value like `+1-08`, the year field has no leading zeros, and the month field uses two digits.



Duration| Type| Interval literal value| Interval format value  
---|---|---|---  
Positive 5 years| INTERVAL YEAR| `INTERVAL '5' YEAR(2)`| `'+5'`  
Positive 1 year and 8 months| INTERVAL YEAR TO MONTH| `INTERVAL '1-08' YEAR(3) TO MONTH`| `'+001-08'`  
Negative 5 months| INTERVAL MONTH| `INTERVAL '-5' MONTH(2)`| `'-5'`  
Positive 14 months| INTERVAL MONTH| `INTERVAL '14' MONTH(2)`| `'+14'`  
Negative 44 years and 11 months| INTERVAL YEAR TO MONTH| `INTERVAL '-44-11' YEAR(2) TO MONTH`| `'-44-11'`  
Positive 11 days, 10 hours, and 9 minutes| INTERVAL DAY TO MINUTE| `INTERVAL '11 10:09' DAY(2) TO MINUTE`| `'+11 10:09'`  
Positive 2 days, 23 hours, 8 minutes, 23 seconds, and 275 milliseconds| INTERVAL DAY TO SECOND| `INTERVAL '02 23:08:23.275' DAY(2) TO SECOND(3)`| `'+2 23:08:23.275'`  
Positive 4 seconds and 300 milliseconds| INTERVAL SECOND| `INTERVAL '4.3' SECOND(5, 6)`| `'+4.300000'`  
  
#### Operations that involve date and time values¶

The following table shows the data type of the result for valid arithmetic operations that involve interval values:

First operand| Operator| Second operand| Result type  
---|---|---|---  
Timestamp| `-`| Timestamp| An interval data type  
Date or timestamp| `+`| Interval| DATE, DATETIME, TIMESTAMP_LTZ, TIMESTAMP_NTZ, or TIMESTAMP_TZ  
Date or timestamp| `-`| Interval| DATE, DATETIME, TIMESTAMP_LTZ, TIMESTAMP_NTZ, or TIMESTAMP_TZ  
Interval| `+`| Date or timestamp| DATE, DATETIME, TIMESTAMP_LTZ, TIMESTAMP_NTZ, or TIMESTAMP_TZ  
Numeric| `*`| Interval| An interval data type  
Interval| `*`| Numeric| An interval data type  
Interval| `/`| Numeric| An interval data type  
Interval| `+`| Interval| An interval data type  
Interval| `-`| Interval| An interval data type  
  
For operations that involve two interval values, the values must both be year-month interval values, or they must both be day-time interval values. Operations that mix year-month interval values and day-time interval values aren’t supported. When the operation involves two year-month interval values, the result type is a year-month interval type. When the operation involves two day-time interval values, the result type is a day-time interval type.

#### Functions that accept interval values as arguments¶

The following functions accept interval values as arguments:

  * [ABS](/sql-reference/functions/abs)
  * [ANY_VALUE](/sql-reference/functions/any_value)
  * [AVG](/sql-reference/functions/avg)
  * [[ NOT ] BETWEEN](/sql-reference/functions/between)
  * [COALESCE](/sql-reference/functions/coalesce)
  * [COUNT](/sql-reference/functions/count)
  * [COUNT_IF](/sql-reference/functions/count_if)
  * [DATE_PART](/sql-reference/functions/date_part)
  * [DAY](/sql-reference/functions/year)
  * [DENSE_RANK](/sql-reference/functions/dense_rank)
  * [[ NOT ] EQUAL_NULL](/sql-reference/functions/equal_null)
  * [EXTRACT](/sql-reference/functions/extract)
  * [FIRST_VALUE](/sql-reference/functions/first_value)
  * [GREATEST](/sql-reference/functions/greatest)
  * [GREATEST_IGNORE_NULLS](/sql-reference/functions/greatest_ignore_nulls)
  * [HOUR](/sql-reference/functions/hour-minute-second)
  * [IFF](/sql-reference/functions/iff)
  * [IFNULL](/sql-reference/functions/ifnull)
  * [[ NOT ] IN](/sql-reference/functions/in)
  * [IS [ NOT ] DISTINCT FROM](/sql-reference/functions/is-distinct-from)
  * [IS [ NOT ] NULL](/sql-reference/functions/is-null)
  * [LAG](/sql-reference/functions/lag)
  * [LAST_VALUE](/sql-reference/functions/last_value)
  * [LEAD](/sql-reference/functions/lead)
  * [LEAST](/sql-reference/functions/least)
  * [LEAST_IGNORE_NULLS](/sql-reference/functions/least_ignore_nulls)
  * [MAX](/sql-reference/functions/max)
  * [MAX_BY](/sql-reference/functions/max_by)
  * [MEDIAN](/sql-reference/functions/median)
  * [MIN](/sql-reference/functions/min)
  * [MINUTE](/sql-reference/functions/hour-minute-second)
  * [MIN_BY](/sql-reference/functions/min_by)
  * [MONTH](/sql-reference/functions/year)
  * [NTH_VALUE](/sql-reference/functions/nth_value)
  * [NVL](/sql-reference/functions/nvl)
  * [NVL2](/sql-reference/functions/nvl2)
  * [PERCENT_RANK](/sql-reference/functions/percent_rank)
  * [PERCENTILE_CONT](/sql-reference/functions/percentile_cont)
  * [PERCENTILE_DISC](/sql-reference/functions/percentile_disc)
  * [RANK](/sql-reference/functions/rank)
  * [SECOND](/sql-reference/functions/hour-minute-second)
  * [SIGN](/sql-reference/functions/sign)
  * [SUM](/sql-reference/functions/sum)
  * [TO_CHAR](/sql-reference/functions/to_char)
  * [TO_VARCHAR](/sql-reference/functions/to_char)
  * [YEAR](/sql-reference/functions/year)



#### Examples for interval data types¶

The following examples show how to use interval data types:

  * Performing arithmetic by using interval data
  * Inserting and querying year-month interval data
  * Inserting and querying day-time interval data
  * Copying interval data into a table and querying the table



##### Performing arithmetic by using interval data¶

The following examples perform arithmetic by using interval data.

Add one year and one month to a date:
[code] 
    SELECT TO_DATE('2024-01-01') + INTERVAL '1-1' YEAR TO MONTH
      AS date_plus_one_year_one_month;
    
[/code]
[code] 
    +------------------------------+
    | DATE_PLUS_ONE_YEAR_ONE_MONTH |
    |------------------------------|
    | 2025-02-01                   |
    +------------------------------+
    
[/code]

Subtract one year and one month from a date:
[code] 
    SELECT TO_DATE('2024-01-01') + INTERVAL '-1-1' YEAR TO MONTH
      AS date_plus_one_year_one_month;
    
[/code]
[code] 
    +------------------------------+
    | DATE_PLUS_ONE_YEAR_ONE_MONTH |
    |------------------------------|
    | 2022-12-01                   |
    +------------------------------+
    
[/code]

Add a period of time to a timestamp:
[code] 
    SELECT TO_TIMESTAMP('2024-01-01 08:08:08.99') + INTERVAL '1 01:01:01.7878' DAY TO SECOND
      AS date_plus_period_of_time;
    
[/code]
[code] 
    +--------------------------+
    | DATE_PLUS_PERIOD_OF_TIME |
    |--------------------------|
    | 2024-01-02 09:09:10.777  |
    +--------------------------+
    
[/code]

The following example uses the [SYSTEM$TYPEOF](/sql-reference/functions/system_typeof) function to show that an INTERVAL DAY TO SECOND value is returned when a query subtracts two timestamp values:
[code] 
    SELECT SYSTEM$TYPEOF(TO_TIMESTAMP('2025-10-05 01:02:03') - TO_TIMESTAMP('2025-09-15 11:36:22'))
      AS type;
    
[/code]
[code] 
    +------------------------------------+
    | TYPE                               |
    |------------------------------------|
    | INTERVAL DAY(9) TO SECOND(9)[SB16] |
    +------------------------------------+
    
[/code]

To view the results of the query in interval format, you can cast the expression to the INTERVAL DAY(2) TO SECOND(2) data type to specify precision, and then cast to VARCHAR:
[code] 
    SELECT (TO_TIMESTAMP('2025-10-05 01:02:03') - TO_TIMESTAMP('2025-09-15 11:36:22'))::INTERVAL DAY(2) TO SECOND(2)::VARCHAR
      AS interval_format_result;
    
[/code]
[code] 
    +------------------------+
    | INTERVAL_FORMAT_RESULT |
    |------------------------|
    | +19 13:25:41.00        |
    +------------------------+
    
[/code]

##### Inserting and querying year-month interval data¶

Create a table that tracks candidates for open positions with an INTERVAL YEAR TO MONTH column, and insert data:
[code] 
    CREATE OR REPLACE TABLE candidates (
      name_first VARCHAR,
      name_last VARCHAR,
      duration_of_experience INTERVAL YEAR(2) TO MONTH);
    
    INSERT INTO candidates VALUES ('Jane', 'Smith', '14-4');
    INSERT INTO candidates VALUES ('Robert', 'Adams', '0-3');
    INSERT INTO candidates VALUES ('Mary', 'Jones', '5-11');
    
[/code]

When you query the table without casting the _duration_of_experience`_ column to a data type, the output shows the column values as the total number of months in each row:
[code] 
    SELECT name_first,
           name_last,
           duration_of_experience AS months_of_experience
      FROM candidates;
    
[/code]
[code] 
    +------------+-----------+----------------------+
    | NAME_FIRST | NAME_LAST | MONTHS_OF_EXPERIENCE |
    |------------+-----------+----------------------|
    | Jane       | Smith     |                  172 |
    | Robert     | Adams     |                    3 |
    | Mary       | Jones     |                   71 |
    +------------+-----------+----------------------+
    
[/code]

When you query the table and cast the `duration_of_experience` column to the VARCHAR data type, the output shows the column values in interval format:
[code] 
    SELECT name_first,
           name_last,
           duration_of_experience::VARCHAR AS duration_of_experience
      FROM candidates;
    
[/code]
[code] 
    +------------+-----------+------------------------+
    | NAME_FIRST | NAME_LAST | DURATION_OF_EXPERIENCE |
    |------------+-----------+------------------------|
    | Jane       | Smith     | +14-04                 |
    | Robert     | Adams     | +0-03                  |
    | Mary       | Jones     | +5-11                  |
    +------------+-----------+------------------------+
    
[/code]

##### Inserting and querying day-time interval data¶

Create a table that specifies the timeout duration for various software features with an INTERVAL HOUR TO SECOND column, and insert data:
[code] 
    CREATE OR REPLACE TABLE feature_timeouts (
      feature VARCHAR,
      timeout_duration INTERVAL HOUR(2) TO SECOND(0));
    
    INSERT INTO feature_timeouts VALUES ('Feature1', '00:00:30');
    INSERT INTO feature_timeouts VALUES ('Feature2', '00:10:00');
    INSERT INTO feature_timeouts VALUES ('Feature3', '01:00:00');
    
[/code]

Query the table and cast the `timeout_duration` column to the VARCHAR data type:
[code] 
    SELECT feature,
           timeout_duration::VARCHAR AS timeout_duration
      FROM feature_timeouts;
    
[/code]
[code] 
    +----------+------------------+
    | FEATURE  | TIMEOUT_DURATION |
    |----------+------------------|
    | Feature1 | +0:00:30         |
    | Feature2 | +0:10:00         |
    | Feature3 | +1:00:00         |
    +----------+------------------+
    
[/code]

##### Copying interval data into a table and querying the table¶

Complete the following steps to stage a file with interval data, and then copy the file into a table:

  1. In a file on your file system, copy the following content:
[code] 1,1-2,28 16:15:14.0
         2,-3-2,-54 16:15:14.123
         
[/code]

This example assumes that the file is named `interval_values.csv` in the `/examples/intervals/` directory.

  2. Create a stage:
[code] CREATE STAGE interval_stage;
         
[/code]

  3. In the internal staging location, stage the file:
[code] PUT file:///examples/intervals/interval_values.csv @~/interval_stage
           AUTO_COMPRESS=false;
         
[/code]

  4. Create a table for the data:
[code] CREATE OR REPLACE TABLE sample_interval_values(
           c1 STRING,
           c2 INTERVAL YEAR(1) TO MONTH,
           c3 INTERVAL DAY(2) TO SECOND(3));
         
[/code]

  5. To load the staged file into the table that you created, use the [COPY INTO <table>](/sql-reference/sql/copy-into-table) command:
[code] COPY INTO sample_interval_values FROM @~/interval_stage;
         
[/code]

  6. To view the loaded data, query the table, and cast to the VARCHAR type to view the loaded data:
[code] SELECT c1,
             c2::VARCHAR AS YEAR_TO_MONTH,
             c3::VARCHAR AS DAY_TO_SECOND,
           FROM sample_interval_values;
         
[/code]
[code] +----+---------------+------------------+
         | C1 | YEAR_TO_MONTH | DAY_TO_SECOND    |
         |----+---------------+------------------|
         | 1  | +1-02         | +28 16:15:14.000 |
         | 2  | -3-02         | -54 16:15:14.123 |
         +----+---------------+------------------+
         
[/code]




#### Limitations for interval data types¶

The following limitations apply to interval data types:

  * Year-month interval values can’t be combined or compared with day-time interval values.

  * Interval constants and values of interval data type can’t be combined or compared.

  * Interval constants can’t be inserted into a column that has an interval data type.

  * [VARIANT](/sql-reference/data-types-semistructured#label-data-type-variant) values can’t contain interval values.

  * [Structured data type](/sql-reference/data-types-structured) values can’t contain interval values.

  * Interval expressions can’t be used in [user-defined functions (UDFs)](/developer-guide/udf/udf-overview) or [Snowflake Scripting](/developer-guide/snowflake-scripting/index).

  * The following types of tables can’t have interval columns:

    * [Clustered tables](/user-guide/tables-clustering-keys)
    * [Dynamic tables](/user-guide/dynamic-tables/overview)
    * [Apache Iceberg™ tables](/user-guide/tables-iceberg)
  * Queries on interval columns can’t benefit from the [Search optimization service](/user-guide/search-optimization-service).




### TIME¶

Snowflake supports a single TIME data type for storing times in the form of `HH:MI:SS`.

TIME supports an optional precision parameter for fractional seconds (for example, `TIME(3)`). Time precision can range from 0 (seconds) to 9 (nanoseconds). The default precision is 9.

All TIME values must be between `00:00:00` and `23:59:59.999999999`. TIME internally stores “wallclock” time, and all operations on TIME values are performed without taking any time zone into consideration.

### TIMESTAMP_LTZ , TIMESTAMP_NTZ , TIMESTAMP_TZ¶

Snowflake supports three variations of timestamp:

TIMESTAMP_LTZ:
    

TIMESTAMP_LTZ internally stores UTC values with a specified precision. However, all operations are performed in the current session’s time zone, controlled by the [TIMEZONE](/sql-reference/parameters#label-timezone) session parameter.

Synonymous with TIMESTAMP_LTZ:

  * TIMESTAMPLTZ
  * TIMESTAMP WITH LOCAL TIME ZONE


TIMESTAMP_NTZ:
    

TIMESTAMP_NTZ internally stores “wallclock” time with a specified precision. All operations are performed without taking any time zone into account.

If the output format contains a time zone, the UTC indicator (`Z`) is displayed.

TIMESTAMP_NTZ is the default for TIMESTAMP.

Synonymous with TIMESTAMP_NTZ:

  * TIMESTAMPNTZ
  * TIMESTAMP WITHOUT TIME ZONE
  * DATETIME


TIMESTAMP_TZ:
    

TIMESTAMP_TZ internally stores UTC values together with an associated _time zone offset_. When a time zone isn’t provided, the session time zone offset is used. All operations are performed with the time zone offset specific to each record.

Synonymous with TIMESTAMP_TZ:

  * TIMESTAMPTZ
  * TIMESTAMP WITH TIME ZONE



TIMESTAMP_TZ values are compared based on their times in UTC. For example, the following comparison between different times in different timezones returns TRUE because the two values have equivalent times in UTC.
[code]
    SELECT '2024-01-01 00:00:00 +0000'::TIMESTAMP_TZ = '2024-01-01 01:00:00 +0100'::TIMESTAMP_TZ;
    
[/code]

Attention

TIMESTAMP_TZ currently only stores the _offset_ of a given time zone, not the actual _time zone_ , at the moment of creation for a given value. This is especially important for daylight saving time, which is not utilized by UTC.

For example, with the [TIMEZONE](/sql-reference/parameters#label-timezone) parameter set to `"America/Los_Angeles"`, converting a value to TIMESTAMP_TZ in January of a given year stores the time zone offset of `-0800`. If six months are later added to the value, the `-0800` offset is retained, even though in July the offset for Los Angeles is `-0700`. This is because, after the value is created, the actual time zone information (`"America/Los_Angeles"`) is no longer available. The following code sample illustrates this behavior:
[code]
    SELECT '2024-01-01 12:00:00'::TIMESTAMP_TZ;
    
[/code]
[code]
    +-------------------------------------+
    | '2024-01-01 12:00:00'::TIMESTAMP_TZ |
    |-------------------------------------|
    | 2024-01-01 12:00:00.000 -0800       |
    +-------------------------------------+
    
[/code]
[code]
    SELECT DATEADD(MONTH, 6, '2024-01-01 12:00:00'::TIMESTAMP_TZ);
    
[/code]
[code]
    +--------------------------------------------------------+
    | DATEADD(MONTH, 6, '2024-01-01 12:00:00'::TIMESTAMP_TZ) |
    |--------------------------------------------------------|
    | 2024-07-01 12:00:00.000 -0800                          |
    +--------------------------------------------------------+
    
[/code]

#### TIMESTAMP¶

TIMESTAMP in Snowflake is a user-specified alias associated with one of the TIMESTAMP_* variations. In all operations where TIMESTAMP is used, the associated TIMESTAMP_* variation is automatically used. The TIMESTAMP data type is never stored in tables.

The TIMESTAMP_* variation associated with TIMESTAMP is specified by the [TIMESTAMP_TYPE_MAPPING](/sql-reference/parameters#label-timestamp-type-mapping) session parameter. The default is TIMESTAMP_NTZ.

All timestamp variations, as well as the TIMESTAMP alias, support an optional precision parameter for fractional seconds (for example, `TIMESTAMP(3)`). Timestamp precision can range from 0 (seconds) to 9 (nanoseconds). The default precision is 9.

#### Timestamp examples¶

These examples create a table using different timestamps.

First, create a table with a TIMESTAMP column (mapped to TIMESTAMP_NTZ):
[code] 
    ALTER SESSION SET TIMESTAMP_TYPE_MAPPING = TIMESTAMP_NTZ;
    
    CREATE OR REPLACE TABLE ts_test(ts TIMESTAMP);
    
    DESC TABLE ts_test;
    
[/code]
[code] 
    +------+------------------+--------+-------+---------+-------------+------------+-------+------------+---------+-------------+----------------+
    | name | type             | kind   | null? | default | primary key | unique key | check | expression | comment | policy name | privacy domain |
    |------+------------------+--------+-------+---------+-------------+------------+-------+------------+---------+-------------+----------------|
    | TS   | TIMESTAMP_NTZ(9) | COLUMN | Y     | NULL    | N           | N          | NULL  | NULL       | NULL    | NULL        | NULL           |
    +------+------------------+--------+-------+---------+-------------+------------+-------+------------+---------+-------------+----------------+
    
[/code]

Next, explicitly use one of the TIMESTAMP variations (TIMESTAMP_LTZ):
[code] 
    CREATE OR REPLACE TABLE ts_test(ts TIMESTAMP_LTZ);
    
    DESC TABLE ts_test;
    
[/code]
[code] 
    +------+------------------+--------+-------+---------+-------------+------------+-------+------------+---------+-------------+----------------+
    | name | type             | kind   | null? | default | primary key | unique key | check | expression | comment | policy name | privacy domain |
    |------+------------------+--------+-------+---------+-------------+------------+-------+------------+---------+-------------+----------------|
    | TS   | TIMESTAMP_LTZ(9) | COLUMN | Y     | NULL    | N           | N          | NULL  | NULL       | NULL    | NULL        | NULL           |
    +------+------------------+--------+-------+---------+-------------+------------+-------+------------+---------+-------------+----------------+
    
[/code]

Use TIMESTAMP_LTZ with different time zones:
[code] 
    CREATE OR REPLACE TABLE ts_test(ts TIMESTAMP_LTZ);
    
    ALTER SESSION SET TIMEZONE = 'America/Los_Angeles';
    
    INSERT INTO ts_test VALUES('2024-01-01 16:00:00');
    INSERT INTO ts_test VALUES('2024-01-02 16:00:00 +00:00');
    
[/code]

This query shows that the time for January 2nd is 08:00 in Los Angeles (which is 16:00 in UTC):
[code] 
    SELECT ts, HOUR(ts) FROM ts_test;
    
[/code]
[code] 
    +-------------------------------+----------+
    | TS                            | HOUR(TS) |
    |-------------------------------+----------|
    | 2024-01-01 16:00:00.000 -0800 |       16 |
    | 2024-01-02 08:00:00.000 -0800 |        8 |
    +-------------------------------+----------+
    
[/code]

Next, note that the times change with a different time zone:
[code] 
    ALTER SESSION SET TIMEZONE = 'America/New_York';
    
    SELECT ts, HOUR(ts) FROM ts_test;
    
[/code]
[code] 
    +-------------------------------+----------+
    | TS                            | HOUR(TS) |
    |-------------------------------+----------|
    | 2024-01-01 19:00:00.000 -0500 |       19 |
    | 2024-01-02 11:00:00.000 -0500 |       11 |
    +-------------------------------+----------+
    
[/code]

Create a table and use TIMESTAMP_NTZ:
[code] 
    CREATE OR REPLACE TABLE ts_test(ts TIMESTAMP_NTZ);
    
    ALTER SESSION SET TIMEZONE = 'America/Los_Angeles';
    
    INSERT INTO ts_test VALUES('2024-01-01 16:00:00');
    INSERT INTO ts_test VALUES('2024-01-02 16:00:00 +00:00');
    
[/code]

Note that both times from different time zones are converted to the same “wallclock” time:
[code] 
    SELECT ts, HOUR(ts) FROM ts_test;
    
[/code]
[code] 
    +-------------------------+----------+
    | TS                      | HOUR(TS) |
    |-------------------------+----------|
    | 2024-01-01 16:00:00.000 |       16 |
    | 2024-01-02 16:00:00.000 |       16 |
    +-------------------------+----------+
    
[/code]

Next, note that changing the session time zone doesn’t affect the results:
[code] 
    ALTER SESSION SET TIMEZONE = 'America/New_York';
    
    SELECT ts, HOUR(ts) FROM ts_test;
    
[/code]
[code] 
    +-------------------------+----------+
    | TS                      | HOUR(TS) |
    |-------------------------+----------|
    | 2024-01-01 16:00:00.000 |       16 |
    | 2024-01-02 16:00:00.000 |       16 |
    +-------------------------+----------+
    
[/code]

Create a table and use TIMESTAMP_TZ:
[code] 
    CREATE OR REPLACE TABLE ts_test(ts TIMESTAMP_TZ);
    
    ALTER SESSION SET TIMEZONE = 'America/Los_Angeles';
    
    INSERT INTO ts_test VALUES('2024-01-01 16:00:00');
    INSERT INTO ts_test VALUES('2024-01-02 16:00:00 +00:00');
    
[/code]

Note that the January 1st record inherited the session time zone, and `America/Los_Angeles` was converted to a numeric time zone offset:
[code] 
    SELECT ts, HOUR(ts) FROM ts_test;
    
[/code]
[code] 
    +-------------------------------+----------+
    | TS                            | HOUR(TS) |
    |-------------------------------+----------|
    | 2024-01-01 16:00:00.000 -0800 |       16 |
    | 2024-01-02 16:00:00.000 +0000 |       16 |
    +-------------------------------+----------+
    
[/code]

Next, note that changing the session time zone doesn’t affect the results:
[code] 
    ALTER SESSION SET TIMEZONE = 'America/New_York';
    
    SELECT ts, HOUR(ts) FROM ts_test;
    
[/code]
[code] 
    +-------------------------------+----------+
    | TS                            | HOUR(TS) |
    |-------------------------------+----------|
    | 2024-01-01 16:00:00.000 -0800 |       16 |
    | 2024-01-02 16:00:00.000 +0000 |       16 |
    +-------------------------------+----------+
    
[/code]

## Supported calendar¶

Snowflake uses the Gregorian Calendar for all dates and timestamps. The Gregorian Calendar starts in the year 1582, but recognizes prior years, which is important to note because Snowflake does not adjust dates prior to 1582 (or calculations involving dates prior to 1582) to match the Julian Calendar. The `UUUU` format element supports negative years.

## Date and time formats¶

All of these data types accept most non-ambiguous date, time, or date + time formats. See [Supported formats for AUTO detection](/sql-reference/date-time-input-output#label-date-time-input-output-supported-formats-for-auto-detection) for the formats that Snowflake recognizes when [configured to detect the format automatically](/sql-reference/date-time-input-output#label-date-time-input-output-how-snowflake-determines).

You can also [specify the date and time format manually](/sql-reference/date-time-input-output#label-date-time-input-output-how-snowflake-determines). When specifying the format, you can use the case-insensitive elements listed in the following table:

Format element| Description  
---|---  
`YYYY`| Four-digit [1] year.  
`YY`| Two-digit [1] year, controlled by the [TWO_DIGIT_CENTURY_START](/sql-reference/parameters#label-two-digit-century-start) session parameter. For example, when set to `1980`, values of `79` and `80` are parsed as `2079` and `1980`, respectively.  
`Y`| One-digit or two-digit [2] year without leading zeros, controlled by the [TWO_DIGIT_CENTURY_START](/sql-reference/parameters#label-two-digit-century-start) session parameter. For example, when the parameter is set to `1990`, values of `2005` and `1991` are serialized as `5` and `91`, respectively.  
`MM`| Two-digit [1] month (`01` = January, and so on).  
`MO`| One-digit or two-digit [2] month without leading zeros (`1` = January, and so on).  
`MON`| Abbreviated month name [3].  
`MMMM`| Full month name [3].  
`DD`| Two-digit [1] day of month (`01` through `31`).  
`D`| One-digit or two-digit [2] day of month without leading zeros (`1` through `31`).  
`DY`| Abbreviated day of week.  
`HH24`| Two digits [1] for hour (`00` through `23`). You _must not_ specify `AM` / `PM` or `A` / `P`.  
`HH12`| Two digits [1] for hour (`01` through `12`). You can specify `AM` / `PM` or `A` / `P`.  
`H24`| One or two digits [2] for hour without leading zeros (`0` through `23`). You _must not_ specify `AM` / `PM` or `A` / `P`.  
`H12`| One or two digits [2] for hour without leading zeros (`1` through `12`). You can specify `AM` / `PM` or `A` / `P`.  
`AM` , `PM`| Ante meridiem (`AM`) / post meridiem (`PM`). Use this only with `HH12` and `H12` (_not_ with `HH24` or `H24`).  
`P`| Ante meridiem (`A`) / post meridiem (`P`). Use this only with `HH12` and `H12` (_not_ with `HH24` or `H24`).  
`HH`| Synonym for `HH24`.  
`H`| Synonym for `H24`.  
`MI`| Two digits [1] for minute (`00` through `59`).  
`ME`| One or two digits [2] for minute without leading zeros (`0` through `59`).  
`SS`| Two digits [1] for second (`00` through `59`).  
`S`| One or two digits [2] for second without leading zeros (`0` through `59`).  
`FF[0-9]`| Fractional seconds with precision `0` (seconds) to `9` (nanoseconds), for example, `FF`, `FF0`, `FF3`, `FF9`. Specifying `FF` is equivalent to `FF9` (nanoseconds).  
`TZH:TZM` , `TZHTZM` , `TZH`| Two-digit [1] time zone hour and minute, offset from UTC. Can be prefixed by `+`/`-` for sign.  
`UUUU`| Four-digit year in [ISO format](https://en.wikipedia.org/wiki/ISO_8601), which is negative for BCE years.  
  
[1] The number of digits describes the output produced when serializing values to text. When parsing text, Snowflake accepts up to the specified number of digits. For example, a day number can be one or two digits.

[2] The number of digits describes the output produced when serializing values to text: values are serialized without leading zeros. When parsing text, Snowflake accepts one or two digits.

[3] For the MON format element, the output produced when serializing values to text is the abbreviated month name. For the MMMM format element, the output produced when serializing values to text is the full month name. When parsing text, Snowflake accepts the three-letter abbreviation or the full month name for both MON and MMMM. For example, “January” or “Jan”, “February” or “Feb”, and so on are accepted when parsing text.

Note

  * When a date-only format is used, the associated time is assumed to be midnight on that day.
  * Anything in the format between double quotes or other than the above elements is parsed/formatted without being interpreted. Snowflake recommends always enclosing literal characters in double quotes (for example, `"T"`, `"EST"`, `"Z"`) to ensure they are treated as literals.
  * For more details about valid ranges, number of digits, and best practices, see [Additional information about using date, time, and timestamp formats](/sql-reference/date-time-input-output#label-date-time-input-output-additional-information).



### Examples of using date and time formats¶

The following example uses `FF` to indicate that the output has 9 digits in the fractional seconds field:
[code] 
    CREATE OR REPLACE TABLE timestamp_demo_table(
      tstmp TIMESTAMP,
      tstmp_tz TIMESTAMP_TZ,
      tstmp_ntz TIMESTAMP_NTZ,
      tstmp_ltz TIMESTAMP_LTZ);
    INSERT INTO timestamp_demo_table (tstmp, tstmp_tz, tstmp_ntz, tstmp_ltz) VALUES (
      '2024-03-12 01:02:03.123456789',
      '2024-03-12 01:02:03.123456789',
      '2024-03-12 01:02:03.123456789',
      '2024-03-12 01:02:03.123456789');
    
[/code]
[code] 
    ALTER SESSION SET TIMESTAMP_OUTPUT_FORMAT = 'YYYY-MM-DD HH24:MI:SS.FF';
    ALTER SESSION SET TIMESTAMP_TZ_OUTPUT_FORMAT = 'YYYY-MM-DD HH24:MI:SS.FF';
    ALTER SESSION SET TIMESTAMP_NTZ_OUTPUT_FORMAT = 'YYYY-MM-DD HH24:MI:SS.FF';
    ALTER SESSION SET TIMESTAMP_LTZ_OUTPUT_FORMAT = 'YYYY-MM-DD HH24:MI:SS.FF';
    
[/code]
[code] 
    SELECT tstmp, tstmp_tz, tstmp_ntz, tstmp_ltz
      FROM timestamp_demo_table;
    
[/code]
[code] 
    +-------------------------------+-------------------------------+-------------------------------+-------------------------------+
    | TSTMP                         | TSTMP_TZ                      | TSTMP_NTZ                     | TSTMP_LTZ                     |
    |-------------------------------+-------------------------------+-------------------------------+-------------------------------|
    | 2024-03-12 01:02:03.123456789 | 2024-03-12 01:02:03.123456789 | 2024-03-12 01:02:03.123456789 | 2024-03-12 01:02:03.123456789 |
    +-------------------------------+-------------------------------+-------------------------------+-------------------------------+
    
[/code]

## Date and time constants¶

_Constants_ (also known as _literals_) are fixed data values. Snowflake supports using string constants to specify fixed date, time, or timestamp values. String constants must always be enclosed between delimiter characters. Snowflake supports using single quotes to delimit string constants.

For example:
[code] 
    DATE '2024-08-14'
    TIME '10:03:56'
    TIMESTAMP '2024-08-15 10:59:43'
    
[/code]

The string is parsed as a DATE, TIME, or TIMESTAMP value based on the input format for the data type, as set through the following parameters:

DATE:
    

[DATE_INPUT_FORMAT](/sql-reference/parameters#label-date-input-format)

TIME:
    

[TIME_INPUT_FORMAT](/sql-reference/parameters#label-time-input-format)

TIMESTAMP:
    

[TIMESTAMP_INPUT_FORMAT](/sql-reference/parameters#label-timestamp-input-format)

For example, to insert a specific date into a column in a table:
[code] 
    CREATE TABLE t1 (d1 DATE);
    
    INSERT INTO t1 (d1) VALUES (DATE '2024-08-15');
    
[/code]

## Interval constants¶

You can use interval constants to add or subtract a period of time to or from a date, time, or timestamp. Interval constants are implemented using the INTERVAL keyword, which has the following syntax:
[code] 
    { + | - } INTERVAL '<integer> [ <date_time_part> ] [ , <integer> [ <date_time_part> ] ... ]'
    
[/code]

As with all string constants, Snowflake requires single quotes to delimit interval constants.

Note

Interval constants support date and time arithmetic, but they don’t support interval storage as a column type. To store interval values in a column, you can use interval data types.

The INTERVAL keyword supports one or more integers and, optionally, one or more date or time parts. For example:

  * `INTERVAL '1 year'` represents one year.
  * `INTERVAL '4 years, 5 months, 3 hours'` represents four years, five months, and three hours.



If a date or time part isn’t specified, the interval represents seconds (for example, `INTERVAL '2'` is the same as `INTERVAL '2 seconds'`). Note that this is different from the default unit of time for performing date arithmetic. For more details, see Simple arithmetic for dates.

For the list of supported date and time parts, see Supported Date and Time Parts for Intervals.

Note

  * The order of interval increments is important. The increments are added or subtracted in the order listed. For example:

    * `INTERVAL '1 year, 1 day'` first adds or subtracts a year and then a day.
    * `INTERVAL '1 day, 1 year'` first adds or subtracts a day and then a year.

Ordering differences can affect calculations influenced by calendar events, such as leap years:
[code] SELECT TO_DATE ('2019-02-28') + INTERVAL '1 day, 1 year';
    
[/code]
[code] +---------------------------------------------------+
    | TO_DATE ('2019-02-28') + INTERVAL '1 DAY, 1 YEAR' |
    |---------------------------------------------------|
    | 2020-03-01                                        |
    +---------------------------------------------------+
    
[/code]
[code] SELECT TO_DATE ('2019-02-28') + INTERVAL '1 year, 1 day';
    
[/code]
[code] +---------------------------------------------------+
    | TO_DATE ('2019-02-28') + INTERVAL '1 YEAR, 1 DAY' |
    |---------------------------------------------------|
    | 2020-02-29                                        |
    +---------------------------------------------------+
    
[/code]

  * INTERVAL is not a data type (that is, you can’t define a table column to be of data type INTERVAL). Intervals can only be used in date, time, and timestamp arithmetic.

  * You can’t use an interval with a [SQL variable](/sql-reference/session-variables). For example, the following query returns an error:
[code] SET v1 = '1 year';
        
        SELECT TO_DATE('2023-04-15') + INTERVAL $v1;
        
[/code]




### Supported date and time parts for intervals¶

The INTERVAL keyword supports the following date and time parts as arguments (case-insensitive):

Date or Time Part| Abbreviations / Variations  
---|---  
`year`| `y` , `yy` , `yyy` , `yyyy` , `yr` , `years` , `yrs`  
`quarter`| `q` , `qtr` , `qtrs` , `quarters`  
`month`| `mm` , `mon` , `mons` , `months`  
`week`| `w` , `wk` , `weekofyear` , `woy` , `wy` , `weeks`  
`day`| `d` , `dd` , `days`, `dayofmonth`  
`hour`| `h` , `hh` , `hr` , `hours` , `hrs`  
`minute`| `m` , `mi` , `min` , `minutes` , `mins`  
`second`| `s` , `sec` , `seconds` , `secs`  
`millisecond`| `ms` , `msec` , `milliseconds`  
`microsecond`| `us` , `usec` , `microseconds`  
`nanosecond`| `ns` , `nsec` , `nanosec` , `nsecond` , `nanoseconds` , `nanosecs` , `nseconds`  
  
### Interval examples¶

Add a year interval to a specific date:
[code] 
    SELECT TO_DATE('2023-04-15') + INTERVAL '1 year';
    
[/code]
[code] 
    +-------------------------------------------+
    | TO_DATE('2023-04-15') + INTERVAL '1 YEAR' |
    |-------------------------------------------|
    | 2024-04-15                                |
    +-------------------------------------------+
    
[/code]

Add an interval of 3 hours and 18 minutes to a specific time:
[code] 
    SELECT TO_TIME('04:15:29') + INTERVAL '3 hours, 18 minutes';
    
[/code]
[code] 
    +------------------------------------------------------+
    | TO_TIME('04:15:29') + INTERVAL '3 HOURS, 18 MINUTES' |
    |------------------------------------------------------|
    | 07:33:29                                             |
    +------------------------------------------------------+
    
[/code]

Add a complex interval to the output of the CURRENT_TIMESTAMP function:
[code] 
    SELECT CURRENT_TIMESTAMP + INTERVAL
        '1 year, 3 quarters, 4 months, 5 weeks, 6 days, 7 minutes, 8 seconds,
        1000 milliseconds, 4000000 microseconds, 5000000001 nanoseconds'
      AS complex_interval1;
    
[/code]

The following is sample output. The output is different when the current timestamp is different.
[code] 
    +-------------------------------+
    | COMPLEX_INTERVAL1             |
    |-------------------------------|
    | 2026-11-07 18:07:19.875000001 |
    +-------------------------------+
    
[/code]

Add a complex interval with abbreviated date/time part notation to a specific date:
[code] 
    SELECT TO_DATE('2025-01-17') + INTERVAL
        '1 y, 3 q, 4 mm, 5 w, 6 d, 7 h, 9 m, 8 s,
        1000 ms, 445343232 us, 898498273498 ns'
      AS complex_interval2;
    
[/code]
[code] 
    +-------------------------------+
    | COMPLEX_INTERVAL2             |
    |-------------------------------|
    | 2027-03-30 07:31:32.841505498 |
    +-------------------------------+
    
[/code]

Query a table of employee information and return the names of employees who were hired within the past two years and three months:
[code] 
    SELECT name, hire_date
      FROM employees
      WHERE hire_date > CURRENT_DATE - INTERVAL '2 y, 3 month';
    
[/code]

Filter a TIMESTAMP column named `ts` from a table named `t1` and add four seconds to each returned value:
[code] 
    SELECT ts + INTERVAL '4 seconds'
      FROM t1
      WHERE ts > TO_TIMESTAMP('2024-04-05 01:02:03');
    
[/code]

## Simple arithmetic for dates¶

In addition to using interval constants to add to and subtract from dates, times, and timestamps, you can also add and subtract days to and from DATE values, in the form of `{ + | - }` `_integer_`, where `_integer_` specifies the number of days to add or subtract.

Note

TIME and TIMESTAMP values don’t yet support simple arithmetic.

### Date arithmetic examples¶

Add one day to a specific date:
[code] 
    SELECT TO_DATE('2024-04-15') + 1;
    
[/code]
[code] 
    +---------------------------+
    | TO_DATE('2024-04-15') + 1 |
    |---------------------------|
    | 2024-04-16                |
    +---------------------------+
    
[/code]

Subtract four days from a specific date:
[code] 
    SELECT TO_DATE('2024-04-15') - 4;
    
[/code]
[code] 
    +---------------------------+
    | TO_DATE('2024-04-15') - 4 |
    |---------------------------|
    | 2024-04-11                |
    +---------------------------+
    
[/code]

Query a table named `employees` and return the names of people who left the company, but were employed more than 365 days:
[code] 
    SELECT name
      FROM employees
      WHERE end_date > start_date + 365;
    
[/code]
