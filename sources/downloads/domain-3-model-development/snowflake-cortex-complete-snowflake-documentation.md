---
title: "snowflake.cortex.Complete | Snowflake Documentation"
source: https://docs.snowflake.com/en/developer-guide/snowpark-ml/reference/1.6.2/api/model/snowflake.cortex.Complete
cert_domain: domain-3-model-development
crawl_depth: 0
crawled: 2026-08-23
---

You are viewing documentation about an older version (1.6.2).  [View latest version](/en/developer-guide/snowpark-ml/reference/1.52.0/index)

# snowflake.cortex.Complete¶

snowflake.cortex.Complete(_model : Union[str, Column]_, _prompt : Union[str, List[ConversationMessage], Column]_, _*_ , _options : Optional[[CompleteOptions](snowflake.cortex.CompleteOptions.html#snowflake.cortex.CompleteOptions "snowflake.cortex._complete.CompleteOptions")] = None_, _session : Optional[Session] = None_, _stream : bool = False_, _timeout : Optional[float] = None_, _deadline : Optional[float] = None_) → Union[str, Iterator[str], Column]¶
    

Complete calls into the LLM inference service to perform completion.

Parameters:
    

  * **model** – A Column of strings representing model types.

  * **prompt** – A Column of prompts to send to the LLM.

  * **options** – A instance of snowflake.cortex.CompleteOptions

  * **session** – The snowpark session to use. Will be inferred by context if not specified.

  * **stream** (_bool_) – Enables streaming. When enabled, a generator function is returned that provides the streaming output as it is received. Each update is a string containing the new text content since the previous update.

  * **timeout** (_float_) – Timeout in seconds to retry failed REST requests.

  * **deadline** (_float_) – Time in seconds since the epoch (as returned by time.time()) to retry failed REST requests.



Raises:
    

**ValueError** – incorrect argument.

Returns:
    

A column of string responses.
