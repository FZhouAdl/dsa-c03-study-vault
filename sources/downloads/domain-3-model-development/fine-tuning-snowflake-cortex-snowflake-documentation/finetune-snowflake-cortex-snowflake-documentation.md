---
title: "FINETUNE (SNOWFLAKE.CORTEX) | Snowflake Documentation"
source: https://docs.snowflake.com/sql-reference/functions/finetune-snowflake-cortex
cert_domain: domain-3-model-development
crawl_depth: 1
crawled: 2026-08-23
---

Categories:
    

[String & binary functions](/sql-reference/functions-string) (AI Functions)

# FINETUNE (SNOWFLAKE.CORTEX)¶

This function lets you create and manage large language models customized for your specific task.

## Syntax¶
[code] 
    FINETUNE (
      { 'CREATE' | 'SHOW' | 'DESCRIBE' | 'CANCEL' }
      ...
      )
    
[/code]

The syntax varies considerably between the different commands. For specific syntax, usage notes, and examples, see:

  * [FINETUNE (‘CREATE’) (SNOWFLAKE.CORTEX)](/sql-reference/functions/finetune-create)
  * [FINETUNE (‘DESCRIBE’) (SNOWFLAKE.CORTEX)](/sql-reference/functions/finetune-describe)
  * [FINETUNE (‘SHOW’) (SNOWFLAKE.CORTEX)](/sql-reference/functions/finetune-show)
  * [FINETUNE (‘CANCEL’) (SNOWFLAKE.CORTEX)](/sql-reference/functions/finetune-cancel)



## Access control requirements¶

For access requirements, see [Access control requirements](/user-guide/snowflake-cortex/cortex-finetuning#label-cortex-finetune-privileges).
