---
title: "Differential privacy functions | Snowflake Documentation"
source: https://docs.snowflake.com/sql-reference/functions-differential-privacy
cert_domain: domain-2-data-preparation-and-feature-engineering
crawl_depth: 1
crawled: 2026-08-23
---

# Differential privacy functions¶

The following functions are associated with [differential privacy](/user-guide/diff-privacy/differential-privacy-overview).

Function| Description  
---|---  
[DP_INTERVAL_LOW](/sql-reference/functions/dp_interval_low)| Returns the lower bound of the [noise interval](/user-guide/diff-privacy/differential-privacy-analyst#label-diff-privacy-understand-results).  
[DP_INTERVAL_HIGH](/sql-reference/functions/dp_interval_high)| Returns the upper bound of the [noise interval](/user-guide/diff-privacy/differential-privacy-analyst#label-diff-privacy-understand-results).  
[ESTIMATE_REMAINING_DP_AGGREGATES](/sql-reference/functions/estimate_remaining_dp_aggregates)| Returns the estimated remaining number of aggregation function calls in the current user’s [privacy budget](/user-guide/diff-privacy/differential-privacy-admin-privacy-budgets).
