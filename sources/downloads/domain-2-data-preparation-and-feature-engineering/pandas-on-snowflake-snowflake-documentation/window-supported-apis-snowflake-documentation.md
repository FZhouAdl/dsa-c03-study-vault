---
title: "Window supported APIs | Snowflake Documentation"
source: https://docs.snowflake.com/en/developer-guide/snowpark/reference/python/latest/modin/supported/window_supported
cert_domain: domain-2-data-preparation-and-feature-engineering
crawl_depth: 1
crawled: 2026-08-23
---

# Window supported APIs¶

The following table is structured as follows: The first column contains the method name. The second column is a flag for whether or not there is an implementation in Snowpark for the method in the left column.

Note

`Y` stands for yes, i.e., supports distributed implementation, `N` stands for no and API simply errors out, `P` stands for partial (meaning some parameters may not be supported yet), and `D` stands for defaults to single node pandas execution via UDF/Sproc. `engine` and `engine_kwargs` are always ignored in Snowpark pandas. The execution engine will always be Snowflake.

Rolling window functions

Rolling window functions | Snowpark implemented? (Y/N/P/D) | Notes for current implementation  
---|---|---  
`aggregate` | N |   
`apply` | N |   
`corr` | P | `N` for non-integer `window`, `axis = 1`, `pairwise = True`, `other = None`, or `min_periods != window`  
`count` | P | `N` for `axis = 1` or `min_periods = 0`. `N` for string `window` with `center = True`. `N` for Timedelta or BaseIndexer `window`.  
`cov` | N |   
`kurt` | N |   
`max` | P | `N` for `axis = 1` or `min_periods = 0`. `N` for string `window` with `center = True`. `N` for Timedelta or BaseIndexer `window`.  
`mean` | P | `N` for `axis = 1` or `min_periods = 0`. `N` for string `window` with `center = True`. `N` for Timedelta or BaseIndexer `window`.  
`median` | N |   
`min` | P | `N` for `axis = 1` or `min_periods = 0`. `N` for string `window` with `center = True`. `N` for Timedelta or BaseIndexer `window`.  
`quantile` | N |   
`rank` | N |   
`sem` | P | `N` for `axis = 1` or `min_periods = 0`. `N` for string `window` with `center = True`. `N` for Timedelta or BaseIndexer `window`.  
`skew` | N |   
`std` | P | `N` for `axis = 1` or `min_periods = 0`. `N` for string `window` with `center = True`. `N` for Timedelta or BaseIndexer `window`.  
`sum` | P | `N` for `axis = 1` or `min_periods = 0`. `N` for string `window` with `center = True`. `N` for Timedelta or BaseIndexer `window`.  
`var` | P | `N` for `axis = 1` or `min_periods = 0`. `N` for string `window` with `center = True`. `N` for Timedelta or BaseIndexer `window`.  
  
Weighted window functions

Weighted window functions | Snowpark implemented? (Y/N/P/D) | Notes for current implementation  
---|---|---  
`mean` | N |   
`std` | N |   
`sum` | N |   
`var` | N |   
  
Expanding window functions

Expanding window functions | Snowpark implemented? (Y/N/P/D) | Notes for current implementation  
---|---|---  
`aggregate` | N |   
`apply` | N |   
`corr` | N |   
`count` | P | `N` if `axis = 1`  
`cov` | N |   
`kurt` | N |   
`max` | P | `N` if `axis = 1`  
`mean` | P | `N` if `axis = 1`  
`median` | N |   
`min` | P | `N` if `axis = 1`  
`quantile` | N |   
`rank` | N |   
`sem` | P | `N` if `axis = 1`  
`skew` | N |   
`std` | P | `N` if `axis = 1` or `ddof` is not 0 or 1  
`sum` | P | `N` if `axis = 1`  
`var` | P | `N` if `axis = 1` or `ddof` is not 0 or 1  
  
Exponentially-weighted window functions

Exponential moving window functions | Snowpark implemented? (Y/N/P/D) | Notes for current implementation  
---|---|---  
`corr` | N |   
`cov` | N |   
`mean` | N |   
`std` | N |   
`sum` | N |   
`var` | N |   
  
Window indexer

Window Functions | Snowpark implemented? (Y/N/P/D) | Notes for current implementation  
---|---|---  
`api.indexers.BaseIndexer` | N |   
`api.indexers.FixedForwardWindowIndexer` | N |   
`api.indexers.VariableOffsetWindowIndexer` | N |
