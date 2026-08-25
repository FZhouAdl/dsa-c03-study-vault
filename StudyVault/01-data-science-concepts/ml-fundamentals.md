---
source_pdf: sources/SnowProDataScientistStudyGuide.pdf
part: "1.0"
keywords: supervised learning, unsupervised learning, reinforcement learning, problem types, genai
---
# ML Fundamentals (★★★)
#domain-1 #ml-fundamentals

## Overview Table

| Item | Key Point |
|---|---|
| Machine Learning | Algorithms infer relationships between inputs and outputs instead of having rules stated explicitly up front |
| Supervised learning | Learns from **labeled data** (input → known target); regression and classification |
| Unsupervised learning | Finds structure in **unlabeled data**; e.g., clustering |
| Reinforcement learning | **Agent** learns by trial-and-error via **rewards/penalties** from an environment |
| Structured problems | Linear regression, binary/multi-class classification, time-series forecasting |
| Unstructured problems | Image classification, segmentation |
| Unsupervised problem | Clustering |
| GenAI problem | Association models |

## Learning Paradigms (Objective 1.1)

| Paradigm | Data | Goal | Examples |
|---|---|---|---|
| **Supervised** | Labeled pairs (x, y) | Predict y for new x | Churn prediction, price prediction, spam detection |
| **Unsupervised** | Unlabeled x only | Discover hidden structure | Customer segmentation, anomaly detection, dimensionality reduction |
| **Reinforcement** | Interaction with environment | Maximize cumulative reward | Game-playing agents, robotics, dynamic pricing |

Key distinctions:

- Supervised = there is a **ground-truth target** during training.
- Unsupervised = **no target**; algorithm must find pattern on its own.
- Reinforcement = no fixed dataset at all; learning signal is a **delayed reward**, not a label.

## Supervised Learning Problem Types (Objective 1.2)

### Structured Data

| Problem Type | Target | Example | Typical Metric |
|---|---|---|---|
| **Linear regression** | Continuous number | Predict house price from size/location | R², RMSE |
| **Binary classification** | One of 2 classes | Flight overbooked: yes/no; churn: yes/no | Precision, recall, confusion matrix, AUC |
| **Multi-class classification** | One of N classes | Classify support ticket into product area | Accuracy, per-class F1 |
| **Time-series forecasting** | Future continuous values | Demand/sales forecast next quarter | MAPE, MAE |

### Unstructured Data

| Problem Type | Input | Output |
|---|---|---|
| **Image classification** | Whole image | Single label ("cat" vs "dog") |
| **Segmentation** | Image pixels | Per-pixel class mask (object boundaries) |

> [!warning]
> Segmentation ≠ image classification: segmentation assigns a label to **every pixel** (region-level output), not one label per image. Exam scenarios mentioning pixel-level or boundary outlines point to segmentation.

## Unsupervised & GenAI Problem Types

- **Clustering**: group similar rows without labels (e.g., customer segments via k-means). Keyword cues: *segments*, *groups*, *no labeled outcome*.
- **GenAI – Association models**: models that associate/generate content across modalities — text classification, sentiment, entity extraction, summarization. In Snowflake these map to Cortex AI task-specific functions such as `AI_CLASSIFY` (text/images into user-defined categories), `AI_SENTIMENT`, `AI_EXTRACT`, `AI_EMBED` (embedding vectors usable for similarity search, clustering, classification), and `AI_COMPLETE`.

> [!warning]
> `AI_EMBED` produces vectors that can feed clustering/classification, but the Cortex AI Functions themselves are managed, purpose-built functions requiring no custom training — do not confuse them with training a supervised model from scratch.

## Exam Patterns

| Scenario/Keyword | Answer |
|---|---|
| Labeled historical outcomes, predict yes/no | Supervised – binary classification |
| Predict continuous value (price, revenue) | Linear regression (structured) |
| Forecast future values over time | Time-series forecasting |
| Pixel-level labeling of images | Segmentation |
| Group customers, no labels | Unsupervised – clustering |
| Agent learns from rewards/penalties | Reinforcement learning |
| Classify text into categories with LLM function | GenAI association model (`AI_CLASSIFY`) |
| One label among N classes | Multi-class classification |

## Related Notes

- [ml-lifecycle](ml-lifecycle-and-mlops.md)
- [statistics-for-data-science](statistics-for-data-science.md)
- [practice questions](data-science-concepts-practice.md)

## Source Documents

- [snowpro-data-scientist-study-guide.txt](../../sources/snowpro-data-scientist-study-guide.txt) (Domain 1.0 objectives, lines 134–180)
- [Snowflake Cortex AI Functions (including LLM functions)](../../sources/downloads/domain-1-data-science-concepts/snowflake-cortex-ai-functions-including-llm-functions-snowflake-docume.md)
