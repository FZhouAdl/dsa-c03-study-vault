---
source_pdf: sources/SnowProDataScientistStudyGuide.pdf
part: "3.0"
keywords: domain-3 practice, udf udtf stored procedure, metrics, cortex functions
---
# Model Development Practice (Domain 3) (★★★)

#practice #domain-3

## Related Concepts

- [connecting-tools-to-snowflake](connecting-tools-to-snowflake.md)
- [cortex-genai-and-llms](cortex-genai-and-llms.md)
- [model-training-pipelines](model-training-pipelines.md)
- [hyperparameter-tuning-and-validation](hyperparameter-tuning-and-validation.md)
- [model-interpretation](model-interpretation.md)

---

> [!hint]- Key patterns (click to expand)
> | Keyword | Answer |
> | Multi-statement orchestration / DML | Stored procedure |
> | Scalar per-row Python | UDF |
> | Tabular per-partition output | UDTF |
> | Pushdown DataFrame compute | Snowpark |
> | pandas-batch inference | Vectorized UDF |
> | Generation / summary / embedding / classify | COMPLETE / SUMMARIZE / EMBED_TEXT_768 / AI_CLASSIFY |
> | Probabilistic classification metric | log loss |
> | Threshold-independent ranking | AUC |
> | Regression fit metric | RMSE |
> | Change detection for automation | SYSTEM$STREAM_HAS_DATA + task |


## Q1 — Connector Pandas API (Recall)

Which Python Connector cursor methods return a **pandas DataFrame**?

> [!hint]- Hint
> Two methods: one fetches everything, one streams chunks.

> [!answer]- Show answer
> `fetch_pandas_all()` and `fetch_pandas_batches()`. Writing back uses `write_pandas()` or `to_sql(..., method=pd_writer)`. Requires installing `"snowflake-connector-python[pandas]"` (PyArrow).

---

## Q2 — Snowpark Pushdown (Recall)

You run `session.table("t").filter(col("x") > 0).group_by("g").mean()` from a laptop. Where does the heavy compute happen?

> [!hint]- Hint
> Lazy DataFrame → compiled SQL.

> [!answer]- Show answer
> In the Snowflake warehouse. Snowpark compiles DataFrame operations to SQL (pushdown); only results return to the client. This is the key difference from the raw Python connector pulling data locally.

---

## Q3 — SP vs UDF Scenario (Recall)

A data scientist needs a scheduled job that trains an sklearn model, writes model artifacts to a stage, and updates a training-log table. Which construct?

> [!hint]- Hint
> Multi-statement orchestration + DML.

> [!answer]- Show answer
> Python **stored procedure** (on a Snowpark-optimized warehouse), invoked via `CALL`, optionally wrapped in a CREATE TASK schedule. Procedures allow DDL/DML and multi-step logic; UDFs cannot write tables.

---

## Q4 — UDTF Definition (Recall)

What distinguishes a Python UDTF from a scalar UDF?

> [!hint]- Hint
> Rows in, rows out — grouped how?

> [!answer]- Show answer
> A UDTF receives a **partition of rows** and returns a **table (set of rows)** per partition; it's called in the FROM clause via `TABLE(func(...) OVER (PARTITION BY ...))`. Scalar UDFs return one value per input row inside expressions.

---

## Q5 — Cortex Function ID: Summarize (Recall)

Column `ticket_text` needs a short summary per row in pure SQL. Which function?

> [!hint]- Hint
> Task-specific managed function, no prompt needed.

> [!answer]- Show answer
> `SNOWFLAKE.CORTEX.SUMMARIZE(ticket_text)`. (COMPLETE would also work but requires choosing a model + crafting a prompt; SUMMARIZE is the purpose-built task-specific function.)

---

## Q6 — Embedding Vector (Recall)

What does `SNOWFLAKE.CORTEX.EMBED_TEXT_768('snowflake-arctic-embed-m-v1.5', text)` return, and name two use cases?

> [!hint]- Hint
> Type VECTOR; think similarity.

> [!answer]- Show answer
> A VECTOR of **768 dimensions** (English text). Uses: similarity/semantic search, clustering, classification, RAG retrieval. Requires SNOWFLAKE.CORTEX_USER role.

---

## Q7 — Fine-Tuning Data Shape (Recall)

Cortex FINETUNE errors on your training query. What column names must the result contain?

> [!hint]- Hint
> Input/output pair naming.

> [!answer]- Show answer
> Columns named **`prompt`** and **`completion`** (use aliases if needed). All other columns are ignored. Fine-tuning creates PEFT adapters to specialize a base LLM when prompting/RAG underperforms; inference later runs through COMPLETE('<tuned_model>', ...).

---

## Q8 — Task Guard (Recall)

Why add `WHEN SYSTEM$STREAM_HAS_DATA('s')` to a task that consumes stream `s`? And what state are new tasks born in?

> [!hint]- Hint
> Cost control; suspended.

> [!answer]- Show answer
> The guard skips the run when the stream has no CDC records, avoiding unnecessary warehouse start/resume costs. New (or cloned) tasks are created **suspended** and must be resumed with ALTER TASK ... RESUME. Note the function can produce false positives — consume the stream regardless.

---

## Q9 — Metric Selection: Probabilities Matter (Application)

Churn model feeds a budget optimizer that needs well-calibrated probabilities. Which optimization metric during GridSearchCV: accuracy, AUC, or log loss? Why are the others wrong here?

> [!hint]- Hint
> Confident-but-wrong predictions should hurt.

> [!answer]- Show answer
> **Log loss** — it scores predicted probabilities directly and heavily penalizes confident mistakes. Accuracy ignores probability quality entirely; AUC measures ranking/threshold-independent discrimination and says nothing about calibration.

---

## Q10 — Expected Payout Calculation (Application)

Confusion matrix (counts) and $ impact:

| | Predict Treat | Predict Skip |
|---|---|---|
| Actual responder | TP=80 | FN=20 |
| Actual non-responder | FP=30 | TN=70 |

Cost matrix ($): treat a true responder = **+100**; miss (FN) = **−50**; wasted treatment (FP) = **−10**; correct skip = **0**. Compute expected payout and say whether lowering the threshold toward more TP is likely profitable if each new TP adds ~$100 and converts some TN→FP (−$10).

> [!hint]- Hint
> Payout = Σ count × value.

> [!answer]- Show answer
> Payout = 80(100) + 20(−50) + 30(−10) + 70(0) = 8000 − 1000 − 300 = **$6,700**. Lowering threshold trades TN for TP at roughly +100 −10 = +90 net per conversion while possibly adding FPs at −10 — marginally profitable as long as gained cells are mostly TPs; recompute the full matrix at the new threshold before deciding.

---

## Q11 — CV vs Hold-Out + Leakage (Analysis)

Team has 4k labeled rows (5% positive). They up-sample the minority class **before** running 5-fold CV, report AUC = 0.95, then deploy. Identify two methodological problems and fixes.

> [!hint]- Hint
> Where did resampling see validation rows? What does 20% hold-out variance look like?

> [!answer]- Show answer
> (1) **Leakage**: resampling before splitting duplicates minority rows into both train and validation folds, inflating AUC. Fix: put the resampler inside the pipeline so it fits on training folds only (e.g., imblearn Pipeline), and use **stratified** folds. (2) Small/imbalanced data makes any single hold-out noisy — k-fold gives a lower-variance estimate, but keep an untouched final hold-out for unbiased confirmation. Also prefer AUC/F1 over accuracy given 5% positives.

---

## Q12 — Residual Plot Diagnosis (Analysis)

Regression predicting delivery times. Residuals-vs-fitted shows clear U-shape and widening spread at high fitted values. Two diagnoses and two remedies.

> [!hint]- Hint
> Non-random pattern ≠ good model.

> [!answer]- Show answer
> Diagnoses: (1) **misspecification** — a nonlinear relationship not captured (U-shape); (2) **heteroscedasticity** — error variance grows with prediction level (funnel). Remedies: add nonlinear terms/features or switch to a more flexible model (trees/GAM); transform the target (log) or fit weighted/heteroscedasticity-robust models. Interpret with context first — verify the pattern isn't a real operational regime change.

---

## Coverage Summary

| # | Level | Topic |
|---|---|---|
| 1–8 | Recall | Connector pandas API, pushdown, SP/UDTF choice, SUMMARIZE, EMBED_TEXT_768, FINETUNE format, task guard |
| 9–10 | Application | Log loss selection, expected payout math |
| 11–12 | Analysis | Leakage/CV trade-offs, residual diagnostics |

> [!summary]- Session summary
> Domain 3 spans connectivity (connector vs Snowpark vs Snowpark ML), GenAI (Cortex COMPLETE/SUMMARIZE/EMBED/FINETUNE + task-specific functions), pipelines (SP vs UDF vs UDTF, dynamic tables/tasks/streams, vectorized UDFs), validation (log loss/AUC/RMSE, CV vs hold-out, sampling, expected payout, residuals), interpretation (SHAP/PDP/CI/feature impact). Weakest area? Re-drill corresponding concept note.
