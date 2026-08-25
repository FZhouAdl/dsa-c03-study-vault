---
title: "<model_name>!SHOW_CONFUSION_MATRIX | Snowflake Documentation"
source: https://docs.snowflake.com/sql-reference/classes/classification/methods/show_confusion_matrix
cert_domain: domain-4-model-deployment
crawl_depth: 1
crawled: 2026-08-23
---

# <model_name>!SHOW_CONFUSION_MATRIX¶

Returns a table containing the number of instances of each combination of actual class and predicted class in models where evaluation was enabled at instantiation. You can use this dataset to plot a confusion matrix. This method takes no arguments. See [Confusion Matrix in `show_confusion_matrix`](/user-guide/ml-functions/classification#label-cortex-classification-show-confusion-matrix).

## Output¶

Column| Type| Description  
---|---|---  
`dataset_type`| [VARCHAR](/sql-reference/data-types-text#label-character-datatypes)| The name of the dataset used for metrics calculation, currently EVAL.  
`actual_class`| [VARCHAR](/sql-reference/data-types-text#label-character-datatypes)| The actual class.  
`predicted_class`| [VARCHAR](/sql-reference/data-types-text#label-character-datatypes)| The predicted class.  
`count`| [INTEGER](/sql-reference/data-types-numeric#label-data-type-integer)| The number of instances of the given combination of actual and predicted class.  
`logs`| [VARIANT](/sql-reference/data-types-semistructured#label-data-type-variant)| Contains error or warning messages.
