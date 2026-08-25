---
source_pdf: sources/SnowProDataScientistStudyGuide.pdf
part: "1.0"
keywords: normal distribution, skewness, central limit theorem, hypothesis testing, bootstrapping
---
# Statistics for Data Science (★★★)
#domain-1 #statistics

## Overview Table

| Item | Key Point |
|---|---|
| Normal distribution | Symmetric, bell-shaped; mean = median = mode |
| Skewed distribution | Asymmetric tail; **mean pulled toward tail**, median more robust |
| Central limit theorem | Sample means approach normal as n grows, regardless of population shape |
| Z test | Known population σ, large n |
| T test | Unknown σ, small n |
| Bootstrapping | Resample with replacement to estimate a statistic's distribution |
| Confidence interval | Range of plausible values for a parameter at a confidence level |

## Normal vs Skewed Distributions (Objective 1.4)

| Property | Normal | Right-skewed | Left-skewed |
|---|---|---|---|
| Shape | Symmetric bell | Long right tail | Long left tail |
| Mean vs median | Mean = median | Mean > median | Mean < median |
| Outlier effect | Balanced | Outliers inflate mean | Low outliers drag mean |

- In skewed data the **mean is pulled toward the tail**; the **median is robust to outliers**.
- Report median/IQR for heavily skewed data (e.g., income, latency).

```
Normal:            Right-skewed:
      ___                 __
    /     \              |  \__
   /       \             |     \___
__/_________\__        __|________\____
   ^mean^median          ^median ^mean
```

> [!warning]
> "Mean is the best measure of center" only holds for roughly symmetric distributions. Any exam scenario with outliers/skew pointing at the *mean* being distorted should steer you to the **median**.

## Central Limit Theorem

- For i.i.d. samples with sufficiently large n, the distribution of the **sample mean** ≈ normal, even if the population is not.
- Sampling distribution has mean μ and standard error **σ/√n**.
- Practical threshold rule of thumb: n ≥ 30.
- Justifies using normal-based intervals/tests for means on large samples.

## Z Test vs T Test

| Feature | Z test | T test |
|---|---|---|
| Population std dev | **Known** | **Unknown** (use sample s) |
| Sample size | Large | Small |
| Distribution | Standard normal | t-distribution (heavier tails) |

- Both test hypotheses about means; the choice hinges on whether σ is known and sample size.
- Tails of the t-distribution are fatter → wider intervals, more conservative conclusions with small samples.

> [!warning]
> Real-world σ is almost never truly known — in practice you almost always use a t test. Exam questions usually signal Z by explicitly stating "population standard deviation known" plus a large sample.

## Bootstrapping

- Repeatedly resample the observed data **with replacement** (same size as original), many times.
- Compute the statistic (mean, median, etc.) per resample → build its empirical distribution.
- Use cases: CI without parametric assumptions; works well when n is small or distribution unknown.

```
Original sample [a b c d]
Resample 1: [a c c d] -> stat 1
Resample 2: [b b d a] -> stat 2
... x1000 -> percentile interval from stats
```

## Confidence Intervals

- A 95% CI means: if sampling were repeated many times, ~95% of such intervals would contain the true parameter.
- Common form: estimate ± z* · (σ/√n), e.g., ±1.96·SE for 95%.
- Wider interval = more uncertainty; increases with confidence level, decreases with larger n.

> [!warning]
> A 95% CI does NOT mean "there is a 95% probability this specific interval contains the parameter." It describes the long-run capture rate of the procedure.

## Exam Patterns

| Scenario/Keyword | Answer |
|---|---|
| Income data, few extreme high values, best center measure | Median (mean pulled by skew) |
| Sample means become normal for any population | Central limit theorem |
| Population σ known, large n | Z test |
| Small sample, σ unknown | T test |
| Estimate CI by resampling with replacement | Bootstrapping |
| Interpret 95% CI | Procedure captures true parameter in ~95% of repeated samples |
| Standard error of sample mean | σ/√n |

## Related Notes

- [ml-fundamentals](ml-fundamentals.md)
- [ml-lifecycle](ml-lifecycle-and-mlops.md)
- [practice questions](data-science-concepts-practice.md)

## Source Documents

- [snowpro-data-scientist-study-guide.txt](../../sources/snowpro-data-scientist-study-guide.txt) (Domain 1.0 objectives lines 134–180)
