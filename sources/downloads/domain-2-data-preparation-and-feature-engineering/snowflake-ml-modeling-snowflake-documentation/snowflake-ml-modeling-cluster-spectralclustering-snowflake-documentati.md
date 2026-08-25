---
title: "snowflake.ml.modeling.cluster.SpectralClustering | Snowflake Documentation"
source: https://docs.snowflake.com/en/developer-guide/snowpark-ml/reference/1.6.1/api/modeling/snowflake.ml.modeling.cluster.SpectralClustering.html
cert_domain: domain-2-data-preparation-and-feature-engineering
crawl_depth: 1
crawled: 2026-08-23
---

You are viewing documentation about an older version (1.6.1).  [View latest version](/en/developer-guide/snowpark-ml/reference/1.52.0/index)

# snowflake.ml.modeling.cluster.SpectralClustering¶

_class _snowflake.ml.modeling.cluster.SpectralClustering(_*_ , _n_clusters =8_, _eigen_solver =None_, _n_components =None_, _random_state =None_, _n_init =10_, _gamma =1.0_, _affinity ='rbf'_, _n_neighbors =10_, _eigen_tol ='auto'_, _assign_labels ='kmeans'_, _degree =3_, _coef0 =1_, _kernel_params =None_, _n_jobs =None_, _verbose =False_, _input_cols : Optional[Union[str, Iterable[str]]] = None_, _output_cols : Optional[Union[str, Iterable[str]]] = None_, _label_cols : Optional[Union[str, Iterable[str]]] = None_, _passthrough_cols : Optional[Union[str, Iterable[str]]] = None_, _drop_input_cols : Optional[bool] = False_, _sample_weight_col : Optional[str] = None_)¶
    

Bases: `BaseTransformer`

Apply clustering to a projection of the normalized Laplacian For more details on this class, see [sklearn.cluster.SpectralClustering](https://scikit-learn.org/stable/modules/generated/sklearn.cluster.SpectralClustering.html)

Parameters:
    

  * **input_cols** (_Optional_ _[__Union_ _[__str_ _,__List_ _[__str_ _]__]__]_) – A string or list of strings representing column names that contain features. If this parameter is not specified, all columns in the input DataFrame except the columns specified by label_cols, sample_weight_col, and passthrough_cols parameters are considered input columns. Input columns can also be set after initialization with the set_input_cols method.

  * **label_cols** (_Optional_ _[__Union_ _[__str_ _,__List_ _[__str_ _]__]__]_) – A string or list of strings representing column names that contain labels. Label columns must be specified with this parameter during initialization or with the set_label_cols method before fitting.

  * **output_cols** (_Optional_ _[__Union_ _[__str_ _,__List_ _[__str_ _]__]__]_) – A string or list of strings representing column names that will store the output of predict and transform operations. The length of output_cols must match the expected number of output columns from the specific predictor or transformer class used. If you omit this parameter, output column names are derived by adding an OUTPUT_ prefix to the label column names for supervised estimators, or OUTPUT_<IDX>for unsupervised estimators. These inferred output column names work for predictors, but output_cols must be set explicitly for transformers. In general, explicitly specifying output column names is clearer, especially if you don’t specify the input column names. To transform in place, pass the same names for input_cols and output_cols. be set explicitly for transformers. Output columns can also be set after initialization with the set_output_cols method.

  * **sample_weight_col** (_Optional_ _[__str_ _]_) – A string representing the column name containing the sample weights. This argument is only required when working with weighted datasets. Sample weight column can also be set after initialization with the set_sample_weight_col method.

  * **passthrough_cols** (_Optional_ _[__Union_ _[__str_ _,__List_ _[__str_ _]__]__]_) – A string or a list of strings indicating column names to be excluded from any operations (such as train, transform, or inference). These specified column(s) will remain untouched throughout the process. This option is helpful in scenarios requiring automatic input_cols inference, but need to avoid using specific columns, like index columns, during training or inference. Passthrough columns can also be set after initialization with the set_passthrough_cols method.

  * **drop_input_cols** (_Optional_ _[__bool_ _]__,__default=False_) – If set, the response of predict(), transform() methods will not contain input columns.

  * **n_clusters** (_int_ _,__default=8_) – The dimension of the projection subspace.

  * **eigen_solver** (_{'arpack'__,__'lobpcg'__,__'amg'}__,__default=None_) – The eigenvalue decomposition strategy to use. AMG requires pyamg to be installed. It can be faster on very large, sparse problems, but may also lead to instabilities. If None, then `'arpack'` is used. See [4]_ for more details regarding ‘lobpcg’.

  * **n_components** (_int_ _,__default=None_) – Number of eigenvectors to use for the spectral embedding. If None, defaults to n_clusters.

  * **random_state** (_int_ _,__RandomState instance_ _,__default=None_) – A pseudo random number generator used for the initialization of the lobpcg eigenvectors decomposition when eigen_solver == ‘amg’, and for the K-Means initialization. Use an int to make the results deterministic across calls (See Glossary).

  * **n_init** (_int_ _,__default=10_) – Number of time the k-means algorithm will be run with different centroid seeds. The final results will be the best output of n_init consecutive runs in terms of inertia. Only used if `assign_labels='kmeans'`.

  * **gamma** (_float_ _,__default=1.0_) – Kernel coefficient for rbf, poly, sigmoid, laplacian and chi2 kernels. Ignored for `affinity='nearest_neighbors'`.

  * **affinity** (_str_ _or_ _callable_ _,__default='rbf'_) – 

How to construct the affinity matrix.
    
    * ’nearest_neighbors’: construct the affinity matrix by computing a graph of nearest neighbors.

    * ’rbf’: construct the affinity matrix using a radial basis function (RBF) kernel.

    * ’precomputed’: interpret `X` as a precomputed affinity matrix, where larger values indicate greater similarity between instances.

    * ’precomputed_nearest_neighbors’: interpret `X` as a sparse graph of precomputed distances, and construct a binary affinity matrix from the `n_neighbors` nearest neighbors of each instance.

    * one of the kernels supported by `pairwise_kernels()`.

Only kernels that produce similarity scores (non-negative values that increase with similarity) should be used. This property is not checked by the clustering algorithm.

  * **n_neighbors** (_int_ _,__default=10_) – Number of neighbors to use when constructing the affinity matrix using the nearest neighbors method. Ignored for `affinity='rbf'`.

  * **eigen_tol** (_float_ _,__default="auto"_) – 

Stopping criterion for eigendecomposition of the Laplacian matrix. If eigen_tol=”auto” then the passed tolerance will depend on the eigen_solver:

    * If eigen_solver=”arpack”, then eigen_tol=0.0;

    * If eigen_solver=”lobpcg” or eigen_solver=”amg”, then eigen_tol=None which configures the underlying lobpcg solver to automatically resolve the value according to their heuristics. See, `scipy.sparse.linalg.lobpcg()` for details.

Note that when using eigen_solver=”lobpcg” or eigen_solver=”amg” values of tol<1e-5 may lead to convergence issues and should be avoided.

  * **assign_labels** (_{'kmeans'__,__'discretize'__,__'cluster_qr'}__,__default='kmeans'_) – The strategy for assigning labels in the embedding space. There are two ways to assign labels after the Laplacian embedding. k-means is a popular choice, but it can be sensitive to initialization. Discretization is another approach which is less sensitive to random initialization [3]_. The cluster_qr method [5]_ directly extract clusters from eigenvectors in spectral clustering. In contrast to k-means and discretization, cluster_qr has no tuning parameters and runs no iterations, yet may outperform k-means and discretization in terms of both quality and speed.

  * **degree** (_float_ _,__default=3_) – Degree of the polynomial kernel. Ignored by other kernels.

  * **coef0** (_float_ _,__default=1_) – Zero coefficient for polynomial and sigmoid kernels. Ignored by other kernels.

  * **kernel_params** (_dict of str to any_ _,__default=None_) – Parameters (keyword arguments) and values for kernel passed as callable object. Ignored by other kernels.

  * **n_jobs** (_int_ _,__default=None_) – The number of parallel jobs to run when affinity=’nearest_neighbors’ or affinity=’precomputed_nearest_neighbors’. The neighbors search will be done in parallel. `None` means 1 unless in a `joblib.parallel_backend` context. `-1` means using all processors. See Glossary for more details.

  * **verbose** (_bool_ _,__default=False_) – Verbosity mode.




Base class for all transformers.

Methods

fit(_dataset : Union[DataFrame, DataFrame]_) → BaseEstimator¶
    

Runs universal logics for all fit implementations.

fit_predict(_dataset : Union[DataFrame, DataFrame]_, _output_cols_prefix : str = 'fit_predict_'_) → Union[DataFrame, DataFrame]¶
    

Perform spectral clustering on X and return cluster labels For more details on this function, see [sklearn.cluster.SpectralClustering.fit_predict](https://scikit-learn.org/stable/modules/generated/sklearn.cluster.SpectralClustering.html#sklearn.cluster.SpectralClustering.fit_predict)

Raises:
    

**TypeError** – Supported dataset types: snowpark.DataFrame, pandas.DataFrame.

Parameters:
    

**dataset** – Union[snowflake.snowpark.DataFrame, pandas.DataFrame] Snowpark or Pandas DataFrame.

output_cols_prefix: Prefix for the response columns :returns: Predicted dataset.

fit_transform(_dataset : Union[DataFrame, DataFrame]_, _output_cols_prefix : str = 'fit_transform_'_) → Union[DataFrame, DataFrame]¶
    

Method not supported for this class.

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
    

set_input_cols(_input_cols : Optional[Union[str, Iterable[str]]]_) → SpectralClustering¶
    

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
    

Get sklearn.cluster.SpectralClustering object.

Attributes

model_signatures¶
    

Returns model signature of current class.

Raises:
    

**SnowflakeMLException** – If estimator is not fitted, then model signature cannot be inferred

Returns:
    

Dict with each method and its input output signature
