---
title: "snowflake.ml.registry.Registry | Snowflake Documentation"
source: https://docs.snowflake.com/en/developer-guide/snowpark-ml/reference/latest/api/registry/snowflake.ml.registry.Registry
cert_domain: domain-4-model-deployment
crawl_depth: 1
crawled: 2026-08-23
---

# snowflake.ml.registry.Registry¶

_class _snowflake.ml.registry.Registry(_session : Session_, _*_ , _database_name : Optional[str] = None_, _schema_name : Optional[str] = None_, _options : Optional[dict[str, Any]] = None_)¶
    

Bases: `object`

Opens a registry within a pre-created Snowflake schema.

Parameters:
    

  * **session** – The Snowpark Session to connect with Snowflake.

  * **database_name** – The name of the database. If None, the current database of the session will be used. Defaults to None.

  * **schema_name** – The name of the schema. If None, the current schema of the session will be used. If there is no active schema, the PUBLIC schema will be used. Defaults to None.

  * **options** – Optional set of configurations to modify registry. Registry Options include: \- enable_monitoring: Feature flag to indicate whether registry can be used for monitoring.



Raises:
    

**ValueError** – When there is no specified or active database in the session.

Methods

add_monitor(_name : str_, _source_config : [ModelMonitorSourceConfig](../monitoring/snowflake.ml.monitoring.entities.model_monitor_config.ModelMonitorSourceConfig.html#snowflake.ml.monitoring.entities.model_monitor_config.ModelMonitorSourceConfig "snowflake.ml.monitoring.entities.model_monitor_config.ModelMonitorSourceConfig")_, _model_monitor_config : [ModelMonitorConfig](../monitoring/snowflake.ml.monitoring.entities.model_monitor_config.ModelMonitorConfig.html#snowflake.ml.monitoring.entities.model_monitor_config.ModelMonitorConfig "snowflake.ml.monitoring.entities.model_monitor_config.ModelMonitorConfig")_) → [ModelMonitor](../monitoring/snowflake.ml.monitoring.model_monitor.ModelMonitor.html#snowflake.ml.monitoring.model_monitor.ModelMonitor "snowflake.ml.monitoring.model_monitor.ModelMonitor")¶
    

Add a Model Monitor to the Registry.

Parameters:
    

  * **name** – Name of Model Monitor to create.

  * **source_config** – Configuration options of table for Model Monitor.

  * **model_monitor_config** – Configuration options of Model Monitor.



Returns:
    

The newly added Model Monitor object.

Raises:
    

**ValueError** – If monitoring is not enabled in the Registry.

delete_model(_model_name : str_) → None¶
    

Delete the model by its name.

Parameters:
    

**model_name** – The name of the model to be deleted.

delete_monitor(_name : str_) → None¶
    

Delete a Model Monitor by name from the Registry.

Parameters:
    

**name** – Name of the Model Monitor to delete.

Raises:
    

**ValueError** – If monitoring is not enabled in the registry.

get_model(_model_name : str_) → [Model](../model/snowflake.ml.model.Model.html#snowflake.ml.model.Model "snowflake.ml.model._client.model.model_impl.Model")¶
    

Get the model object by its name.

Parameters:
    

**model_name** – The name of the model.

Returns:
    

The corresponding model object.

get_monitor(_*_ , _model_version : [ModelVersion](../model/snowflake.ml.model.ModelVersion.html#snowflake.ml.model.ModelVersion "snowflake.ml.model._client.model.model_version_impl.ModelVersion")_) → [ModelMonitor](../monitoring/snowflake.ml.monitoring.model_monitor.ModelMonitor.html#snowflake.ml.monitoring.model_monitor.ModelMonitor "snowflake.ml.monitoring.model_monitor.ModelMonitor")¶
get_monitor(_*_ , _name : str_) → [ModelMonitor](../monitoring/snowflake.ml.monitoring.model_monitor.ModelMonitor.html#snowflake.ml.monitoring.model_monitor.ModelMonitor "snowflake.ml.monitoring.model_monitor.ModelMonitor")
    

Get a Model Monitor from the Registry.

Parameters:
    

  * **name** – Name of Model Monitor to retrieve.

  * **model_version** – Model Version for which to retrieve the Model Monitor.



Returns:
    

The fetched Model Monitor.

Raises:
    

**ValueError** – If monitoring is not enabled in the Registry.

log_model(_model : type_hints.SupportedModelType_, _*_ , _model_name : str_, _version_name : Optional[str] = None_, _comment : Optional[str] = None_, _metrics : Optional[dict[str, Any]] = None_, _conda_dependencies : Optional[list[str]] = None_, _pip_requirements : Optional[list[str]] = None_, _artifact_repository_map : Optional[dict[str, str]] = None_, _resource_constraint : Optional[dict[str, str]] = None_, _target_platforms : Optional[list[Union[target_platform.TargetPlatform, str]]] = None_, _python_version : Optional[str] = None_, _signatures : Optional[dict[str, [ModelSignature](../model/snowflake.ml.model.model_signature.ModelSignature.html#snowflake.ml.model.model_signature.ModelSignature "snowflake.ml.model.model_signature.ModelSignature")]] = None_, _sample_input_data : Optional[type_hints.SupportedDataType] = None_, _user_files : Optional[dict[str, list[str]]] = None_, _code_paths : Optional[list[type_hints.CodePathLike]] = None_, _ext_modules : Optional[list[ModuleType]] = None_, _task : task.Task = task.Task.UNKNOWN_, _options : Optional[type_hints.ModelSaveOption] = None_) → [ModelVersion](../model/snowflake.ml.model.ModelVersion.html#snowflake.ml.model.ModelVersion "snowflake.ml.model.ModelVersion")¶
log_model(_model : [ModelVersion](../model/snowflake.ml.model.ModelVersion.html#snowflake.ml.model.ModelVersion "snowflake.ml.model.ModelVersion")_, _*_ , _model_name : str_, _version_name : Optional[str] = None_) → [ModelVersion](../model/snowflake.ml.model.ModelVersion.html#snowflake.ml.model.ModelVersion "snowflake.ml.model.ModelVersion")
    

Log a model with various parameters and metadata, or a ModelVersion object.

Parameters:
    

  * **model** – Supported model or ModelVersion object. \- Supported model: Model object of supported types such as Scikit-learn, XGBoost, LightGBM, Snowpark ML, PyTorch, TorchScript, Tensorflow, Tensorflow Keras, MLFlow, HuggingFace Pipeline, Sentence Transformers, or Custom Model. \- ModelVersion: Source ModelVersion object used to create the new ModelVersion object.

  * **model_name** – Name to identify the model. This must be a valid Snowflake SQL Identifier. Alphanumeric characters and underscores are permitted. See <https://docs.snowflake.com/en/sql-reference/identifiers-syntax> for more.

  * **version_name** – Version identifier for the model. Combination of model_name and version_name must be unique. If not specified, a random name will be generated.

  * **comment** – Comment associated with the model version. Defaults to None.

  * **metrics** – A JSON serializable dictionary containing metrics linked to the model version. Defaults to None.

  * **conda_dependencies** – List of Conda package specifications. Use “[channel::]package [operator version]” syntax to specify a dependency. It is a recommended way to specify your dependencies using conda. When channel is not specified, Snowflake Anaconda Channel will be used. Defaults to None.

  * **pip_requirements** – List of Pip package specifications. Defaults to None. Models running in a Snowflake Warehouse must also specify a pip artifact repository (see artifact_repository_map). Otherwise, models with pip requirements are runnable only in Snowpark Container Services. See <https://docs.snowflake.com/en/developer-guide/snowflake-ml/model-registry/container> for more.

  * **artifact_repository_map** – 

Specifies a mapping of package channels or platforms to custom artifact repositories. Defaults to None. Currently, the mapping applies only to Warehouse execution. Note : This feature is currently in Public Preview. Format: {channel_name: artifact_repository_name}, where:

>     * channel_name: Currently must be ‘pip’.
> 
>     * artifact_repository_name: The identifier of the artifact repository to fetch packages from, e.g. snowflake.snowpark.pypi_shared_repository.

  * **resource_constraint** – Mapping of resource constraint keys and values, e.g. {“architecture”: “x86”}.

  * **target_platforms** – 

List of target platforms to run the model. The only acceptable inputs are a combination of “WAREHOUSE” and “SNOWPARK_CONTAINER_SERVICES”, or a target platform constant: \- [“WAREHOUSE”] or snowflake.ml.model.target_platform.WAREHOUSE_ONLY (Warehouse only) \- [“SNOWPARK_CONTAINER_SERVICES”] or

> snowflake.ml.model.target_platform.SNOWPARK_CONTAINER_SERVICES_ONLY (Snowpark Container Services only)

    * [“WAREHOUSE”, “SNOWPARK_CONTAINER_SERVICES”] or snowflake.ml.model.target_platform.BOTH_WAREHOUSE_AND_SNOWPARK_CONTAINER_SERVICES (Both)

Defaults to None. When None, the target platforms will be both.

  * **python_version** – Python version in which the model is run. Defaults to None.

  * **signatures** – Model data signatures for inputs and outputs for various target methods. If it is None, sample_input_data would be used to infer the signatures for those models that cannot automatically infer the signature. If not None, sample_input_data should not be specified. Defaults to None.

  * **sample_input_data** – Sample input data to infer model signatures from. It would also be used as background data in explanation and to capture data lineage. Defaults to None.

  * **user_files** – Dictionary where the keys are subdirectories, and values are lists of local file name strings. The local file name strings can include wildcards (? or *) for matching multiple files.

  * **code_paths** – List of directories or CodePath objects containing code to import. Defaults to None.

  * **ext_modules** – List of external modules to pickle with the model object. Only supported when logging the following types of model: Scikit-learn, Snowpark ML, PyTorch, TorchScript and Custom Model. Defaults to None.

  * **task** – The task of the Model Version. It is an enum class Task with values TABULAR_REGRESSION, TABULAR_BINARY_CLASSIFICATION, TABULAR_MULTI_CLASSIFICATION, TABULAR_RANKING, or UNKNOWN. By default, it is set to Task.UNKNOWN and may be overridden by inferring from the Model Object.

  * **options** (_Dict_ _[__str_ _,__Any_ _]__,__optional_) – 

Additional model saving options.

Model Saving Options include:

    * embed_local_ml_library: Embed local Snowpark ML into the code directory or folder.
    

Override to True if the local Snowpark ML version is not available in the Snowflake Anaconda Channel. Otherwise, defaults to False

    * relax_version: Whether to relax the version constraints of the dependencies when running in the
    

Warehouse. It detects any ==x.y.z in specifiers and replaced with >=x.y, <(x+1). Defaults to True.

    * function_type: Set the method function type globally. To set method function types individually see function_type in model_options.

    * volatility: Set the volatility for all model methods globally (use Volatility.VOLATILE or Volatility.IMMUTABLE). Volatility.VOLATILE functions may return different results for the same arguments, while Volatility.IMMUTABLE functions always return the same result for the same arguments. Defaults are set automatically based on model type: supported models (sklearn, xgboost, pytorch, huggingface_pipeline, mlflow, etc.) default to IMMUTABLE, while custom models default to VOLATILE. Individual method volatility can be set in method_options and will override this global setting.

    * target_methods: List of target methods to register when logging the model. This option is not used in MLFlow models. Defaults to None, in which case the model handler’s default target methods will be used.

    * enable_explainability: Whether to enable the model explainability feature. Defaults to False.
    

Set to True to generate an explain method for supported model types. Explainability is only available when the model runs in the Warehouse.

Note: In Snowpark Container Services, the explain function is only available for batch inference jobs, not online inference services.

    * save_location: Location to save the model and metadata.

    * capture_sample_input_data: When True (default), a representative row from sample_input_data is captured and stored alongside the model so that inference code snippets shown in the model registry UI can be pre-filled with realistic values. Set to False to opt out (e.g., for sensitive data); generic placeholder values will be used in the snippets instead.

    * method_options: Per-method saving options. This dictionary has method names as keys and dictionary
    

values with the desired options. See the example below.

The following are the available method options:

      * case_sensitive: Indicates whether the method and its signature should be case sensitive.
    

This means when you refer the method in the SQL, you need to double quote it. This will be helpful if you need case to tell apart your methods or features, or you have non-alphabetic characters in your method or feature name. Defaults to False.

      * max_batch_size: Maximum batch size that the method could accept in the Snowflake Warehouse.
    

Defaults to None, determined automatically by Snowflake.

      * function_type: One of supported model method function types (FUNCTION or TABLE_FUNCTION).

      * volatility: Volatility level for the function (use Volatility.VOLATILE or Volatility.IMMUTABLE).
    

Volatility.VOLATILE functions may return different results for the same arguments, while Volatility.IMMUTABLE functions always return the same result for the same arguments. This per-method setting overrides any global volatility setting. Defaults to None (no volatility specified).



Raises:
    

  * **ValueError** – If extra arguments are specified ModelVersion is provided.

  * **Exception** – If the model logging fails.



Returns:
    

ModelVersion object corresponding to the model just logged.

Return type:
    

[_ModelVersion_](../model/snowflake.ml.model.ModelVersion.html#snowflake.ml.model.ModelVersion "snowflake.ml.model.ModelVersion")

Example:
[code] 
    from snowflake.ml.registry import Registry
    
    # create a session
    session = ...
    
    registry = Registry(session=session)
    
    # Define `method_options` for each inference method if needed.
    method_options={
      "predict": {
        "case_sensitive": True
      }
    }
    
    registry.log_model(
      model=model,
      model_name="my_model",
      options={"method_options": method_options},
    )
    
[/code]

models() → list[[Model](../model/snowflake.ml.model.Model.html#snowflake.ml.model.Model "snowflake.ml.model._client.model.model_impl.Model")]¶
    

Get all models in the schema where the registry is opened.

Returns:
    

A list of Model objects representing all models in the opened registry.

show_model_monitors() → list[snowflake.snowpark.row.Row]¶
    

Show all model monitors in the registry.

Returns:
    

List of snowpark.Row containing metadata for each model monitor.

Raises:
    

**ValueError** – If monitoring is not enabled in the Registry.

show_models() → DataFrame¶
    

Show information of all models in the schema where the registry is opened.

Returns:
    

A Pandas DataFrame containing information of all models in the schema.

Attributes

location¶
    

Get the location (database.schema) of the registry.
