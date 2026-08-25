---
source_pdf: sources/SnowProDataScientistStudyGuide.pdf
part: "3.0"
keywords: hyperparameter tuning, cross validation, log loss, auc, rmse
---
# Hyperparameter Tuning and Validation (★★★)

#domain-3 #model-validation

## Overview Table

| Item | Key Point |
|---|---|
| Hyperparameter tuning | Search over settings not learned from data (max_depth, learning_rate, C); GridSearchCV / random search |
| Optimization metric selection | Must match objective: log loss (probabilistic classification), AUC (ranking/threshold-free), RMSE (regression) |
| Partitioning | k-fold CV vs train/validation/hold-out split |
| Imbalance handling | Down-sampling majority or up-sampling minority class |
| Validation outputs | ROC curve, confusion matrix → expected payout; residuals plot for regression |

## Metric Selection

| Metric | Task | What it measures | Sensitive to |
|---|---|---|---|
| Log loss (cross-entropy) | Classification with probabilities | Penalizes confident wrong predictions; uses predicted probabilities, not labels | Overconfidence — badly calibrated models crushed |
| AUC (ROC) | Binary classification ranking | Probability a random positive ranks above a random negative; **threshold-independent** | Class imbalance distorts intuition but not the score itself |
| RMSE | Regression | Root mean squared error in target units | **Outliers** (squared errors) |
| MAE | Regression | Mean absolute error | More outlier-robust than RMSE |
| Precision / Recall / F1 | Classification at chosen threshold | Trade-off FP vs FN; F1 = harmonic mean | Threshold choice |

> [!warning] Caveats
> - Optimize the metric aligned with business cost; accuracy is misleading under class imbalance.
> - AUC ignores actual probability calibration — a model can rank well yet have poor log loss.
> - RMSE penalizes large misses quadratically; for heavy-tailed targets consider MAE or transform target.

```python
from sklearn.model_selection import GridSearchCV, StratifiedKFold
from sklearn.linear_model import LogisticRegression

gs = GridSearchCV(
    LogisticRegression(),
    param_grid={"C": [0.01, 0.1, 1]},
    scoring="neg_log_loss",        # probabilistic classification
    cv=StratifiedKFold(n_splits=5),
)
gs.fit(X_train, y_train)
```

## Partitioning Strategies

| Strategy | How | Pros | Cons |
|---|---|---|---|
| Train/validation hold-out | e.g., 80/20 single split (+ separate test) | Fast, simple, preserves temporal ordering possible | High variance if small data; one split may be lucky/unlucky |
| k-fold CV | Split into k folds; each fold validates once; average scores | Uses all data for train+validate; lower variance estimate of generalization | k× training cost; leakage risk without proper pipelines per fold |
| Stratified k-fold | Preserve class ratios per fold | Better for imbalanced classification | Same cost as CV |
| Hold-out after CV | CV selects model/hyperparams; untouched hold-out gives final unbiased check | Clean separation of selection vs evaluation | Needs more data |

| Data situation | Recommended |
|---|---|
| Large dataset | Simple hold-out |
| Small dataset | k-fold (5–10) |
| Imbalanced classes | Stratified k-fold |
| Time-series | Forward-chaining/time-based splits (no future leakage) |

## Down/Up-Sampling (Imbalance)

| Technique | Action | Effect |
|---|---|---|
| Down-sampling | Drop majority-class rows | Faster; discards information |
| Up-sampling | Duplicate/augment minority rows (train set ONLY) | Balances loss; risks overfitting via duplication |
| Where applied | Fit resamplers on **training folds only** | Resample before split → leaky validation estimates |

## ROC Curve / Confusion Matrix / Expected Payout

```
Confusion matrix (counts):          Cost/revenue matrix ($ per cell):
              Pred P   Pred N                    Pred P    Pred N
 Actual P     TP       FN            Actual P    +100      -50   (treat saves $100, miss costs)
 Actual N     FP       TN            Actual N    -10        0    (outreach costs $10)
```

**Expected payout** = Σ (confusion-matrix cell × cost matrix cell). Example above:

`payout = 80·(+100) + 20·(−50) + 30·(−10) + 70·(0) = 8000 − 1000 − 300 = 6700`

| Term | Meaning |
|---|---|
| ROC curve | TPR vs FPR across all thresholds |
| AUC | Area under that curve (0.5 = random, 1.0 = perfect) |
| Threshold tuning | Move threshold along ROC to maximize payout, not accuracy |

## Regression Diagnostics: Residuals Plot

| Pattern in residuals vs fitted | Interpretation |
|---|---|
| Random scatter around 0 | Good fit — assumptions roughly satisfied |
| Curved/U-shape | Missing nonlinearity → add features/polynomials |
| Funnel shape (variance grows) | Heteroscedasticity → transform target, weighted least squares |
| Outliers far from 0 | Leverage points/influential observations → inspect data |

> [!warning] Context matters
> Always interpret residual plots with domain context: a "bad" pattern may reflect real-world regime change rather than model misspecification.

## Exam Patterns

| Scenario/Keyword | Answer |
|---|---|
| "predicted probability quality", calibration | Log loss |
| "threshold-independent ranking metric" | AUC |
| "regression error, outliers present" | RMSE (sensitive) / MAE (robust alternative) |
| "small dataset, reliable estimate" | k-fold cross validation |
| "final unbiased performance number" | Separate hold-out/test set never used in tuning |
| "imbalanced binary classes" | Stratified CV + up/down-sample train only + AUC/F1 over accuracy |
| "which customers to target for max profit" | Expected payout = confusion matrix × cost matrix |
| "residuals show U-shape" | Model misspecification — missing nonlinearity |

## Related Notes

- [model-training-pipelines](model-training-pipelines.md)
- [model-interpretation](model-interpretation.md)
- [ml-fundamentals](../01-data-science-concepts/ml-fundamentals.md)

## Source Documents

- SnowPro Advanced: Data Scientist Study Guide — Domain 3.3/3.4 objectives and resource list (`sources/snowpro-data-scientist-study-guide.txt`)
