---
title: "sfguide-getting-started-with-snowflake-feature-store/Step03_TPCXAI_UC01_Operations.ipynb at main · Snowflake-Labs/sfguide-getting-started-with-snowflake-feature-store · GitHub"
source: https://github.com/Snowflake-Labs/sfguide-getting-started-with-snowflake-feature-store/tree/main/Step03_TPCXAI_UC01_Operations.ipynb
cert_domain: domain-2-data-preparation-and-feature-engineering
crawl_depth: 1
crawled: 2026-08-23
note: fetched from raw.githubusercontent
---

Version: 0.0.2  Updated date: 07/05/2024
Conda Environment : py-snowpark_df_ml_fs-1.15.0_v1

# Getting Started with Snowflake Feature Store -  Customer Segmentation

The Customer segmentation (UC01) use case is designed to emulate a data science pipeline to find clusters of customers based  on  aggregate  features where  the  customers  are  grouped  based  on  their  spending behavior. <br>

It  involves  creating subgroups of customers based on similar traits. <br>

The input in this use case consists of order and return transaction data from a retail business. <br>

The use case uses Tables Customer, Order, Lineitem and Order_returns. <br>

K-means  clustering  algorithm  is  used  to  derive  the  optimum  number  of  clusters  and  understand  the  underlying customer segments based on the data provided. <br>
Clustering is an unsupervised machine learning technique, where there are no defined dependent and independent variables, i.e. the training samples are unlabeled. <br>
The pattern in the data is used to identify and group similar observations. <br>

We will use the Use-Case to show how Snowflake Feature Store (and Model Registry) can be used to maintain & store features, retrieve them for training and perform micro-batch inference.

In the development (TRAINING) enviroment we will 
- create FeatureViews in the Feature Store that maintain the required customer-behaviour features.
- use these Features to train a model, and save the model in the Snowflake model-registry.
- plot the clusters for the trained model to visually verify. 

In the production (SERVING) environment we will
- re-create the FeatureViews on production data
- generate an Inference FeatureView that uses the saved model to perform incremental inference

# Model Operationalisation in Production

```python
%load_ext autoreload
%autoreload 2
```

#### Notebook Packages

```python
# Python packages
import os
from os import listdir
from os.path import isfile, join
import time
import json
import timeit
import numpy as np
import pandas as pd
import tabulate
import datetime
import ast      
import sqlglot
import sqlglot.optimizer.optimizer

# SNOWFLAKE
# Snowpark
from snowflake.snowpark import Session, DataFrame, Window, WindowSpec
#from snowflake.snowpark import Analytics

import snowflake.snowpark.functions as F
import snowflake.snowpark.types as T
from snowflake.snowpark.version import VERSION

# Snowflake Feature Store
from snowflake.ml.feature_store import (
    FeatureStore,
    FeatureView,
    Entity,
    CreationMode)

# Snowflake Model Registry
from snowflake.ml.registry import Registry
from snowflake.ml.utils import connection_params
from snowflake.ml._internal.utils import identifier  


# COMMON FUNCTIONS
from useful_fns import check_and_update, formatSQL, create_ModelRegistry, create_FeatureStore 

#### Use-Case 01 - Specific Packages
# K-Means clustering
#from sklearn.pipeline import Pipeline as skl_Pipeline
from snowflake.ml.modeling.pipeline import Pipeline as sml_Pipeline
#from sklearn.preprocessing import MinMaxScaler as skl_MinMaxScaler
from snowflake.ml.modeling.preprocessing import MinMaxScaler as sml_MinMaxScaler
#from sklearn.cluster import KMeans as skl_KMeans
from snowflake.ml.modeling.cluster import KMeans as sml_KMeans

# Feature Engineering Functions
from feature_engineering_fns import uc01_load_data, uc01_pre_process
```

### Setup Snowflake connection and database parameters

```python
# Scale Factor
scale_factor               = 'SF0001'

# Roles
fs_qs_role                 = 'FS_QS_ROLE'

# Database
tpcxai_database_base       = f'TPCXAI_{scale_factor}_QUICKSTART'
tpcxai_database            = f'{tpcxai_database_base}_INC'

# Schemas
tpcxai_training_schema     = 'TRAINING'
tpcxai_scoring_schema      = 'SCORING'
tpcxai_serving_schema      = 'SERVING'
```

We point the `tpcxai_schema` variable to our `SERVING` schema, and this one change allows us to recreate the model development pipeline in production.

```python
# Set the Schema (Environment)
tpcxai_schema = tpcxai_serving_schema
```

```python
connection_parameters = connection_params.SnowflakeLoginOptions("ak32940")

session = Session.builder.configs(connection_parameters).create()
session.sql_simplifier_enabled = True
snowflake_environment = session.sql('SELECT current_user(), current_version()').collect()
snowpark_version = VERSION

# Set  Environment
session.sql(f'''use database {tpcxai_database}''').collect()
session.sql(f'''use schema {tpcxai_schema}''').collect()
session.sql(f'''use role {fs_qs_role}''').collect()

# Create a Warehouse
warehouse_sz = 'MEDIUM'
warehouse_env = f'TPCXAI_{scale_factor}_QUICKSTART_WH'
session.sql(f'''use warehouse {warehouse_env}''').collect()
session.sql(f'''alter warehouse {warehouse_env} set warehouse_size = {warehouse_sz}''').collect()

# Current Environment Details
print('\nConnection Established with the following parameters:')
print(f'User                        : {snowflake_environment[0][0]}')
print(f'Role                        : {session.get_current_role()}')
print(f'Database                    : {session.get_current_database()}')
print(f'Schema                      : {session.get_current_schema()}')
print(f'Warehouse                   : {session.get_current_warehouse()}')
print(f'Snowflake version           : {snowflake_environment[0][1]}')
print(f'Snowpark for Python version : {snowpark_version[0]}.{snowpark_version[1]}.{snowpark_version[2]} \n')
```

### MODEL OPERATIONALISATION
* Recreate production Entity, FeatureViews in Production FeatureStore
* Reuse the model fitted in development/training
* Create new Inference FeatureView for incremental model-inference

#### Setup Production Feature Store and references

```python
# Create/Reference Snowflake Model Registry - Common across Environments
mr = create_ModelRegistry(session, tpcxai_database, '_MODEL_REGISTRY')

# Create/Reference Snowflake Feature Store for Training (Development) Environment
fs = create_FeatureStore(session, tpcxai_database, f'''_{tpcxai_schema}_FEATURE_STORE''', warehouse_env)

### Reference Data to Snowflake Dataframe Objects
# Tables
customer_tbl               = '.'.join([tpcxai_database, tpcxai_schema,'CUSTOMER'])
line_item_tbl              = '.'.join([tpcxai_database, tpcxai_schema,'LINEITEM'])
order_tbl                  = '.'.join([tpcxai_database, tpcxai_schema,'ORDERS'])
order_returns_tbl          = '.'.join([tpcxai_database, tpcxai_schema,'ORDER_RETURNS'])

# Snowpark Dataframe
customer_sdf               = session.table(customer_tbl)
line_item_sdf              = session.table(line_item_tbl)
order_sdf                  = session.table(order_tbl)
order_returns_sdf          = session.table(order_returns_tbl)
print('''--- Created Data References ---''')

# Model Name
model_name = "UC01_SNOWFLAKEML_KMEANS_MODEL"

```

We can now rerun the exact same code that we lifted from our Development (TRAINING) process to recreate the Feature Engineering pipelines in production

```python
### CUSTOMER Entity
if "CUSTOMER" not in json.loads(fs.list_entities().select(F.to_json(F.array_agg("NAME", True))).collect()[0][0]):
    customer_entity = Entity(name="CUSTOMER", join_keys=["O_CUSTOMER_SK"],desc="Primary Key for CUSTOMER")
    fs.register_entity(customer_entity)
else:
    customer_entity = fs.get_entity("CUSTOMER")
print('''--- Created CUSTOMER Entity ---''')

### Create & Load Source Data
raw_data = uc01_load_data(order_sdf, line_item_sdf, order_returns_sdf)
rd_sql = formatSQL(raw_data.queries['queries'][0], True)
print('''--- Created Source Data ---''')

### Create & Run Preprocessing Function 
preprocessed_data = uc01_pre_process(raw_data)
ppd_sql = formatSQL(preprocessed_data.queries['queries'][0], True)
print('''--- Created Preprocessed Data ---''')

### Create Preprocessing FeatureView from Preprocess Dataframe (SQL)
ppd_fv_name = "FV_UC01_PREPROCESS"
ppd_fv_version = "V_1"
# Define descriptions for the FeatureView's Features.  These will be added as comments to the database object
preprocess_features_desc = { "FREQUENCY":"Average yearly order frequency",
                             "RETURN_RATIO":"Average of, Per Order Returns Ratio.  Per order returns ratio : total returns value / total order value" }
# Create Inference Feature View
try:
    # If FeatureView already exists just return the reference to it
    fv_uc01_preprocess = fs.get_feature_view(name=ppd_fv_name,version=ppd_fv_version)
except:
    # Create the FeatureView instance
    fv_uc01_preprocess_instance = FeatureView(
        name=ppd_fv_name, 
        entities=[customer_entity], 
        #feature_df=preprocessed_data,      # <- We can use the snowpark dataframe as-is from our Python
        feature_df=session.sql(ppd_sql),    # <- Or we can use SQL, in this case linted from the dataframe generated SQL to make more human readable
        timestamp_col="LATEST_ORDER_DATE",
        refresh_freq="60 minute",           # <- specifying optional refresh_freq creates FeatureView as Dynamic Table, else created as View.
        desc="Features to support Use Case 01").attach_feature_desc(preprocess_features_desc)

    # Register the FeatureView instance.  Creates  object in Snowflake
    fv_uc01_preprocess = fs.register_feature_view(
        feature_view=fv_uc01_preprocess_instance, 
        version=ppd_fv_version, 
        block=True
    )
    print(f"Feature View : {ppd_fv_name}_{ppd_fv_version} created in {tpcxai_schema}")   
else:
    print(f"Feature View : {ppd_fv_name}_{ppd_fv_version} already created in {tpcxai_schema}")

print('''---            DONE               ---''')

```

#### Create Scheduled Inference Pipeline

We now recreate our model inference process that will
- retrieve the latest version of the model from the Model Registry.
- read features from our feature pipeline (fv_uc01_preprocess featureview)
- pass features & model into inference function (uc01_serve) and return inference dataframe
- use inference dataframe to define a new FeatureView to maintain inference process

```python
def uc01_serve(featurevector, km4_purchases) -> DataFrame:
    return km4_purchases.run(featurevector, function_name="predict")
```

```python
# Create an Inference Dataframe that reads from our feature-engineering pipeline
inference_input_sdf = fs.read_feature_view(fv_uc01_preprocess)
inference_input_sdf.show()
```

```python
# Get latest version of the model
m = mr.get_model(model_name)
latest_version = m.show_versions().iloc[-1]['name']
mv = m.version(latest_version)
```

```python
# Test Inference process
inference_result_sdf = uc01_serve(inference_input_sdf, mv)
inference_result_sdf.sort(F.col('LATEST_ORDER_DATE').desc(), F.col('O_CUSTOMER_SK')).show()
```

We can see in the SQL output below how our model is packaged and called from SQL `MODEL_VERSION_ALIAS!PREDICT(RETURN_RATIO, FREQUENCY) AS TMP_RESULT`

```python
ind_sql = inference_result_sdf.queries['queries'][0]
ind_fmtd_sql = os.linesep.join(ind_sql.split(os.linesep)[:1000])
print(ind_fmtd_sql)
```

### Create & Register Inference-FeatureView to run scheduled Inference

We can now define a new Inference Feature View using our Spine and Dataframe reading from our Feature Engineering pipeline.  The FeatureView when created as a Dynamic Table will run to the required refresh_freq and automatically perform incremental inference on new data that arrives through the pipeline.

```python
## Create & Register Inference-FeatureView to run scheduled Inference
inf_fvname = "FV_UC01_INFERENCE_RESULT"
inf_fv_version = "V_2"

inference_features_desc = { "FREQUENCY":"Average yearly order frequency",
                              "RETURN_RATIO":"Average of, Per Order Returns Ratio.  Per order returns ratio : total returns value / total order value", 
                              "RETURN_RATIO_MMS":f"Min/Max Scaled version of RETURN_RATIO using Model Registry ({tpcxai_database}_MODEL_REGISTRY) Model ({mv.model_name}) Model-Version({mv.version_name}) Model Comment ({mv.comment})",
                              "FREQUENCY_MMS":f"Min/Max Scaled version of FREQUENCY using Model Registry ({tpcxai_database}_MODEL_REGISTRY) Model ({mv.model_name}) Model-Version({mv.version_name})  Model Comment ({mv.comment}",
                              "CLUSTER":f"Kmeans Cluster for Customer Clustering Model (UC01) using Model Registry ({tpcxai_database}_MODEL_REGISTRY) Model ({mv.model_name}) Model-Version({mv.version_name})  Model Comment ({mv.comment}"}

try:
   fv_uc01_inference_result = fs.get_feature_view(name= inf_fvname, version= inf_fv_version)
except:
   fv_uc01_inference_result = FeatureView(
         name= inf_fvname, 
         entities=[customer_entity], 
         feature_df=inference_result_sdf,
         ## refresh_freq="60 minute",
         desc="Inference Result from kmeans model for Use Case 01").attach_feature_desc(inference_features_desc)
   
   fv_uc01_inference_result = fs.register_feature_view(
         feature_view=fv_uc01_inference_result, 
         version= inf_fv_version, 
         block=True
   )
   print(f"Inference Feature View : fv_uc01_inference_result_{inf_fv_version} created")   
else:
   print(f"Inference Feature View : fv_uc01_inference_result_{inf_fv_version} already created")
finally:
   fs_serving_fviews = fs.list_feature_views().filter(F.col("NAME") == inf_fvname ).sort(F.col("VERSION").desc())
   fs_serving_fviews.show()  
```

```python
fv_uc01_inference_result
```

```python
fv_uc01_inference_result.feature_df.sort(F.col("LATEST_ORDER_DATE").desc()).show(100)
```

## CLEAN UP

```python
session.close()
```

```python

```

