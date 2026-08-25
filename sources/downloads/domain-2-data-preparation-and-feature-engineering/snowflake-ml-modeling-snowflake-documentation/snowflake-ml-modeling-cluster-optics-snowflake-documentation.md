---
title: "snowflake.ml.modeling.cluster.OPTICS | Snowflake Documentation"
source: https://docs.snowflake.com/en/developer-guide/snowpark-ml/reference/1.6.1/api/modeling/snowflake.ml.modeling.cluster.OPTICS.html
cert_domain: domain-2-data-preparation-and-feature-engineering
crawl_depth: 1
crawled: 2026-08-23
---

You are viewing documentation about an older version (1.6.1).  [View latest version](/en/developer-guide/snowpark-ml/reference/1.52.0/index)

# snowflake.ml.modeling.cluster.OPTICS¶

_class _snowflake.ml.modeling.cluster.OPTICS(_*_ , _min_samples =5_, _max_eps =inf_, _metric ='minkowski'_, _p =2_, _metric_params =None_, _cluster_method ='xi'_, _eps =None_, _xi =0.05_, _predecessor_correction =True_, _min_cluster_size =None_, _algorithm ='auto'_, _leaf_size =30_, _memory =None_, _n_jobs =None_, _input_cols : Optional[Union[str, Iterable[str]]] = None_, _output_cols : Optional[Union[str, Iterable[str]]] = None_, _label_cols : Optional[Union[str, Iterable[str]]] = None_, _passthrough_cols : Optional[Union[str, Iterable[str]]] = None_, _drop_input_cols : Optional[bool] = False_, _sample_weight_col : Optional[str] = None_)¶
    

Bases: `BaseTransformer`

Estimate clustering structure from vector array For more details on this class, see [sklearn.cluster.OPTICS](https://scikit-learn.org/stable/modules/generated/sklearn.cluster.OPTICS.html)

Parameters:
    

  * **input_cols** (_Optional_ _[__Union_ _[__str_ _,__List_ _[__str_ _]__]__]_) – A string or list of strings representing column names that contain features. If this parameter is not specified, all columns in the input DataFrame except the columns specified by label_cols, sample_weight_col, and passthrough_cols parameters are considered input columns. Input columns can also be set after initialization with the set_input_cols method.

  * **label_cols** (_Optional_ _[__Union_ _[__str_ _,__List_ _[__str_ _]__]__]_) – A string or list of strings representing column names that contain labels. Label columns must be specified with this parameter during initialization or with the set_label_cols method before fitting.

  * **output_cols** (_Optional_ _[__Union_ _[__str_ _,__List_ _[__str_ _]__]__]_) – A string or list of strings representing column names that will store the output of predict and transform operations. The length of output_cols must match the expected number of output columns from the specific predictor or transformer class used. If you omit this parameter, output column names are derived by adding an OUTPUT_ prefix to the label column names for supervised estimators, or OUTPUT_<IDX>for unsupervised estimators. These inferred output column names work for predictors, but output_cols must be set explicitly for transformers. In general, explicitly specifying output column names is clearer, especially if you don’t specify the input column names. To transform in place, pass the same names for input_cols and output_cols. be set explicitly for transformers. Output columns can also be set after initialization with the set_output_cols method.

  * **sample_weight_col** (_Optional_ _[__str_ _]_) – A string representing the column name containing the sample weights. This argument is only required when working with weighted datasets. Sample weight column can also be set after initialization with the set_sample_weight_col method.

  * **passthrough_cols** (_Optional_ _[__Union_ _[__str_ _,__List_ _[__str_ _]__]__]_) – A string or a list of strings indicating column names to be excluded from any operations (such as train, transform, or inference). These specified column(s) will remain untouched throughout the process. This option is helpful in scenarios requiring automatic input_cols inference, but need to avoid using specific columns, like index columns, during training or inference. Passthrough columns can also be set after initialization with the set_passthrough_cols method.

  * **drop_input_cols** (_Optional_ _[__bool_ _]__,__default=False_) – If set, the response of predict(), transform() methods will not contain input columns.

  * **min_samples** (_int > 1_ _or_ _float between 0 and 1_ _,__default=5_) – The number of samples in a neighborhood for a point to be considered as a core point. Also, up and down steep regions can’t have more than `min_samples` consecutive non-steep points. Expressed as an absolute number or a fraction of the number of samples (rounded to be at least 2).

  * **max_eps** (_float_ _,__default=np.inf_) – The maximum distance between two samples for one to be considered as in the neighborhood of the other. Default value of `np.inf` will identify clusters across all scales; reducing `max_eps` will result in shorter run times.

  * **metric** (_str_ _or_ _callable_ _,__default='minkowski'_) – 

Metric to use for distance computation. Any metric from scikit-learn or scipy.spatial.distance can be used.

If metric is a callable function, it is called on each pair of instances (rows) and the resulting value recorded. The callable should take two arrays as input and return one value indicating the distance between them. This works for Scipy’s metrics, but is less efficient than passing the metric name as a string. If metric is “precomputed”, X is assumed to be a distance matrix and must be square.

Valid values for metric are:

    * from scikit-learn: [‘cityblock’, ‘cosine’, ‘euclidean’, ‘l1’, ‘l2’, ‘manhattan’]

    * from scipy.spatial.distance: [‘braycurtis’, ‘canberra’, ‘chebyshev’, ‘correlation’, ‘dice’, ‘hamming’, ‘jaccard’, ‘kulsinski’, ‘mahalanobis’, ‘minkowski’, ‘rogerstanimoto’, ‘russellrao’, ‘seuclidean’, ‘sokalmichener’, ‘sokalsneath’, ‘sqeuclidean’, ‘yule’]

Sparse matrices are only supported by scikit-learn metrics. See the documentation for scipy.spatial.distance for details on these metrics.

  * **p** (_float_ _,__default=2_) – Parameter for the Minkowski metric from `pairwise_distances`. When p = 1, this is equivalent to using manhattan_distance (l1), and euclidean_distance (l2) for p = 2. For arbitrary p, minkowski_distance (l_p) is used.

  * **metric_params** (_dict_ _,__default=None_) – Additional keyword arguments for the metric function.

  * **cluster_method** (_str_ _,__default='xi'_) – The extraction method used to extract clusters using the calculated reachability and ordering. Possible values are “xi” and “dbscan”.

  * **eps** (_float_ _,__default=None_) – The maximum distance between two samples for one to be considered as in the neighborhood of the other. By default it assumes the same value as `max_eps`. Used only when `cluster_method='dbscan'`.

  * **xi** (_float between 0 and 1_ _,__default=0.05_) – Determines the minimum steepness on the reachability plot that constitutes a cluster boundary. For example, an upwards point in the reachability plot is defined by the ratio from one point to its successor being at most 1-xi. Used only when `cluster_method='xi'`.

  * **predecessor_correction** (_bool_ _,__default=True_) – Correct clusters according to the predecessors calculated by OPTICS [2]_. This parameter has minimal effect on most datasets. Used only when `cluster_method='xi'`.

  * **min_cluster_size** (_int > 1_ _or_ _float between 0 and 1_ _,__default=None_) – Minimum number of samples in an OPTICS cluster, expressed as an absolute number or a fraction of the number of samples (rounded to be at least 2). If `None`, the value of `min_samples` is used instead. Used only when `cluster_method='xi'`.

  * **algorithm** (_{'auto'__,__'ball_tree'__,__'kd_tree'__,__'brute'}__,__default='auto'_) – 

Algorithm used to compute the nearest neighbors:

    * ’ball_tree’ will use `BallTree`.

    * ’kd_tree’ will use `KDTree`.

    * ’brute’ will use a brute-force search.

    * ’auto’ (default) will attempt to decide the most appropriate algorithm based on the values passed to `fit()` method.

Note: fitting on sparse input will override the setting of this parameter, using brute force.

  * **leaf_size** (_int_ _,__default=30_) – Leaf size passed to `BallTree` or `KDTree`. This can affect the speed of the construction and query, as well as the memory required to store the tree. The optimal value depends on the nature of the problem.

  * **memory** (_str_ _or_ _object with the joblib.Memory interface_ _,__default=None_) – Used to cache the output of the computation of the tree. By default, no caching is done. If a string is given, it is the path to the caching directory.

  * **n_jobs** (_int_ _,__default=None_) – The number of parallel jobs to run for neighbors search. `None` means 1 unless in a `joblib.parallel_backend` context. `-1` means using all processors. See Glossary for more details.




Base class for all transformers.

Methods

fit(_dataset : Union[DataFrame, DataFrame]_) → BaseEstimator¶
    

Runs universal logics for all fit implementations.

fit_predict(_dataset : Union[DataFrame, DataFrame]_, _output_cols_prefix : str = 'fit_predict_'_) → Union[DataFrame, DataFrame]¶
    

Perform clustering on X and returns cluster labels For more details on this function, see [sklearn.cluster.OPTICS.fit_predict](https://scikit-learn.org/stable/modules/generated/sklearn.cluster.OPTICS.html#sklearn.cluster.OPTICS.fit_predict)

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
    

set_input_cols(_input_cols : Optional[Union[str, Iterable[str]]]_) → OPTICS¶
    

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
    

Get sklearn.cluster.OPTICS object.

Attributes

model_signatures¶
    

Returns model signature of current class.

Raises:
    

**SnowflakeMLException** – If estimator is not fitted, then model signature cannot be inferred

Returns:
    

Dict with each method and its input output signature
