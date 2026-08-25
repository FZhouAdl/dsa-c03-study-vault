---
title: "snowflake.ml.modeling.decomposition.KernelPCA | Snowflake Documentation"
source: https://docs.snowflake.com/en/developer-guide/snowpark-ml/reference/1.6.1/api/modeling/snowflake.ml.modeling.decomposition.KernelPCA.html
cert_domain: domain-2-data-preparation-and-feature-engineering
crawl_depth: 1
crawled: 2026-08-23
---

You are viewing documentation about an older version (1.6.1).  [View latest version](/en/developer-guide/snowpark-ml/reference/1.52.0/index)

# snowflake.ml.modeling.decomposition.KernelPCA¶

_class _snowflake.ml.modeling.decomposition.KernelPCA(_*_ , _n_components =None_, _kernel ='linear'_, _gamma =None_, _degree =3_, _coef0 =1_, _kernel_params =None_, _alpha =1.0_, _fit_inverse_transform =False_, _eigen_solver ='auto'_, _tol =0_, _max_iter =None_, _iterated_power ='auto'_, _remove_zero_eig =False_, _random_state =None_, _copy_X =True_, _n_jobs =None_, _input_cols : Optional[Union[str, Iterable[str]]] = None_, _output_cols : Optional[Union[str, Iterable[str]]] = None_, _label_cols : Optional[Union[str, Iterable[str]]] = None_, _passthrough_cols : Optional[Union[str, Iterable[str]]] = None_, _drop_input_cols : Optional[bool] = False_, _sample_weight_col : Optional[str] = None_)¶
    

Bases: `BaseTransformer`

Kernel Principal component analysis (KPCA) [1]_ For more details on this class, see [sklearn.decomposition.KernelPCA](https://scikit-learn.org/stable/modules/generated/sklearn.decomposition.KernelPCA.html)

Parameters:
    

  * **input_cols** (_Optional_ _[__Union_ _[__str_ _,__List_ _[__str_ _]__]__]_) – A string or list of strings representing column names that contain features. If this parameter is not specified, all columns in the input DataFrame except the columns specified by label_cols, sample_weight_col, and passthrough_cols parameters are considered input columns. Input columns can also be set after initialization with the set_input_cols method.

  * **label_cols** (_Optional_ _[__Union_ _[__str_ _,__List_ _[__str_ _]__]__]_) – A string or list of strings representing column names that contain labels. Label columns must be specified with this parameter during initialization or with the set_label_cols method before fitting.

  * **output_cols** (_Optional_ _[__Union_ _[__str_ _,__List_ _[__str_ _]__]__]_) – A string or list of strings representing column names that will store the output of predict and transform operations. The length of output_cols must match the expected number of output columns from the specific predictor or transformer class used. If you omit this parameter, output column names are derived by adding an OUTPUT_ prefix to the label column names for supervised estimators, or OUTPUT_<IDX>for unsupervised estimators. These inferred output column names work for predictors, but output_cols must be set explicitly for transformers. In general, explicitly specifying output column names is clearer, especially if you don’t specify the input column names. To transform in place, pass the same names for input_cols and output_cols. be set explicitly for transformers. Output columns can also be set after initialization with the set_output_cols method.

  * **sample_weight_col** (_Optional_ _[__str_ _]_) – A string representing the column name containing the sample weights. This argument is only required when working with weighted datasets. Sample weight column can also be set after initialization with the set_sample_weight_col method.

  * **passthrough_cols** (_Optional_ _[__Union_ _[__str_ _,__List_ _[__str_ _]__]__]_) – A string or a list of strings indicating column names to be excluded from any operations (such as train, transform, or inference). These specified column(s) will remain untouched throughout the process. This option is helpful in scenarios requiring automatic input_cols inference, but need to avoid using specific columns, like index columns, during training or inference. Passthrough columns can also be set after initialization with the set_passthrough_cols method.

  * **drop_input_cols** (_Optional_ _[__bool_ _]__,__default=False_) – If set, the response of predict(), transform() methods will not contain input columns.

  * **n_components** (_int_ _,__default=None_) – Number of components. If None, all non-zero components are kept.

  * **kernel** (_{'linear'__,__'poly'__,__'rbf'__,__'sigmoid'__,__'cosine'__,__'precomputed'}__or_ _callable_ _,__default='linear'_) – Kernel used for PCA.

  * **gamma** (_float_ _,__default=None_) – Kernel coefficient for rbf, poly and sigmoid kernels. Ignored by other kernels. If `gamma` is `None`, then it is set to `1/n_features`.

  * **degree** (_int_ _,__default=3_) – Degree for poly kernels. Ignored by other kernels.

  * **coef0** (_float_ _,__default=1_) – Independent term in poly and sigmoid kernels. Ignored by other kernels.

  * **kernel_params** (_dict_ _,__default=None_) – Parameters (keyword arguments) and values for kernel passed as callable object. Ignored by other kernels.

  * **alpha** (_float_ _,__default=1.0_) – Hyperparameter of the ridge regression that learns the inverse transform (when fit_inverse_transform=True).

  * **fit_inverse_transform** (_bool_ _,__default=False_) – Learn the inverse transform for non-precomputed kernels (i.e. learn to find the pre-image of a point). This method is based on [2]_.

  * **eigen_solver** (_{'auto'__,__'dense'__,__'arpack'__,__'randomized'}__,__default='auto'_) – 

Select eigensolver to use. If n_components is much less than the number of training samples, randomized (or arpack to a smaller extent) may be more efficient than the dense eigensolver. Randomized SVD is performed according to the method of Halko et al [3]_.

auto :
    

the solver is selected by a default policy based on n_samples (the number of training samples) and n_components: if the number of components to extract is less than 10 (strict) and the number of samples is more than 200 (strict), the ‘arpack’ method is enabled. Otherwise the exact full eigenvalue decomposition is computed and optionally truncated afterwards (‘dense’ method).

dense :
    

run exact full eigenvalue decomposition calling the standard LAPACK solver via scipy.linalg.eigh, and select the components by postprocessing

arpack :
    

run SVD truncated to n_components calling ARPACK solver using scipy.sparse.linalg.eigsh. It requires strictly 0 < n_components < n_samples

randomized :
    

run randomized SVD by the method of Halko et al. [3]_. The current implementation selects eigenvalues based on their module; therefore using this method can lead to unexpected results if the kernel is not positive semi-definite. See also [4]_.

  * **tol** (_float_ _,__default=0_) – Convergence tolerance for arpack. If 0, optimal value will be chosen by arpack.

  * **max_iter** (_int_ _,__default=None_) – Maximum number of iterations for arpack. If None, optimal value will be chosen by arpack.

  * **iterated_power** (_int >= 0_ _, or_ _'auto'__,__default='auto'_) – Number of iterations for the power method computed by svd_solver == ‘randomized’. When ‘auto’, it is set to 7 when n_components < 0.1 * min(X.shape), other it is set to 4.

  * **remove_zero_eig** (_bool_ _,__default=False_) – If True, then all components with zero eigenvalues are removed, so that the number of components in the output may be < n_components (and sometimes even zero due to numerical instability). When n_components is None, this parameter is ignored and components with zero eigenvalues are removed regardless.

  * **random_state** (_int_ _,__RandomState instance_ _or_ _None_ _,__default=None_) – Used when `eigen_solver` == ‘arpack’ or ‘randomized’. Pass an int for reproducible results across multiple function calls. See Glossary.

  * **copy_X** (_bool_ _,__default=True_) – If True, input X is copied and stored by the model in the X_fit_ attribute. If no further changes will be done to X, setting copy_X=False saves memory by storing a reference.

  * **n_jobs** (_int_ _,__default=None_) – The number of parallel jobs to run. `None` means 1 unless in a `joblib.parallel_backend` context. `-1` means using all processors. See Glossary for more details.




Base class for all transformers.

Methods

fit(_dataset : Union[DataFrame, DataFrame]_) → BaseEstimator¶
    

Runs universal logics for all fit implementations.

fit_transform(_dataset : Union[DataFrame, DataFrame]_, _output_cols_prefix : str = 'fit_transform_'_) → Union[DataFrame, DataFrame]¶
    

Fit the model from data in X and transform X For more details on this function, see [sklearn.decomposition.KernelPCA.fit_transform](https://scikit-learn.org/stable/modules/generated/sklearn.decomposition.KernelPCA.html#sklearn.decomposition.KernelPCA.fit_transform)

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
    

set_input_cols(_input_cols : Optional[Union[str, Iterable[str]]]_) → KernelPCA¶
    

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
    

Get sklearn.decomposition.KernelPCA object.

transform(_dataset : Union[DataFrame, DataFrame]_) → Union[DataFrame, DataFrame]¶
    

Transform X For more details on this function, see [sklearn.decomposition.KernelPCA.transform](https://scikit-learn.org/stable/modules/generated/sklearn.decomposition.KernelPCA.html#sklearn.decomposition.KernelPCA.transform)

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
