---
title: "modin.pandas.DataFrame.unpin_backend | Snowflake Documentation"
source: https://docs.snowflake.com/en/developer-guide/snowpark/reference/python/latest/modin/pandas_api/modin.pandas.DataFrame.unpin_backend
cert_domain: domain-2-data-preparation-and-feature-engineering
crawl_depth: 1
crawled: 2026-08-23
---

# modin.pandas.DataFrame.unpin_backend¶

DataFrame.unpin_backend(_inplace : bool = False_) → Optional[Self][[source]](https://github.com/snowflakedb/snowpark-python/blob/v1.54.0/.tox/docs/lib/python3.10/site-packages/modin/core/storage_formats/pandas/query_compiler_caster.py#L245-L259)¶
    

Unpin the object’s underlying data, allowing Modin to automatically move it to another backend.

Parameters:
    

**inplace** (_bool_ _,__default: False_) – Whether to update the object in place.

Returns:
    

The newly-unpinned object, if inplace is False. Otherwise, None.

Return type:
    

Optional[Self]
