---
title: "FINETUNE ('CANCEL') (SNOWFLAKE.CORTEX) | Snowflake Documentation"
source: https://docs.snowflake.com/sql-reference/functions/finetune-cancel
cert_domain: domain-3-model-development
crawl_depth: 1
crawled: 2026-08-23
---

Categories:
    

[String & binary functions](/sql-reference/functions-string) (AI Functions)

# FINETUNE (‘CANCEL’) (SNOWFLAKE.CORTEX)¶

Cancels the specified fine-tuning job from the current schema.

## Syntax¶
[code] 
    SNOWFLAKE.CORTEX.FINETUNE(
      'CANCEL',
      '<finetune_job_id>'
    )
    
[/code]

## Parameters¶

`'CANCEL'`
    

Specifies that you want to cancel a fine-tuning job.

`_finetune_job_id_`
    

The ID of the fine-tuning job that was generated when you created the job.

## Output¶

Column| Type| Description  
---|---|---  
SNOWFLAKE.CORTEX.FINETUNE| [STRING](/sql-reference/data-types-text#label-character-datatypes)| Message that the job was canceled.  
  
## Access control requirements¶

For access requirements, see [Access control requirements](/user-guide/snowflake-cortex/cortex-finetuning#label-cortex-finetune-privileges).

## Examples¶
[code] 
    SELECT SNOWFLAKE.CORTEX.FINETUNE(
      'CANCEL',
      'ft_194bbea4-1208-42f3-88c6-cfb202086125'
    );
    
[/code]
[code] 
    Canceled Cortex Fine-tuning job: ft_194bbea4-1208-42f3-88c6-cfb202086125
    
[/code]
