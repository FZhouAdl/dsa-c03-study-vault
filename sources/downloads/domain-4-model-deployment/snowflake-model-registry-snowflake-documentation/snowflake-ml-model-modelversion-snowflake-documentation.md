---
title: "snowflake.ml.model.ModelVersion | Snowflake Documentation"
source: https://docs.snowflake.com/en/developer-guide/snowpark-ml/reference/latest/api/model/snowflake.ml.model.ModelVersion
cert_domain: domain-4-model-deployment
crawl_depth: 1
crawled: 2026-08-23
---

# snowflake.ml.model.ModelVersion¶

_class _snowflake.ml.model.ModelVersion¶
    

Bases: `LineageNode`

Model Version Object representing a specific version of the model that could be run.

Initializes a LineageNode instance.

Parameters:
    

  * **session** – The Snowflake session object.

  * **name** – Fully qualified name of the lineage node, which is in the format ‘<db>.<schema>.<object_name>’.

  * **domain** – The domain of the lineage node.

  * **version** – The version of the lineage node, if applies.

  * **status** – The status of the lineage node. Possible values are: \- ‘MASKED’: The user does not have the privilege to view the node. \- ‘DELETED’: The node has been deleted. \- ‘ACTIVE’: The node is currently active.

  * **created_on** – The creation time of the lineage node.



Raises:
    

**ValueError** – If the name is not fully qualified.

Methods

create_service(_*_ , _service_name : str_, _image_build_compute_pool : Optional[str] = None_, _service_compute_pool : str_, _image_repo : Optional[str] = None_, _ingress_enabled : bool = False_, _min_instances : int = 0_, _max_instances : int = 1_, _cpu_requests : Optional[str] = None_, _memory_requests : Optional[str] = None_, _gpu_requests : Optional[str] = None_, _num_workers : Optional[int] = None_, _max_batch_rows : Optional[int] = None_, _force_rebuild : bool = False_, _build_external_access_integration : Optional[str] = None_, _block : bool = True_, _autocapture : Optional[bool] = None_, _inference_engine_options : Optional[dict[str, Any]] = None_, _experimental_options : Optional[dict[str, Any]] = None_, _feature_sources_per_function : Optional[dict[str, list[[FeatureView](../feature_store/snowflake.ml.feature_store.FeatureView.html#snowflake.ml.feature_store.FeatureView "snowflake.ml.feature_store.feature_view.FeatureView")]]] = None_) → Union[str, async_job.AsyncJob]¶
create_service(_*_ , _service_name : str_, _image_build_compute_pool : Optional[str] = None_, _service_compute_pool : str_, _image_repo : Optional[str] = None_, _ingress_enabled : bool = False_, _min_instances : int = 0_, _max_instances : int = 1_, _cpu_requests : Optional[str] = None_, _memory_requests : Optional[str] = None_, _gpu_requests : Optional[str] = None_, _num_workers : Optional[int] = None_, _max_batch_rows : Optional[int] = None_, _force_rebuild : bool = False_, _build_external_access_integrations : Optional[list[str]] = None_, _block : bool = True_, _autocapture : Optional[bool] = None_, _inference_engine_options : Optional[dict[str, Any]] = None_, _experimental_options : Optional[dict[str, Any]] = None_, _feature_sources_per_function : Optional[dict[str, list[[FeatureView](../feature_store/snowflake.ml.feature_store.FeatureView.html#snowflake.ml.feature_store.FeatureView "snowflake.ml.feature_store.feature_view.FeatureView")]]] = None_) → Union[str, async_job.AsyncJob]
    

Create an inference service with the given spec.

Parameters:
    

  * **service_name** – The name of the service, can be fully qualified. If not fully qualified, the database or schema of the model will be used.

  * **image_build_compute_pool** – The name of the compute pool used to build the model inference image. It uses the service compute pool if None.

  * **service_compute_pool** – The name of the compute pool used to run the inference service.

  * **image_repo** – The name of the image repository, can be fully qualified. If not fully qualified, the database or schema of the model will be used. This can be None, in that case a default hidden image repository will be used.

  * **ingress_enabled** – If true, creates an service endpoint associated with the service. User must have BIND SERVICE ENDPOINT privilege on the account.

  * **min_instances** – The minimum number of instances for the inference service. The service will automatically scale between min_instances and max_instances based on traffic and hardware utilization. If set to 0 (default), the service will automatically suspend after a period of inactivity.

  * **max_instances** – The maximum number of instances for the inference service.

  * **cpu_requests** – The cpu limit for CPU based inference. Can be an integer, fractional or string values. If None, we attempt to utilize all the vCPU of the node.

  * **memory_requests** – The memory limit with for CPU based inference. Can be an integer or a fractional value, but requires a unit (GiB, MiB). If None, we attempt to utilize all the memory of the node.

  * **gpu_requests** – The gpu limit for GPU based inference. Can be integer, fractional or string values. Use CPU if None.

  * **num_workers** – The number of workers to run the inference service for handling requests in parallel within an instance of the service. By default, it is set to 2*vCPU+1 of the node for CPU based inference and 1 for GPU based inference. For GPU based inference, please see best practices before playing with this value.

  * **max_batch_rows** – The maximum number of rows to batch for inference. Auto determined if None. Minimum 32.

  * **force_rebuild** – Whether to force a model inference image rebuild.

  * **build_external_access_integration** – (Deprecated) The external access integration for image build. This is usually permitting access to conda & PyPI repositories.

  * **build_external_access_integrations** – The external access integrations for image build. This is usually permitting access to conda & PyPI repositories.

  * **block** – A bool value indicating whether this function will wait until the service is available. When it is False, this function executes the underlying service creation asynchronously and returns an AsyncJob.

  * **autocapture** – Whether inference autocapture is enabled on the service. If true, inference data will be captured in the model inference table.

  * **inference_engine_options** – Options for the service creation with custom inference engine. Supports engine and engine_args_override. engine is the type of the inference engine to use. Accepts an `InferenceEngine` enum member or a case-insensitive string such as `"vllm"` or `"python_generic"`. engine_args_override is a list of string arguments to pass to the inference engine.

  * **experimental_options** – Experimental options for the service creation.

  * **feature_sources_per_function** – Optional mapping from model function name (e.g. `"predict"`) to the list of registered `FeatureView` objects whose columns should be looked up at inference time. The model service will fetch any missing feature columns from these sources before invoking the model. Currently only one FeatureView per function is supported.



Raises:
    

  * **ValueError** – Illegal external access integration arguments.

  * **ValueError** – If GPU resources are requested but the model does not have GPU runtime support.

  * **ValueError** – If all model methods are TABLE_FUNCTION type, which is not supported for online inference.

  * **exceptions.SnowparkSQLException** – if service already exists.



Returns:
    

If block=True, return result information about service creation from server. Otherwise, return the service creation AsyncJob.

delete_metric(_metric_name : str_) → None¶
    

Delete a metric from metric storage.

Parameters:
    

**metric_name** – The name of the metric to be deleted.

Raises:
    

**KeyError** – When the requested metric name does not exist.

delete_service(_service_name : str_) → None¶
    

Drops the given service.

Parameters:
    

**service_name** – The name of the service, can be fully qualified. If not fully qualified, the database or schema of the model will be used.

Raises:
    

**ValueError** – If the service does not exist or operation is not permitted by user or service does not belong to this model.

export(_target_path : str_, _*_ , _export_mode : [ExportMode](snowflake.ml.model.ExportMode.html#snowflake.ml.model.ExportMode "snowflake.ml.model._client.model.model_version_impl.ExportMode") = ExportMode.MODEL_) → None¶
    

Export model files to a local directory.

Parameters:
    

  * **target_path** – Path to a local directory to export files to. A directory will be created if does not exist.

  * **export_mode** – The mode to export the model. Defaults to ExportMode.MODEL. ExportMode.MODEL: All model files including environment to load the model and model weights. ExportMode.FULL: Additional files to run the model in Warehouse, besides all files in MODEL mode,



Raises:
    

**ValueError** – Raised when the target path is a file or an non-empty folder.

get_metric(_metric_name : str_) → Any¶
    

Get the value of a specific metric.

Parameters:
    

**metric_name** – The name of the metric.

Raises:
    

**KeyError** – When the requested metric name does not exist.

Returns:
    

The value of the metric.

get_model_task() → Task¶
    

lineage(_direction : Literal['upstream', 'downstream'] = 'downstream'_, _domain_filter : Optional[set[Literal['feature_view', 'dataset', 'model', 'table', 'view']]] = None_) → list[typing.Union[ForwardRef('feature_view.FeatureView'), ForwardRef('dataset.Dataset'), ForwardRef('ModelVersion'), ForwardRef('LineageNode')]]¶
    

Retrieves the lineage nodes connected to this node.

Parameters:
    

  * **direction** – The direction to trace lineage. Defaults to “downstream”.

  * **domain_filter** – Set of domains to filter nodes. Defaults to None.



Returns:
    

A list of connected lineage nodes.

Return type:
    

_List_[LineageNode]

list_services() → DataFrame¶
    

List all the service names using this model version.

Returns:
    

name: The name of the service. status: The status of the service. inference_endpoint: The public endpoint of the service, if enabled and services is not in PENDING state.

> This will give privatelink endpoint if the session is created with privatelink connection

internal_endpoint: The internal endpoint of the service, if services is not in PENDING state. autocapture_enabled: Whether service has autocapture enabled, if it is set in service proxy spec.

Return type:
    

List of details about all the services associated with this model version. The details include

load(_*_ , _force : bool = False_, _options : Optional[Union[BaseModelLoadOption, CatBoostModelLoadOptions, CustomModelLoadOption, LGBMModelLoadOptions, ProphetLoadOptions, SKLModelLoadOptions, XGBModelLoadOptions, SNOWModelLoadOptions, PyTorchLoadOptions, TorchScriptLoadOptions, TensorflowLoadOptions, MLFlowLoadOptions, HuggingFaceLoadOptions, SentenceTransformersLoadOptions, KerasLoadOptions]] = None_) → Union[catboost.CatBoost, lightgbm.LGBMModel, lightgbm.Booster, prophet.Prophet, [CustomModel](snowflake.ml.model.custom_model.CustomModel.html#snowflake.ml.model.custom_model.CustomModel "snowflake.ml.model.custom_model.CustomModel"), sklearn.base.BaseEstimator, sklearn.pipeline.Pipeline, xgboost.XGBModel, xgboost.Booster, torch.nn.Module, torch.jit.ScriptModule, tensorflow.Module, keras.Model, base.BaseEstimator, mlflow.pyfunc.PyFuncModel, transformers.Pipeline, sentence_transformers.SentenceTransformer, [HuggingFacePipelineModel](snowflake.ml.model.HuggingFacePipelineModel.html#snowflake.ml.model.HuggingFacePipelineModel "snowflake.ml.model.models.huggingface_pipeline.HuggingFacePipelineModel"), snowflake.ml.model.models.huggingface.SentenceTransformer]¶
    

Load the underlying original Python object back from a model.
    

This operation requires to have the exact the same environment as the one when logging the model, otherwise, the model might be not functional or some other problems might occur.

Parameters:
    

  * **force** – Bypass the best-effort environment validation. Defaults to False.

  * **options** – Options to specify when loading the model, check snowflake.ml.model.type_hints for available options. Defaults to None.



Raises:
    

**ValueError** – Raised when the best-effort environment validation fails.

Returns:
    

The original Python object loaded from the model object.

run(_X : Union[pd.DataFrame, dataframe.DataFrame]_, _*_ , _function_name : Optional[str] = None_, _partition_column : Optional[str] = None_, _strict_input_validation : bool = False_, _params : Optional[dict[str, Any]] = None_) → Union[pd.DataFrame, dataframe.DataFrame]¶
run(_X : Union[pd.DataFrame, dataframe.DataFrame]_, _*_ , _service_name : str_, _function_name : Optional[str] = None_, _strict_input_validation : bool = False_, _params : Optional[dict[str, Any]] = None_) → Union[pd.DataFrame, dataframe.DataFrame]
    

Invoke a method in a model version object via the warehouse or a service.

Parameters:
    

  * **X** – The input data, which could be a pandas DataFrame or Snowpark DataFrame.

  * **service_name** – The service name. If None, the function is invoked via the warehouse. Otherwise, the function is invoked via the given service.

  * **function_name** – The function name to run. It is the name used to call a function in SQL.

  * **partition_column** – The partition column name to partition by.

  * **strict_input_validation** – Enable stricter validation for the input data. This will result value range based type validation to make sure your input data won’t overflow when providing to the model.

  * **params** – Optional dictionary of model inference parameters (e.g., temperature, top_k for LLMs). These are passed as keyword arguments to the model’s inference method. Defaults to None.



Returns:
    

The prediction data. It would be the same type dataframe as your input.

Raises:
    

**ValueError** – When the model does not support running on warehouse and no service name is provided.

run_batch(_X : DataFrame_, _*_ , _compute_pool : str_, _input_spec : Optional[[InputSpec](../model_batch/snowflake.ml.model.batch.InputSpec.html#snowflake.ml.model.batch.InputSpec "snowflake.ml.model._client.model.batch_inference_specs.InputSpec")] = None_, _output_spec : [OutputSpec](../model_batch/snowflake.ml.model.batch.OutputSpec.html#snowflake.ml.model.batch.OutputSpec "snowflake.ml.model._client.model.batch_inference_specs.OutputSpec")_, _job_spec : Optional[[JobSpec](../model_batch/snowflake.ml.model.batch.JobSpec.html#snowflake.ml.model.batch.JobSpec "snowflake.ml.model._client.model.batch_inference_specs.JobSpec")] = None_, _inference_engine_options : Optional[dict[str, Any]] = None_) → [MLJob](../jobs/snowflake.ml.jobs.MLJob.html#snowflake.ml.jobs.MLJob "snowflake.ml.jobs.job.MLJob")[Any]¶
    

Execute batch inference on datasets as an SPCS job.

Deprecated since version The: backend this method uses will be removed in a future release, and it will be replaced by an updated batch inference jobs API. For new work, use `_run_batch_v2()`, which runs on the updated backend and becomes the public `run_batch` in the next major release (2.0).

Parameters:
    

  * **compute_pool** (_str_) – Name of the compute pool to use for building the image containers and batch inference execution.

  * **X** (_dataframe.DataFrame_) – Snowpark DataFrame containing the input data for inference. The DataFrame should contain all required features for model prediction and passthrough columns.

  * **output_spec** ([_OutputSpec_](../model_batch/snowflake.ml.model.batch.OutputSpec.html#snowflake.ml.model.batch.OutputSpec "snowflake.ml.model._client.model.batch_inference_specs.OutputSpec")) – Configuration for where and how to save the inference results. Specifies the stage location and file handling behavior.

  * **input_spec** (_Optional_ _[_[_InputSpec_](../model_batch/snowflake.ml.model.batch.InputSpec.html#snowflake.ml.model.batch.InputSpec "snowflake.ml.model._client.model.batch_inference_specs.InputSpec") _]_) – Optional configuration for input processing including model inference parameters and column handling options. If None, default values will be used for params and column_handling.

  * **job_spec** (_Optional_ _[_[_JobSpec_](../model_batch/snowflake.ml.model.batch.JobSpec.html#snowflake.ml.model.batch.JobSpec "snowflake.ml.model._client.model.batch_inference_specs.JobSpec") _]_) – Optional configuration for job execution parameters such as compute resources, worker counts, and job naming. If None, default values will be used.

  * **inference_engine_options** – Options for batch inference with a custom inference engine. Supports engine and engine_args_override. engine is the type of the inference engine to use. Accepts an `InferenceEngine` enum member or a case-insensitive string such as `"vllm"` or `"python_generic"`. engine_args_override is a list of string arguments to pass to the inference engine.



Returns:
    

A batch inference job object that can be used to monitor progress and manage the job
    

lifecycle.

Return type:
    

[_MLJob_](../jobs/snowflake.ml.jobs.MLJob.html#snowflake.ml.jobs.MLJob "snowflake.ml.jobs.job.MLJob")[_Any_]

Raises:
    

  * **ValueError** – If warehouse is not set in job_spec and no current warehouse is available.

  * **ValueError** – If the specified function is a partitioned model function.

  * **RuntimeError** – If the input data cannot be processed or written to the staging location.




Example
[code] 
    >>> # Prepare input data - Example 1: From a table
    >>> input_df = session.table("my_input_table")
    >>>
    >>> # Prepare input data - Example 2: From a SQL query
    >>> input_df = session.sql(
    ...     "SELECT id, feature_1, feature_2 FROM feature_table WHERE feature_1 > 100"
    ... )
    >>>
    >>> # Prepare input data - Example 3: From Parquet files in a stage
    >>> input_df = session.read.option("pattern", ".*\.parquet").parquet(
    ...     "@my_stage/input_data/"
    ... ).select("id", "feature_1", "feature_2")
    >>>
    >>> # Prepare input data - Example 4: From image files in a stage
    >>> from snowflake.ml.utils.stage_file import list_stage_files
    >>> input_df = list_stage_files(session, "@my_stage/path", pattern=".*\.jpg")
    >>>
    >>> # Configure output location
    >>> output_spec = OutputSpec(
    ...     stage_location='@My_DB.PUBLIC.MY_STAGE/someth/path/',
    ...     mode=SaveMode.OVERWRITE
    ... )
    >>>
    >>> # Configure job parameters
    >>> job_spec = JobSpec(
    ...     job_name="my_batch_inference",
    ...     num_workers=4,
    ...     cpu_requests="2",
    ...     memory_requests="8Gi"
    ... )
    >>>
    >>> # Run batch inference
    >>> job = model_version.run_batch(
    ...     compute_pool="my_compute_pool",
    ...     X=input_df,
    ...     output_spec=output_spec,
    ...     job_spec=job_spec
    ... )
    >>>
    >>> # Run batch inference with InputSpec for additional options
    >>> from snowflake.ml.model._client.model.batch_inference_specs import InputSpec, FileEncoding
    >>> input_spec = InputSpec(
    ...     params={"temperature": 0.7, "top_k": 50},
    ...     column_handling={"image_col": {"encoding": FileEncoding.BASE64}}
    ... )
    >>> job = model_version.run_batch(
    ...     compute_pool="my_compute_pool",
    ...     X=input_df,
    ...     output_spec=output_spec,
    ...     input_spec=input_spec,
    ...     job_spec=job_spec
    ... )
    >>>
    >>> # Run batch inference on image files using list_stage_files
    >>> from snowflake.ml.utils.stage_file import list_stage_files
    >>> from snowflake.ml.model import InputSpec, InputFormat, FileEncoding
    >>> input_df = list_stage_files(session, "@my_stage/images", pattern=".*\.jpg", column_name="IMAGES")
    >>> input_spec = InputSpec(
    ...     column_handling={
    ...         "IMAGES": {"input_format": InputFormat.FULL_STAGE_PATH, "convert_to": FileEncoding.RAW_BYTES}
    ...     }
    ... )
    >>> job = model_version.run_batch(
    ...     compute_pool="my_compute_pool",
    ...     X=input_df,
    ...     output_spec=output_spec,
    ...     input_spec=input_spec,
    ... )
    
[/code]

Note

This method is currently in private preview and requires Snowflake version 1.18.0 or later. The input data is temporarily stored in the output stage location under /_temporary before inference execution.

set_alias(_alias_name : str_) → None¶
    

Set alias to a model version.

Parameters:
    

**alias_name** – Alias to the model version.

set_metric(_metric_name : str_, _value : Any_) → None¶
    

Set the value of a specific metric.

Parameters:
    

  * **metric_name** – The name of the metric.

  * **value** – The value of the metric.




show_functions() → list[snowflake.ml.model._model_composer.model_manifest.model_manifest_schema.ModelFunctionInfo]¶
    

Show all functions information in a model version that is callable.

Returns:
    

  * name: The name of the function to be called (both in SQL and in Python SDK).

  * target_method: The original method name in the logged Python object.

  * signature: Python signature of the original method.




Return type:
    

A list of ModelFunctionInfo objects containing the following information

show_metrics() → dict[str, Any]¶
    

Show all metrics logged with the model version.

Returns:
    

A dictionary showing the metrics.

unset_alias(_version_or_alias : str_) → None¶
    

unset alias to a model version.

Parameters:
    

**version_or_alias** – The name of the version or alias to a version.

Attributes

comment¶
    

The comment to the model version.

description¶
    

The description for the model version. This is an alias of comment.

fully_qualified_model_name¶
    

Return the fully qualified name of the model to which the model version belongs.

model_name¶
    

Return the name of the model to which the model version belongs, usable as a reference in SQL.

session¶
    

version_name¶
    

Return the name of the version to which the model version belongs, usable as a reference in SQL.
