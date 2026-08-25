---
source_pdf: sources/SnowProDataScientistStudyGuide.pdf
part: "1.0"
keywords: practice, ml fundamentals, statistics, ml lifecycle
---
# Data Science Concepts Practice (10 questions)
#practice #domain-1

## Related Concepts

- [ml-fundamentals](ml-fundamentals.md)
- [ml-lifecycle](ml-lifecycle-and-mlops.md)
- [statistics-for-data-science](statistics-for-data-science.md)

> [!hint]- Key patterns (click to expand)
> | Keyword | Answer |
> |---|---|
> | Labeled data → predict target | Supervised learning |
> | No labels → find groups | Unsupervised / clustering |
> | Rewards and penalties | Reinforcement learning |
> | Stage order | Collect → Explore → Feature Engineering → Train → Deploy → Monitor |
> | Not overbooked & predicted not overbooked | True Negative |
> | R² = 0.85 | 85% of response variability explained by linear association |
> | Mean vs median with outliers | Median is robust; mean pulled toward skew tail |
> | Sample means become normal | Central limit theorem |

> [!hint]- Key patterns (click to expand)
> | Keyword | Answer |
> |---|---|
> | Labeled (x,y), predict target | Supervised → regression / classification |
> | Unlabeled, find structure | Unsupervised → clustering |
> | Agent + rewards | Reinforcement |
> | Negative actual + negative prediction | True Negative |
> | Correct workload sequence | collect → visualize/explore → feature eng → train → deploy → monitor |
> | 85%% variability explained | R² = 0.85 (not slope) |
> | Unknown σ / small n | t-test (not z) |
> | Sample means CI | CLT + SE = σ/√n |
> | Resample with replacement | Bootstrap |


## Question 1 - Learning paradigms [recall]

> A model learns to play a game by receiving points for good moves and losing points for bad ones. Which ML paradigm?

> [!answer]- Show answer
> **Reinforcement learning.** An agent interacts with an environment and learns a policy that maximizes cumulative reward. Not supervised (no labeled examples) and not unsupervised (there IS a reward signal).

## Question 2 - Problem type mapping [application]

> A retailer wants to: (a) predict if a customer will churn yes/no, (b) group shoppers into segments without labels, (c) label every pixel of product photos. Name the three problem types.

> [!answer]- Show answer
> (a) Supervised — binary classification (structured). (b) Unsupervised — clustering. (c) Unstructured — image segmentation. Segmentation outputs per-pixel masks, unlike image classification which gives one label per image.

## Question 3 - GenAI association models [recall]

> You need to classify free-text support tickets into user-defined categories directly in SQL without training a custom model. Which Snowflake capability fits this GenAI association use case?

> [!answer]- Show answer
> Cortex AI function **`AI_CLASSIFY`** — classifies text or images into user-defined categories as a purpose-built managed LLM function. Related association functions: `AI_SENTIMENT`, `AI_EXTRACT`, `AI_EMBED`.

## Question 4 - Confusion matrix category [application]

> An airline model predicts flight overbooking. For one flight the flight was NOT overbooked and the model classified it as NOT overbooked. What cell of the confusion matrix is this? *(official sample question)*

> [!answer]- Show answer
> **True Negative.** Actual = negative (not overbooked), prediction = negative. FP would be predicted overbooked but wasn't; FN would be actually overbooked but predicted not; TP requires actual + predicted positive.

## Question 5 - R² interpretation [analysis]

> A linear regression model has R² = 0.85. Which interpretation is correct? *(official sample question)*
> A) Response changes ~0.85 per unit change in explanatory variable
> B) Repeating the model captures the true slope 85% of the time
> C) Strong positive correlation between variables
> D) 85% of variability in the response is explained by the linear association

> [!answer]- Show answer
> **D.** R² is the proportion of variance in the response explained by the model's linear relationship. A confuses slope/coefficient with R²; B describes a confidence-interval capture rate; C is wrong because high R² does not even imply positive direction (and correlation ≠ explained variance statement D makes).

## Question 6 - Skew and outliers [analysis]

> A dataset of customer account balances has a long right tail with a few extreme values. Which statement is correct?
> A) Mean > median; median better represents center
> B) Median > mean; mean is robust
> C) Mean equals median
> D) Outliers have no effect on the mean

> [!answer]- Show answer
> **A.** Right skew pulls the **mean above the median** because the mean is sensitive to outliers while the **median is robust**. Use median/IQR for skewed distributions.

## Question 7 - Lifecycle order [recall]

> Put in order: Model Training, Feature Engineering, Data Collection, Model Monitoring, Visualization/Exploration, Model Deployment. *(derived from official sample question)*

> [!answer]- Show answer
> **Data Collection > Data Visualization/Exploration > Feature Engineering > Training > Deployment > Monitoring.** Exploration precedes feature engineering; monitoring comes after deployment and feeds retraining loops.

## Question 8 - Central limit theorem [recall]

> Population delivery times are heavily right-skewed. You draw many samples of n=50 and average each. What shape is the distribution of those averages?

> [!answer]- Show answer
> Approximately **normal**, by the **central limit theorem**: for large enough n (~30+), the sampling distribution of the mean is normal regardless of population shape, centered at μ with standard error σ/√n.

## Question 9 - Z vs T test [recall]

> You test whether mean order value changed. σ is unknown and your sample has n=15 observations. Which test?

> [!answer]- Show answer
> **T test.** Unknown population standard deviation + small sample ⇒ t-distribution (heavier tails than normal). Z test requires σ known and typically larger samples.

## Question 10 - Bootstrapping and CI [recall]

> With only 20 records you must estimate a confidence interval for the median transaction value without assuming any distribution. Which technique?

> [!answer]- Show answer
> **Bootstrapping**: resample the 20 rows with replacement many times, compute the median per resample, take percentiles of that empirical distribution as the CI. Non-parametric, works for small samples and arbitrary statistics.

> [!summary]- Pattern summary (click to expand)
> | Keyword | Answer |
> |---|---|
> | Reward-driven agent | Reinforcement learning |
> | Yes/no target | Binary classification |
> | Pixel-level labels | Segmentation |
> | Text into categories via LLM function | `AI_CLASSIFY` (GenAI association) |
> | Negative actual + negative prediction | True Negative |
> | R² meaning | % of response variability explained by linear association |
> | Long right tail | Mean > median; prefer median |
> | Averages of large samples | Normal (CLT), SE = σ/√n |
> | σ unknown, small n | T test |
> | Resample with replacement for CI | Bootstrapping |
