---
title: "snowflake.ml.modeling.decomposition.DictionaryLearning | Snowflake Documentation"
source: https://docs.snowflake.com/en/developer-guide/snowpark-ml/reference/1.6.1/api/modeling/snowflake.ml.modeling.decomposition.DictionaryLearning.html
cert_domain: domain-2-data-preparation-and-feature-engineering
crawl_depth: 1
crawled: 2026-08-23
---

You are viewing documentation about an older version (1.6.1).  [View latest version](/en/developer-guide/snowpark-ml/reference/1.52.0/index)

# snowflake.ml.modeling.decomposition.DictionaryLearning¶

_class _snowflake.ml.modeling.decomposition.DictionaryLearning(_*_ , _n_components =None_, _alpha =1_, _max_iter =1000_, _tol =1e-08_, _fit_algorithm ='lars'_, _transform_algorithm ='omp'_, _transform_n_nonzero_coefs =None_, _transform_alpha =None_, _n_jobs =None_, _code_init =None_, _dict_init =None_, _callback =None_, _verbose =False_, _split_sign =False_, _random_state =None_, _positive_code =False_, _positive_dict =False_, _transform_max_iter =1000_, _input_cols : Optional[Union[str, Iterable[str]]] = None_, _output_cols : Optional[Union[str, Iterable[str]]] = None_, _label_cols : Optional[Union[str, Iterable[str]]] = None_, _passthrough_cols : Optional[Union[str, Iterable[str]]] = None_, _drop_input_cols : Optional[bool] = False_, _sample_weight_col : Optional[str] = None_)¶
    

Bases: `BaseTransformer`

Dictionary learning For more details on this class, see [sklearn.decomposition.DictionaryLearning](https://scikit-learn.org/stable/modules/generated/sklearn.decomposition.DictionaryLearning.html)

Parameters:
    

  * **input_cols** (_Optional_ _[__Union_ _[__str_ _,__List_ _[__str_ _]__]__]_) – A string or list of strings representing column names that contain features. If this parameter is not specified, all columns in the input DataFrame except the columns specified by label_cols, sample_weight_col, and passthrough_cols parameters are considered input columns. Input columns can also be set after initialization with the set_input_cols method.

  * **label_cols** (_Optional_ _[__Union_ _[__str_ _,__List_ _[__str_ _]__]__]_) – A string or list of strings representing column names that contain labels. Label columns must be specified with this parameter during initialization or with the set_label_cols method before fitting.

  * **output_cols** (_Optional_ _[__Union_ _[__str_ _,__List_ _[__str_ _]__]__]_) – A string or list of strings representing column names that will store the output of predict and transform operations. The length of output_cols must match the expected number of output columns from the specific predictor or transformer class used. If you omit this parameter, output column names are derived by adding an OUTPUT_ prefix to the label column names for supervised estimators, or OUTPUT_<IDX>for unsupervised estimators. These inferred output column names work for predictors, but output_cols must be set explicitly for transformers. In general, explicitly specifying output column names is clearer, especially if you don’t specify the input column names. To transform in place, pass the same names for input_cols and output_cols. be set explicitly for transformers. Output columns can also be set after initialization with the set_output_cols method.

  * **sample_weight_col** (_Optional_ _[__str_ _]_) – A string representing the column name containing the sample weights. This argument is only required when working with weighted datasets. Sample weight column can also be set after initialization with the set_sample_weight_col method.

  * **passthrough_cols** (_Optional_ _[__Union_ _[__str_ _,__List_ _[__str_ _]__]__]_) – A string or a list of strings indicating column names to be excluded from any operations (such as train, transform, or inference). These specified column(s) will remain untouched throughout the process. This option is helpful in scenarios requiring automatic input_cols inference, but need to avoid using specific columns, like index columns, during training or inference. Passthrough columns can also be set after initialization with the set_passthrough_cols method.

  * **drop_input_cols** (_Optional_ _[__bool_ _]__,__default=False_) – If set, the response of predict(), transform() methods will not contain input columns.

  * **n_components** (_int_ _,__default=None_) – Number of dictionary elements to extract. If None, then `n_components` is set to `n_features`.

  * **alpha** (_float_ _,__default=1.0_) – Sparsity controlling parameter.

  * **max_iter** (_int_ _,__default=1000_) – Maximum number of iterations to perform.

  * **tol** (_float_ _,__default=1e-8_) – Tolerance for numerical error.

  * **fit_algorithm** (_{'lars'__,__'cd'}__,__default='lars'_) – 

    * ‘lars’: uses the least angle regression method to solve the lasso problem (`lars_path()`);

    * ’cd’: uses the coordinate descent method to compute the Lasso solution (`Lasso`). Lars will be faster if the estimated components are sparse.

  * **transform_algorithm** (_{'lasso_lars'__,__'lasso_cd'__,__'lars'__,__'omp'__,__'threshold'}__,__default='omp'_) – 

Algorithm used to transform the data:

    * ’lars’: uses the least angle regression method (`lars_path()`);

    * ’lasso_lars’: uses Lars to compute the Lasso solution.

    * ’lasso_cd’: uses the coordinate descent method to compute the Lasso solution (`Lasso`). ‘lasso_lars’ will be faster if the estimated components are sparse.

    * ’omp’: uses orthogonal matching pursuit to estimate the sparse solution.

    * ’threshold’: squashes to zero all coefficients less than alpha from the projection `dictionary * X'`.

  * **transform_n_nonzero_coefs** (_int_ _,__default=None_) – Number of nonzero coefficients to target in each column of the solution. This is only used by algorithm=’lars’ and algorithm=’omp’. If None, then transform_n_nonzero_coefs=int(n_features / 10).

  * **transform_alpha** (_float_ _,__default=None_) – If algorithm=’lasso_lars’ or algorithm=’lasso_cd’, alpha is the penalty applied to the L1 norm. If algorithm=’threshold’, alpha is the absolute value of the threshold below which coefficients will be squashed to zero. If None, defaults to alpha.

  * **n_jobs** (_int_ _or_ _None_ _,__default=None_) – Number of parallel jobs to run. `None` means 1 unless in a `joblib.parallel_backend` context. `-1` means using all processors. See Glossary for more details.

  * **code_init** (_ndarray of shape_ _(__n_samples_ _,__n_components_ _)__,__default=None_) – Initial value for the code, for warm restart. Only used if code_init and dict_init are not None.

  * **dict_init** (_ndarray of shape_ _(__n_components_ _,__n_features_ _)__,__default=None_) – Initial values for the dictionary, for warm restart. Only used if code_init and dict_init are not None.

  * **callback** (_callable_ _,__default=None_) – Callable that gets invoked every five iterations.

  * **verbose** (_bool_ _,__default=False_) – To control the verbosity of the procedure.

  * **split_sign** (_bool_ _,__default=False_) – Whether to split the sparse feature vector into the concatenation of its negative part and its positive part. This can improve the performance of downstream classifiers.

  * **random_state** (_int_ _,__RandomState instance_ _or_ _None_ _,__default=None_) – Used for initializing the dictionary when `dict_init` is not specified, randomly shuffling the data when `shuffle` is set to `True`, and updating the dictionary. Pass an int for reproducible results across multiple function calls. See Glossary.

  * **positive_code** (_bool_ _,__default=False_) – Whether to enforce positivity when finding the code.

  * **positive_dict** (_bool_ _,__default=False_) – Whether to enforce positivity when finding the dictionary.

  * **transform_max_iter** (_int_ _,__default=1000_) – Maximum number of iterations to perform if algorithm=’lasso_cd’ or ‘lasso_lars’.




Base class for all transformers.

Methods

fit(_dataset : Union[DataFrame, DataFrame]_) → BaseEstimator¶
    

Runs universal logics for all fit implementations.

fit_transform(_dataset : Union[DataFrame, DataFrame]_, _output_cols_prefix : str = 'fit_transform_'_) → Union[DataFrame, DataFrame]¶
    

Fit the model from data in X and return the transformed data For more details on this function, see [sklearn.decomposition.DictionaryLearning.fit_transform](https://scikit-learn.org/stable/modules/generated/sklearn.decomposition.DictionaryLearning.html#sklearn.decomposition.DictionaryLearning.fit_transform)

Raises:
    

**TypeError** – Supported dataset types: snowpark.DataFrame, pandas.DataFrame.

Parameters:
    

**dataset** – Union[snowflake.snowpark.DataFrame, pandas.DataFrame] Snowpark or Pandas DataFrame.

output_cols_prefix: Prefix for the response columns :returns: Transformed dataset.

get_input_cols() → List[str]¶
    

Input columns getter.

Returns:
    

Input columns.

get_label_cols() → List[str]¶
    

Label column getter.

Returns:
    

Label column(s).

get_output_cols() → List[str]¶
    

Output columns getter.

Returns:
    

Output columns.

get_params(_deep : bool = True_) → Dict[str, Any]¶
    

Get the snowflake-ml parameters for this transformer.

Parameters:
    

**deep** – If True, will return the parameters for this transformer and contained subobjects that are transformers.

Returns:
    

Parameter names mapped to their values.

get_passthrough_cols() → List[str]¶
    

Passthrough columns getter.

Returns:
    

Passthrough column(s).

get_sample_weight_col() → Optional[str]¶
    

Sample weight column getter.

Returns:
    

Sample weight column.

get_sklearn_args(_default_sklearn_obj : Optional[object] = None_, _sklearn_initial_keywords : Optional[Union[str, Iterable[str]]] = None_, _sklearn_unused_keywords : Optional[Union[str, Iterable[str]]] = None_, _snowml_only_keywords : Optional[Union[str, Iterable[str]]] = None_, _sklearn_added_keyword_to_version_dict : Optional[Dict[str, str]] = None_, _sklearn_added_kwarg_value_to_version_dict : Optional[Dict[str, Dict[str, str]]] = None_, _sklearn_deprecated_keyword_to_version_dict : Optional[Dict[str, str]] = None_, _sklearn_removed_keyword_to_version_dict : Optional[Dict[str, str]] = None_) → Dict[str, Any]¶
    

Get sklearn keyword arguments.

This method enables modifying object parameters for special cases.

Parameters:
    

  * **default_sklearn_obj** – Sklearn object used to get default parameter values. Necessary when sklearn_added_keyword_to_version_dict is provided.

  * **sklearn_initial_keywords** – Initial keywords in sklearn.

  * **sklearn_unused_keywords** – Sklearn keywords that are unused in snowml.

  * **snowml_only_keywords** – snowml only keywords not present in sklearn.

  * **sklearn_added_keyword_to_version_dict** – Added keywords mapped to the sklearn versions in which they were added.

  * **sklearn_added_kwarg_value_to_version_dict** – Added keyword argument values mapped to the sklearn versions in which they were added.

  * **sklearn_deprecated_keyword_to_version_dict** – Deprecated keywords mapped to the sklearn versions in which they were deprecated.

  * **sklearn_removed_keyword_to_version_dict** – Removed keywords mapped to the sklearn versions in which they were removed.



Returns:
    

Sklearn parameter names mapped to their values.

score_samples(_dataset : Union[DataFrame, DataFrame]_, _output_cols_prefix : str = 'score_samples_'_) → Union[DataFrame, DataFrame]¶
    

Method not supported for this class.

Raises:
    

**TypeError** – Supported dataset types: snowpark.DataFrame, pandas.DataFrame.

Parameters:
    

  * **dataset** – Union[snowflake.snowpark.DataFrame, pandas.DataFrame] Snowpark or Pandas DataFrame.

  * **output_cols_prefix** – Prefix for the response columns



Returns:
    

Output dataset with probability of the sample for each class in the model.

set_drop_input_cols(_drop_input_cols : Optional[bool] = False_) → None¶
    

set_input_cols(_input_cols : Optional[Union[str, Iterable[str]]]_) → DictionaryLearning¶
    

Input columns setter.

Parameters:
    

**input_cols** – A single input column or multiple input columns.

Returns:
    

self

set_label_cols(_label_cols : Optional[Union[str, Iterable[str]]]_) → Base¶
    

Label column setter.

Parameters:
    

**label_cols** – A single label column or multiple label columns if multi task learning.

Returns:
    

self

set_output_cols(_output_cols : Optional[Union[str, Iterable[str]]]_) → Base¶
    

Output columns setter.

Parameters:
    

**output_cols** – A single output column or multiple output columns.

Returns:
    

self

set_params(_** params: Any_) → None¶
    

Set the parameters of this transformer.

The method works on simple transformers as well as on sklearn compatible pipelines with nested objects, once the transformer has been fit. Nested objects have parameters of the form `<component>__<parameter>` so that it’s possible to update each component of a nested object.

Parameters:
    

****params** – Transformer parameter names mapped to their values.

Raises:
    

**SnowflakeMLException** – Invalid parameter keys.

set_passthrough_cols(_passthrough_cols : Optional[Union[str, Iterable[str]]]_) → Base¶
    

Passthrough columns setter.

Parameters:
    

**passthrough_cols** – Column(s) that should not be used or modified by the estimator/transformer. Estimator/Transformer just passthrough these columns without any modifications.

Returns:
    

self

set_sample_weight_col(_sample_weight_col : Optional[str]_) → Base¶
    

Sample weight column setter.

Parameters:
    

**sample_weight_col** – A single column that represents sample weight.

Returns:
    

self

to_sklearn() → Any¶
    

Get sklearn.decomposition.DictionaryLearning object.

transform(_dataset : Union[DataFrame, DataFrame]_) → Union[DataFrame, DataFrame]¶
    

Encode the data as a sparse combination of the dictionary atoms For more details on this function, see [sklearn.decomposition.DictionaryLearning.transform](https://scikit-learn.org/stable/modules/generated/sklearn.decomposition.DictionaryLearning.html#sklearn.decomposition.DictionaryLearning.transform)

Raises:
    

**TypeError** – Supported dataset types: snowpark.DataFrame, pandas.DataFrame.

Parameters:
    

**dataset** – Union[snowflake.snowpark.DataFrame, pandas.DataFrame] Snowpark or Pandas DataFrame.

Returns:
    

Transformed dataset.

Attributes

model_signatures¶
    

Returns model signature of current class.

Raises:
    

**SnowflakeMLException** – If estimator is not fitted, then model signature cannot be inferred

Returns:
    

Dict with each method and its input output signature
