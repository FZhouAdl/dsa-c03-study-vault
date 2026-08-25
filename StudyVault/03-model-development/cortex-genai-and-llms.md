---
source_pdf: domain-3-model-development/snowflake-cortex-ai-functions-including-llm-functions-snowflake-docume.md
part: "3.0"
keywords: cortex, complete, embed_text, finetune, task-specific llm functions
---
# Cortex GenAI and LLMs (★★★)

#domain-3 #cortex-genai

## Overview Table

| Item | Key Point |
|---|---|
| Snowflake Cortex | Managed LLM/AI functions in SQL + Python; models run inside Snowflake security perimeter |
| Generative (COMPLETE) | Open-ended generation with a chosen model; also serves fine-tuned models |
| Vector embedding | EMBED_TEXT_768 → 768-dim VECTOR for similarity search / clustering / RAG |
| Task-specific functions | SUMMARIZE, sentiment, classification, extraction — no customization needed |
| Fine-tuning | CORTEX FINETUNE ('CREATE'/'SHOW'/'DESCRIBE'/'CANCEL'); PEFT adapters on base LLMs |
| Privileges | Role needs SNOWFLAKE.CORTEX_USER database role (or function-specific role) |

## Function Map

| Category | Function(s) | Purpose |
|---|---|---|
| Generation | AI_COMPLETE (`snowflake.cortex.Complete`) | Completion for text/image with selected model |
| Embedding | AI_EMBED; legacy EMBED_TEXT_768 | Vector embedding for similarity/RAG |
| Summarize | SUMMARIZE; AI_SUMMARIZE_AGG | Summary of text; _AGG summarizes across rows (no context-window limit) |
| Sentiment | AI_SENTIMENT | Extract sentiment from text |
| Classification | AI_CLASSIFY | Classify text/images into user-defined categories |
| Extraction | AI_EXTRACT | Extract entities/fields (information extraction) |
| Filtering | AI_FILTER | True/False predicate in SELECT/WHERE/JOIN |
| Aggregation insight | AI_AGG | Insights across a text column |
| Helpers | PROMPT, AI_COUNT_TOKENS, TO_FILE | Build prompts, count tokens, reference files |

```sql
SELECT SNOWFLAKE.CORTEX.SUMMARIZE(review_content) FROM reviews LIMIT 10;

SELECT SNOWFLAKE.CORTEX.EMBED_TEXT_768('snowflake-arctic-embed-m-v1.5', :text);

SELECT SNOWFLAKE.CORTEX.COMPLETE('mistral-7b', 'Summarize this churn risk...');
-- fine-tuned model used by name:
SELECT SNOWFLAKE.CORTEX.COMPLETE('my_tuned_model', 'How to fine-tune mistral models');
```

> [!warning] Naming drift
> Docs now present `AI_*` functions (AI_COMPLETE, AI_EMBED, AI_CLASSIFY...) as the canonical surface; `SNOWFLAKE.CORTEX.*` legacy names (e.g., EMBED_TEXT_768, deprecated end of 2026) still appear in exam-era material. Know both names map to same capability.

## Prompt Engineering

| Item | Key Point |
|---|---|
| PROMPT() helper | Builds prompt objects for COMPLETE |
| Structure | Instruction + context + examples; structured outputs enforce schema |
| Cost | Input AND output tokens billed for COMPLETE |

## Vector Embedding & RAG

| Item | Key Point |
|---|---|
| Output type | VECTOR (EMBED_TEXT_768 = 768 dimensions, English text) |
| Models | `snowflake-arctic-embed-m-v1.5`, `snowflake-arctic-embed-m`, `e5-base-v2` |
| Use | Similarity search, clustering, classification, retrieval for RAG pipelines |

## Fine-Tuning (CORTEX FINETUNE)

| Item | Key Point |
|---|---|
| Method | Parameter-efficient fine-tuning (PEFT): creates adapter on pre-trained model |
| When vs prompting/RAG | Need better latency/results than prompt engineering or RAG can give; avoid training from scratch |
| Data requirement | Query result must contain columns named **`prompt`** and **`completion`** (alias if needed); other columns ignored |
| Operations | FINETUNE('CREATE', name, base_model, train_query, [validation_query]); 'SHOW'; 'DESCRIBE'; 'CANCEL' |
| Base models | llama3/llama3.1 8b & 70b, mistral-7b, mixtral-8x7b |
| Cost | Trained tokens = input tokens × epochs; plus storage + warehouse; usage view CORTEX_FINE_TUNING_USAGE_HISTORY |
| Inference | Call via COMPLETE with fine-tuned model name |
| Artifacts | `training_results.csv` (step, epoch, training_loss, validation_loss) in Model Registry |
| Limits | Jobs long-running, account-level listing; cross-region inference NOT supported for fine-tuned models |

```sql
SELECT SNOWFLAKE.CORTEX.FINETUNE(
  'CREATE',
  'my_tuned_model',
  'mistral-7b',
  'SELECT a AS prompt, d AS completion FROM train',
  'SELECT a AS prompt, d AS completion FROM validation'
);
```

> [!warning] Simplified claims
> - Fine-tuning ≠ retraining all weights: PEFT trains lightweight adapters.
> - "Better than RAG" is not universal — docs position fine-tuning as an option when prompting/RAG underdeliver; domain knowledge injection often still pairs with retrieval.
> - Row-count limits depend on base model and epochs (e.g., mistral-7b ≈ 15k rows at default 3 epochs).

## Exam Patterns

| Scenario/Keyword | Answer |
|---|---|
| "summary of each review row" | SUMMARIZE (AI_SUMMARIZE_AGG for whole column) |
| "768-dim vector", "similarity search" | EMBED_TEXT_768 / AI_EMBED |
| "classify into my categories" | AI_CLASSIFY (task-specific categorization) |
| "extract fields/entities" | AI_EXTRACT |
| "positive/negative tone of tickets" | AI_SENTIMENT |
| "customize LLM to my task/domain style" | CORTEX FINETUNE; data must have prompt/completion columns |
| "use fine-tuned model" | COMPLETE('<tuned_model_name>', ...) |
| Required privilege for any Cortex function | SNOWFLAKE.CORTEX_USER database role |

## Related Notes

- [connecting-tools-to-snowflake](connecting-tools-to-snowflake.md)
- [model-development-practice](model-development-practice.md)

## Source Documents

- [Snowflake Cortex AI Functions (including LLM functions)](../../sources/downloads/domain-3-model-development/snowflake-cortex-ai-functions-including-llm-functions-snowflake-docume.md)
- [Fine-tuning (Snowflake Cortex)](../../sources/downloads/domain-3-model-development/fine-tuning-snowflake-cortex-snowflake-documentation.md)
- [EMBED_TEXT_768 (SNOWFLAKE.CORTEX)](../../sources/downloads/domain-3-model-development/embed-text-768-snowflake-cortex-snowflake-documentation.md)
- [SUMMARIZE (SNOWFLAKE.CORTEX)](../../sources/downloads/domain-3-model-development/summarize-snowflake-cortex-snowflake-documentation.md)
