---
title: "Manage procedures | Snowflake Documentation"
source: https://docs.snowflake.com/developer-guide/snowflake-rest-api/procedure/procedure-introduction
cert_domain: domain-3-model-development
crawl_depth: 1
crawled: 2026-08-23
---

# Manage procedures¶

The Snowflake REST [Procedure API](/developer-guide/snowflake-rest-api/reference/procedure.html) provides the following endpoints to access, update, and perform certain actions on Procedure resources.

**Snowflake REST Procedure API endpoints**

Endpoint| Description  
---|---  
`GET /api/v2/databases/_database_ /schemas/`  
`_schema_ /procedures`| Lists available procedures.  
`POST /api/v2/databases/_database_ /schemas/`  
`_schema_ /procedures`| Creates a procedure.  
`GET /api/v2/databases/_database_ /schemas/`  
`_schema_ /procedures/_nameWithArgs_`|  Fetches a procedure.  
`DELETE /api/v2/databases/_database_ /schemas/`  
`_schema_ /procedures/_nameWithArgs_`|  Deletes a procedure.  
`POST /api/v2/databases/_database_ /schemas/`  
`_schema_ /procedures/_nameWithArgs_ :call`| Calls a procedure.  
  
For reference documentation, see [Snowflake Procedure API reference](/developer-guide/snowflake-rest-api/reference/procedure.html).
