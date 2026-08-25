---
title: "snowflake-ml-python API Reference | Snowflake Documentation"
source: https://docs.snowflake.com/en/developer-guide/snowpark-ml/reference/1.52.0/index
cert_domain: domain-2-data-preparation-and-feature-engineering
crawl_depth: 1
crawled: 2026-08-23
---

# `snowflake-ml-python` API Reference¶

The `snowflake-ml-python` package is a set of tools for creating and working with machine learning models in Python on Snowflake. See the [Snowflake ML Developer Guide](https://docs.snowflake.com/developer-guide/snowpark-ml/index) for more information.

Additional ML APIs are available in `snowflake-ml-python` when running on [Container Runtime](https://docs.snowflake.com/developer-guide/snowflake-ml/container-runtime-ml).

`snowflake-ml-python` also includes Python APIs for Snowflake Cortex.

Acknowledgements
    

Portions of the Snowpark ML API Reference are derived from [scikit-learn](https://scikit-learn.org/stable/user_guide.html), [xgboost](https://xgboost.readthedocs.io/en/stable/), and [lightgbm](https://lightgbm.readthedocs.io/en/stable/) documentation.

**scikit-learn** Copyright © 2007-2025 The scikit-learn developers. All rights reserved.

**xgboost** Copyright © 2019 by xgboost contributors.

**lightgbm** Copyright © Microsoft Corporation.

  


## Container Runtime APIs¶

The following APIs are available only in the version of `snowpark-ml-python` available in the Container Runtime, accessible in Snowflake Notebooks running on Snowpark Container Services (SPCS).

  * [Snowflake ML Container Runtime Reference (Python)](container-runtime/index.html "\(in ML Container Runtime \(Python\)\)")




  * [Sharded Data Connector](data_sharded_data_connector)
    * [`snowflake.ml.data_sharded_data_connector.ShardedDataConnector`](data_sharded_data_connector.html#snowflake-ml-data-sharded-data-connector-shardeddataconnector)



## Standard APIs¶

The following APIs are available in both the Container Runtime and in the standalone client version of `snowpark-ml-python` accessible through conda and pip, in Snowsight Python worksheets, and in Snowflake notebooks running on a warehouse.

  * [Standard ML APIs in `snowflake-ml-python`](index-standard)
    * [snowflake.ml.data](data)
      * [snowflake.ml.data.data_connector.DataConnector](api/data/snowflake.ml.data.data_connector.DataConnector)
      * [snowflake.ml.data.data_ingestor.DataIngestor](api/data/snowflake.ml.data.data_ingestor.DataIngestor)
      * [snowflake.ml.data.data_source.DataSource](api/data/snowflake.ml.data.data_source.DataSource)
      * [snowflake.ml.data.data_source.DataFrameInfo](api/data/snowflake.ml.data.data_source.DataFrameInfo)
      * [snowflake.ml.data.data_source.DatasetInfo](api/data/snowflake.ml.data.data_source.DatasetInfo)
    * [snowflake.ml.dataset](dataset)
      * [snowflake.ml.dataset.Dataset](api/dataset/snowflake.ml.dataset.Dataset)
      * [snowflake.ml.dataset.DatasetReader](api/dataset/snowflake.ml.dataset.DatasetReader)
      * [snowflake.ml.dataset.DatasetVersion](api/dataset/snowflake.ml.dataset.DatasetVersion)
      * [snowflake.ml.dataset.create_from_dataframe](api/dataset/snowflake.ml.dataset.create_from_dataframe)
      * [snowflake.ml.dataset.load_dataset](api/dataset/snowflake.ml.dataset.load_dataset)
    * [snowflake.ml.experiment](experiment)
      * [snowflake.ml.experiment.ExperimentTracking](api/experiment/snowflake.ml.experiment.ExperimentTracking)
      * [snowflake.ml.experiment.callback.keras.SnowflakeKerasCallback](api/experiment/snowflake.ml.experiment.callback.keras.SnowflakeKerasCallback)
      * [snowflake.ml.experiment.callback.lightgbm.SnowflakeLightgbmCallback](api/experiment/snowflake.ml.experiment.callback.lightgbm.SnowflakeLightgbmCallback)
      * [snowflake.ml.experiment.callback.xgboost.SnowflakeXgboostCallback](api/experiment/snowflake.ml.experiment.callback.xgboost.SnowflakeXgboostCallback)
    * [snowflake.ml.feature_store](feature_store)
      * [snowflake.ml.feature_store.Entity](api/feature_store/snowflake.ml.feature_store.Entity)
      * [snowflake.ml.feature_store.FeatureStore](api/feature_store/snowflake.ml.feature_store.FeatureStore)
      * [snowflake.ml.feature_store.FeatureView](api/feature_store/snowflake.ml.feature_store.FeatureView)
      * [snowflake.ml.feature_store.setup_feature_store](api/feature_store/snowflake.ml.feature_store.setup_feature_store)
    * [snowflake.ml.fileset](fileset)
      * [snowflake.ml.fileset.sfcfs.SFFileSystem](api/fileset/snowflake.ml.fileset.sfcfs.SFFileSystem)
      * [snowflake.ml.fileset.fileset.FileSet](api/fileset/snowflake.ml.fileset.fileset.FileSet)
    * [snowflake.ml.jobs](jobs)
      * [snowflake.ml.jobs.MLJob](api/jobs/snowflake.ml.jobs.MLJob)
      * [snowflake.ml.jobs.MLJobDefinition](api/jobs/snowflake.ml.jobs.MLJobDefinition)
      * [snowflake.ml.jobs.JOB_STATUS](api/jobs/snowflake.ml.jobs.JOB_STATUS)
      * [snowflake.ml.jobs.remote](api/jobs/snowflake.ml.jobs.remote)
      * [snowflake.ml.jobs.submit_file](api/jobs/snowflake.ml.jobs.submit_file)
      * [snowflake.ml.jobs.submit_directory](api/jobs/snowflake.ml.jobs.submit_directory)
      * [snowflake.ml.jobs.submit_from_stage](api/jobs/snowflake.ml.jobs.submit_from_stage)
      * [snowflake.ml.jobs.list_jobs](api/jobs/snowflake.ml.jobs.list_jobs)
      * [snowflake.ml.jobs.get_job](api/jobs/snowflake.ml.jobs.get_job)
      * [snowflake.ml.jobs.delete_job](api/jobs/snowflake.ml.jobs.delete_job)
    * [snowflake.ml.model](model)
      * [snowflake.ml.model.Model](api/model/snowflake.ml.model.Model)
      * [snowflake.ml.model.ModelVersion](api/model/snowflake.ml.model.ModelVersion)
      * [snowflake.ml.model.HuggingFacePipelineModel](api/model/snowflake.ml.model.HuggingFacePipelineModel)
      * [snowflake.ml.model.TransformersPipeline](api/model/snowflake.ml.model.TransformersPipeline)
      * [snowflake.ml.model.CodePath](api/model/snowflake.ml.model.CodePath)
      * [snowflake.ml.model.ExportMode](api/model/snowflake.ml.model.ExportMode)
      * [snowflake.ml.model.Volatility](api/model/snowflake.ml.model.Volatility)
      * [snowflake.ml.model.custom_model](model.html#snowflake-ml-model-custom-model)
      * [snowflake.ml.model.model_signature](model.html#snowflake-ml-model-model-signature)
      * [snowflake.ml.model.openai_signatures](model.html#snowflake-ml-model-openai-signatures)
    * [snowflake.ml.model.batch](model_batch)
      * [snowflake.ml.model.batch.InputSpec](api/model_batch/snowflake.ml.model.batch.InputSpec)
      * [snowflake.ml.model.batch.OutputSpec](api/model_batch/snowflake.ml.model.batch.OutputSpec)
      * [snowflake.ml.model.batch.JobSpec](api/model_batch/snowflake.ml.model.batch.JobSpec)
      * [snowflake.ml.model.batch.SaveMode](api/model_batch/snowflake.ml.model.batch.SaveMode)
      * [snowflake.ml.model.batch.InputFormat](api/model_batch/snowflake.ml.model.batch.InputFormat)
      * [snowflake.ml.model.batch.FileEncoding](api/model_batch/snowflake.ml.model.batch.FileEncoding)
      * [snowflake.ml.model.batch.ColumnHandlingOptions](api/model_batch/snowflake.ml.model.batch.ColumnHandlingOptions)
    * [snowflake.ml.modeling](modeling)
      * [snowflake.ml.modeling.calibration](modeling.html#snowflake-ml-modeling-calibration)
      * [snowflake.ml.modeling.cluster](modeling.html#snowflake-ml-modeling-cluster)
      * [snowflake.ml.modeling.compose](modeling.html#snowflake-ml-modeling-compose)
      * [snowflake.ml.modeling.covariance](modeling.html#snowflake-ml-modeling-covariance)
      * [snowflake.ml.modeling.decomposition](modeling.html#snowflake-ml-modeling-decomposition)
      * [snowflake.ml.modeling.discriminant_analysis](modeling.html#snowflake-ml-modeling-discriminant-analysis)
      * [snowflake.ml.modeling.ensemble](modeling.html#snowflake-ml-modeling-ensemble)
      * [snowflake.ml.modeling.feature_selection](modeling.html#snowflake-ml-modeling-feature-selection)
      * [snowflake.ml.modeling.gaussian_process](modeling.html#snowflake-ml-modeling-gaussian-process)
      * [snowflake.ml.modeling.impute](modeling.html#snowflake-ml-modeling-impute)
      * [snowflake.ml.modeling.kernel_approximation](modeling.html#snowflake-ml-modeling-kernel-approximation)
      * [snowflake.ml.modeling.kernel_ridge](modeling.html#snowflake-ml-modeling-kernel-ridge)
      * [snowflake.ml.modeling.lightgbm](modeling.html#snowflake-ml-modeling-lightgbm)
      * [snowflake.ml.modeling.linear_model](modeling.html#snowflake-ml-modeling-linear-model)
      * [snowflake.ml.modeling.manifold](modeling.html#snowflake-ml-modeling-manifold)
      * [snowflake.ml.modeling.metrics](modeling.html#snowflake-ml-modeling-metrics)
      * [snowflake.ml.modeling.mixture](modeling.html#snowflake-ml-modeling-mixture)
      * [snowflake.ml.modeling.model_selection](modeling.html#snowflake-ml-modeling-model-selection)
      * [snowflake.ml.modeling.multiclass](modeling.html#snowflake-ml-modeling-multiclass)
      * [snowflake.ml.modeling.naive_bayes](modeling.html#snowflake-ml-modeling-naive-bayes)
      * [snowflake.ml.modeling.neighbors](modeling.html#snowflake-ml-modeling-neighbors)
      * [snowflake.ml.modeling.neural_network](modeling.html#snowflake-ml-modeling-neural-network)
      * [snowflake.ml.modeling.pipeline](modeling.html#snowflake-ml-modeling-pipeline)
      * [snowflake.ml.modeling.preprocessing](modeling.html#snowflake-ml-modeling-preprocessing)
      * [snowflake.ml.modeling.semi_supervised](modeling.html#snowflake-ml-modeling-semi-supervised)
      * [snowflake.ml.modeling.svm](modeling.html#snowflake-ml-modeling-svm)
      * [snowflake.ml.modeling.tree](modeling.html#snowflake-ml-modeling-tree)
      * [snowflake.ml.modeling.xgboost](modeling.html#snowflake-ml-modeling-xgboost)
    * [snowflake.ml.monitoring](monitoring)
      * [snowflake.ml.monitoring.model_monitor](monitoring.html#snowflake-ml-monitoring-model-monitor)
      * [snowflake.ml.monitoring.entities](monitoring.html#snowflake-ml-monitoring-entities)
      * [snowflake.ml.monitoring.explain_visualize](monitoring.html#snowflake-ml-monitoring-explain-visualize)
    * [snowflake.ml.registry](registry)
      * [snowflake.ml.registry.Registry](api/registry/snowflake.ml.registry.Registry)
  * [ML APIs available in `snowflake-ml-python` running on Container Runtime](index-container)
    * [Distributed Modeling Classes](modeling_distributors)
      * [`snowflake.ml.modeling.distributors.xgboost.XGBEstimator`](modeling_distributors.html#snowflake-ml-modeling-distributors-xgboost-xgbestimator)
      * [`snowflake.ml.modeling.distributors.lightgbm.LightGBMEstimator`](modeling_distributors.html#snowflake-ml-modeling-distributors-lightgbm-lightgbmestimator)
      * [`snowflake.ml.modeling.distributors.pytorch.PyTorchDistributor`](modeling_distributors.html#snowflake-ml-modeling-distributors-pytorch-pytorchdistributor)
    * [Sharded Data Connector](data_sharded_data_connector)
      * [`snowflake.ml.data_sharded_data_connector.ShardedDataConnector`](data_sharded_data_connector.html#snowflake-ml-data-sharded-data-connector-shardeddataconnector)
  * [Snowflake Cortex APIs in `snowflake-ml-python`](index-cortex)
    * [snowflake.cortex](cortex)
      * [snowflake.cortex.CompleteOptions](api/cortex/snowflake.cortex.CompleteOptions)
      * [snowflake.cortex.Finetune](api/cortex/snowflake.cortex.Finetune)
      * [snowflake.cortex.FinetuneJob](api/cortex/snowflake.cortex.FinetuneJob)
      * [snowflake.cortex.FinetuneStatus](api/cortex/snowflake.cortex.FinetuneStatus)
      * [snowflake.cortex.classify_text](api/cortex/snowflake.cortex.classify_text)
      * [snowflake.cortex.complete](api/cortex/snowflake.cortex.complete)
      * [snowflake.cortex.embed_text_1024](api/cortex/snowflake.cortex.embed_text_1024)
      * [snowflake.cortex.embed_text_768](api/cortex/snowflake.cortex.embed_text_768)
      * [snowflake.cortex.extract_answer](api/cortex/snowflake.cortex.extract_answer)
      * [snowflake.cortex.sentiment](api/cortex/snowflake.cortex.sentiment)
      * [snowflake.cortex.summarize](api/cortex/snowflake.cortex.summarize)
      * [snowflake.cortex.translate](api/cortex/snowflake.cortex.translate)
