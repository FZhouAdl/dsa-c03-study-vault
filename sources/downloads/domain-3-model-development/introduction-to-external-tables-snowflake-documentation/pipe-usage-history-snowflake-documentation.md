---
title: "PIPE_USAGE_HISTORY | Snowflake Documentation"
source: https://docs.snowflake.com/sql-reference/functions/pipe_usage_history
cert_domain: domain-3-model-development
crawl_depth: 1
crawled: 2026-08-23
---

Categories:
    

[Information Schema](/sql-reference/info-schema#label-info-schema-functions) , [Table functions](/sql-reference/functions-table)

# PIPE_USAGE_HISTORY¶

This table function can be used to query the history of data loaded into Snowflake tables using [Snowpipe](/user-guide/data-load-snowpipe-intro) within a specified date range. The function returns the history of data loaded and credits billed for your entire Snowflake account.

Note

This function returns pipe activity within the last 14 days.

Note

This function is generally deprecated in favor of the [ACCOUNT_USAGE.PIPE_USAGE_HISTORY view](/sql-reference/account-usage/pipe_usage_history), which provides a more complete data set and supports longer date ranges.

## Syntax¶
[code] 
    PIPE_USAGE_HISTORY(
          [ DATE_RANGE_START => <constant_expr> ]
          [, DATE_RANGE_END => <constant_expr> ]
          [, PIPE_NAME => '<string>' ] )
    
[/code]

## Arguments¶

All the arguments are optional.

`DATE_RANGE_START => _constant_expr_` ,   
`DATE_RANGE_END => _constant_expr_`
    

The date/time range, within the last 2 weeks, for which to retrieve the data load history:

  * If an end date is not specified, then [CURRENT_DATE](/sql-reference/functions/current_date) is used as the end of the range.
  * If a start date is not specified, then the range starts 10 minutes prior to the start of `DATE_RANGE_END` (i.e. the default is to show the previous 10 minutes of data load history). For example, if `DATE_RANGE_END` is [CURRENT_DATE](/sql-reference/functions/current_date), then the default `DATE_RANGE_START` is 11:50 PM on the previous day.



History is displayed in increments of 5 minutes, 1 hour, or 24 hours (depending on the length of the specified range).

If the range falls outside the last 15 days, an error is returned.

`PIPE_NAME => _string_`
    

A string specifying a pipe. Only data loads that use the specified pipe are returned.

If a pipe name is not specified, then the PIPE_NAME column in the results displays NULL. Each row includes the totals for all pipes in use within the time range.

## Usage notes¶

  * Returns results only for the ACCOUNTADMIN role or any role that has been explicitly granted the MONITOR USAGE global privilege.

  * When calling an Information Schema table function, the session must have an INFORMATION_SCHEMA schema in use or the function name must be fully-qualified. For more details, see [Snowflake Information Schema](/sql-reference/info-schema).

  * Occasionally, the data compaction and maintenance process can consume Snowflake credits. For example, the returned results might show that you consumed credits with 0 BYTES_INSERTED and 0 FILES_INSERTED. This means that your data is not being loaded, but the data compaction and maintenance process has consumed some credits.

  * Snowflake bills for auto-refresh notifications in external tables and directory tables on internal named stages or external stages at a rate equivalent to the Snowpipe file charge. You can estimate charges incurred by your external table and directory table auto-refresh notifications by querying the PIPE_USAGE_HISTORY function or examining the Account Usage [PIPE_USAGE_HISTORY view](/sql-reference/account-usage/pipe_usage_history). Note that the auto-refresh pipes will be listed under a NULL pipe name. You can also view your external table auto-refresh notification history at the table-level/stage-level granularity by using the Information Schema table function [AUTO_REFRESH_REGISTRATION_HISTORY](/sql-reference/functions/auto_refresh_registration_history).

To avoid charges for auto-refresh notifications, perform a manual refresh for external tables and directory tables. For external tables, the ALTER EXTERNAL TABLE <name> REFRESH … statement can be used to manually synchronize your external table to external storage. For directory tables, the ALTER STAGE <name> REFRESH … statement can be used to manually synchronize the directory to external storage.




## Output¶

The function returns the following columns:

Column Name| Data Type| Description  
---|---|---  
START_TIME| TIMESTAMP_LTZ| Start of the specified time range in which data loads took place.  
END_TIME| TIMESTAMP_LTZ| End of the specified time range in which data loads took place.  
PIPE_NAME| TEXT| Name of the pipe used for a data load. Displays NULL if no pipe name is specified in the query. Each row includes the totals for all pipes in use within the time range.  
CREDITS_USED| TEXT| Number of credits billed for Snowpipe data loads during the START_TIME and END_TIME window.  
BYTES_INSERTED| NUMBER| Number of bytes loaded during the START_TIME and END_TIME window.  
FILES_INSERTED| NUMBER| Number of files loaded during the START_TIME and END_TIME window.  
BYTES_BILLED| NUMBER| Represents the number of bytes Snowpipe uses for billing purposes, providing visibility into Snowpipe’s cost implications directly within these history views.  
  
## Examples¶

Retrieve the data load history from a specific 30-minute range, in 5-minute periods, for all pipes in your account:

> 
[code]
>     select *
>       from table(information_schema.pipe_usage_history(
>         date_range_start=>to_timestamp_tz('2017-10-24 12:00:00.000 -0700'),
>         date_range_end=>to_timestamp_tz('2017-10-24 12:30:00.000 -0700')));
>     
[/code]

Retrieve the data load history from the last 14 days, in 1-day periods, for all pipes in your account:

> 
[code]
>     select *
>       from table(information_schema.pipe_usage_history(
>         date_range_start=>dateadd('day',-14,current_date()),
>         date_range_end=>current_date()));
>     
[/code]

Retrieve the data load history from the last 12 hours, in 1-hour periods, for a specified pipe in your account:

> 
[code]
>     select *
>       from table(information_schema.pipe_usage_history(
>         date_range_start=>dateadd('hour',-12,current_timestamp()),
>         pipe_name=>'mydb.public.mypipe'));
>     
[/code]

Retrieve the data load history from the last 14 days, in 1-day periods, for a specified pipe in your account:

> 
[code]
>     select *
>       from table(information_schema.pipe_usage_history(
>         date_range_start=>dateadd('day',-14,current_date()),
>         date_range_end=>current_date(),
>         pipe_name=>'mydb.public.mypipe'));
>     
[/code]
