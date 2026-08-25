---
title: "snowflake.ml.modeling.decomposition.PCA | Snowflake Documentation"
source: https://docs.snowflake.com/en/developer-guide/snowpark-ml/reference/1.6.1/api/modeling/snowflake.ml.modeling.decomposition.PCA.html
cert_domain: domain-2-data-preparation-and-feature-engineering
crawl_depth: 1
crawled: 2026-08-23
---

You are viewing documentation about an older version (1.6.1).  [View latest version](/en/developer-guide/snowpark-ml/reference/1.52.0/index)

# snowflake.ml.modeling.decomposition.PCA¶

_class _snowflake.ml.modeling.decomposition.PCA(_*_ , _n_components =None_, _copy =True_, _whiten =False_, _svd_solver ='auto'_, _tol =0.0_, _iterated_power ='auto'_, _n_oversamples =10_, _power_iteration_normalizer ='auto'_, _random_state =None_, _input_cols : Optional[Union[str, Iterable[str]]] = None_, _output_cols : Optional[Union[str, Iterable[str]]] = None_, _label_cols : Optional[Union[str, Iterable[str]]] = None_, _passthrough_cols : Optional[Union[str, Iterable[str]]] = None_, _drop_input_cols : Optional[bool] = False_, _sample_weight_col : Optional[str] = None_)¶
    

Bases: `BaseTransformer`

Principal component analysis (PCA) For more details on this class, see [sklearn.decomposition.PCA](https://scikit-learn.org/stable/modules/generated/sklearn.decomposition.PCA.html)

Parameters:
    

  * **input_cols** (_Optional_ _[__Union_ _[__str_ _,__List_ _[__str_ _]__]__]_) – A string or list of strings representing column names that contain features. If this parameter is not specified, all columns in the input DataFrame except the columns specified by label_cols, sample_weight_col, and passthrough_cols parameters are considered input columns. Input columns can also be set after initialization with the set_input_cols method.

  * **label_cols** (_Optional_ _[__Union_ _[__str_ _,__List_ _[__str_ _]__]__]_) – A string or list of strings representing column names that contain labels. Label columns must be specified with this parameter during initialization or with the set_label_cols method before fitting.

  * **output_cols** (_Optional_ _[__Union_ _[__str_ _,__List_ _[__str_ _]__]__]_) – A string or list of strings representing column names that will store the output of predict and transform operations. The length of output_cols must match the expected number of output columns from the specific predictor or transformer class used. If you omit this parameter, output column names are derived by adding an OUTPUT_ prefix to the label column names for supervised estimators, or OUTPUT_<IDX>for unsupervised estimators. These inferred output column names work for predictors, but output_cols must be set explicitly for transformers. In general, explicitly specifying output column names is clearer, especially if you don’t specify the input column names. To transform in place, pass the same names for input_cols and output_cols. be set explicitly for transformers. Output columns can also be set after initialization with the set_output_cols method.

  * **sample_weight_col** (_Optional_ _[__str_ _]_) – A string representing the column name containing the sample weights. This argument is only required when working with weighted datasets. Sample weight column can also be set after initialization with the set_sample_weight_col method.

  * **passthrough_cols** (_Optional_ _[__Union_ _[__str_ _,__List_ _[__str_ _]__]__]_) – A string or a list of strings indicating column names to be excluded from any operations (such as train, transform, or inference). These specified column(s) will remain untouched throughout the process. This option is helpful in scenarios requiring automatic input_cols inference, but need to avoid using specific columns, like index columns, during training or inference. Passthrough columns can also be set after initialization with the set_passthrough_cols method.

  * **drop_input_cols** (_Optional_ _[__bool_ _]__,__default=False_) – If set, the response of predict(), transform() methods will not contain input columns.

  * **n_components** (_int_ _,__float_ _or_ _'mle'__,__default=None_) – 

Number of components to keep. if n_components is not set all components are kept:
[code] n_components == min(n_samples, n_features)
        
[/code]

If `n_components == 'mle'` and `svd_solver == 'full'`, Minka’s MLE is used to guess the dimension. Use of `n_components == 'mle'` will interpret `svd_solver == 'auto'` as `svd_solver == 'full'`.

If `0 < n_components < 1` and `svd_solver == 'full'`, select the number of components such that the amount of variance that needs to be explained is greater than the percentage specified by n_components.

If `svd_solver == 'arpack'`, the number of components must be strictly less than the minimum of n_features and n_samples.

Hence, the None case results in:
[code] n_components == min(n_samples, n_features) - 1
        
[/code]

  * **copy** (_bool_ _,__default=True_) – If False, data passed to fit are overwritten and running fit(X).transform(X) will not yield the expected results, use fit_transform(X) instead.

  * **whiten** (_bool_ _,__default=False_) – 

When True (False by default) the components_ vectors are multiplied by the square root of n_samples and then divided by the singular values to ensure uncorrelated outputs with unit component-wise variances.

Whitening will remove some information from the transformed signal (the relative variance scales of the components) but can sometime improve the predictive accuracy of the downstream estimators by making their data respect some hard-wired assumptions.

  * **svd_solver** (_{'auto'__,__'full'__,__'arpack'__,__'randomized'}__,__default='auto'_) – 

If auto :
    

The solver is selected by a default policy based on X.shape and n_components: if the input data is larger than 500x500 and the number of components to extract is lower than 80% of the smallest dimension of the data, then the more efficient ‘randomized’ method is enabled. Otherwise the exact full SVD is computed and optionally truncated afterwards.

If full :
    

run exact full SVD calling the standard LAPACK solver via scipy.linalg.svd and select the components by postprocessing

If arpack :
    

run SVD truncated to n_components calling ARPACK solver via scipy.sparse.linalg.svds. It requires strictly 0 < n_components < min(X.shape)

If randomized :
    

run randomized SVD by the method of Halko et al.

  * **tol** (_float_ _,__default=0.0_) – Tolerance for singular values computed by svd_solver == ‘arpack’. Must be of range [0.0, infinity).

  * **iterated_power** (_int_ _or_ _'auto'__,__default='auto'_) – Number of iterations for the power method computed by svd_solver == ‘randomized’. Must be of range [0, infinity).

  * **n_oversamples** (_int_ _,__default=10_) – This parameter is only relevant when svd_solver=”randomized”. It corresponds to the additional number of random vectors to sample the range of X so as to ensure proper conditioning. See `randomized_svd()` for more details.

  * **power_iteration_normalizer** (_{'auto'__,__'QR'__,__'LU'__,__'none'}__,__default='auto'_) – Power iteration normalizer for randomized SVD solver. Not used by ARPACK. See `randomized_svd()` for more details.

  * **random_state** (_int_ _,__RandomState instance_ _or_ _None_ _,__default=None_) – Used when the ‘arpack’ or ‘randomized’ solvers are used. Pass an int for reproducible results across multiple function calls. See Glossary.




Base class for all transformers.

Methods

fit(_dataset : Union[DataFrame, DataFrame]_) → BaseEstimator¶
    

Runs universal logics for all fit implementations.

fit_transform(_dataset : Union[DataFrame, DataFrame]_, _output_cols_prefix : str = 'fit_transform_'_) → Union[DataFrame, DataFrame]¶
    

Fit the model with X and apply the dimensionality reduction on X For more details on this function, see [sklearn.decomposition.PCA.fit_transform](https://scikit-learn.org/stable/modules/generated/sklearn.decomposition.PCA.html#sklearn.decomposition.PCA.fit_transform)

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

score(_dataset : Union[DataFrame, DataFrame]_) → float¶
    

Return the average log-likelihood of all samples For more details on this function, see [sklearn.decomposition.PCA.score](https://scikit-learn.org/stable/modules/generated/sklearn.decomposition.PCA.html#sklearn.decomposition.PCA.score)

Raises:
    

**TypeError** – Supported dataset types: snowpark.DataFrame, pandas.DataFrame.

Parameters:
    

**dataset** – Union[snowflake.snowpark.DataFrame, pandas.DataFrame] Snowpark or Pandas DataFrame.

Returns:
    

Score.

score_samples(_dataset : Union[DataFrame, DataFrame]_, _output_cols_prefix : str = 'score_samples_'_) → Union[DataFrame, DataFrame]¶
    

Return the log-likelihood of each sample For more details on this function, see [sklearn.decomposition.PCA.score_samples](https://scikit-learn.org/stable/modules/generated/sklearn.decomposition.PCA.html#sklearn.decomposition.PCA.score_samples)

Raises:
    

**TypeError** – Supported dataset types: snowpark.DataFrame, pandas.DataFrame.

Parameters:
    

  * **dataset** – Union[snowflake.snowpark.DataFrame, pandas.DataFrame] Snowpark or Pandas DataFrame.

  * **output_cols_prefix** – Prefix for the response columns



Returns:
    

Output dataset with probability of the sample for each class in the model.

set_drop_input_cols(_drop_input_cols : Optional[bool] = False_) → None¶
    

set_input_cols(_input_cols : Optional[Union[str, Iterable[str]]]_) → PCA¶
    

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
    

Get sklearn.decomposition.PCA object.

transform(_dataset : Union[DataFrame, DataFrame]_) → Union[DataFrame, DataFrame]¶
    

Apply dimensionality reduction to X For more details on this function, see [sklearn.decomposition.PCA.transform](https://scikit-learn.org/stable/modules/generated/sklearn.decomposition.PCA.html#sklearn.decomposition.PCA.transform)

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
