---
title: "sfguide-getting-started-with-snowflake-feature-store/Step04_TPCXAI_UC01_Cleanup.ipynb at main · Snowflake-Labs/sfguide-getting-started-with-snowflake-feature-store · GitHub"
source: https://github.com/Snowflake-Labs/sfguide-getting-started-with-snowflake-feature-store/tree/main/Step04_TPCXAI_UC01_Cleanup.ipynb
cert_domain: domain-2-data-preparation-and-feature-engineering
crawl_depth: 1
crawled: 2026-08-23
note: fetched from raw.githubusercontent
---

Version: 0.0.2  Updated date: 07/05/2024
Conda Environment : py-snowpark_df_ml_fs-1.15.0_v1

# Getting Started with Snowflake Feature Store -  Customer Segmentation

## Setup Database Environment

This Notebook is used to teardown the environment created for the QuickStart when you no longer need it.

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
import datetime


# SNOWFLAKE
# Snowpark
from snowflake.snowpark import Session, DataFrame, Window, WindowSpec
import snowflake.snowpark.functions as F
import snowflake.snowpark.types as T
from snowflake.snowpark.version import VERSION
from snowflake.ml.utils import connection_params

from useful_fns import run_sql
```

### Setup Snowflake connection and database parameters

Change the settings below if you want to if need to apply to your Snowflake Account.

E.g. if you need to use a different role with ACCOUNTADMIN privileges to setup the environment

```python
# Roles
aa_role = 'ACCOUNTADMIN'        # Either a ACCOUNTADMIN, or another role that has been granted ACCOUNTADMIN privileges
fs_qs_role = 'FS_QS_ROLE'       # The Data-Scientist role that will create an have permissions over the data, Feature-Store and Model-Registry

# Database
scale_factor               = 'SF0001'  # TPCXAI data comes in a number of Scale Factors.  For this quickstart we are using the lowest Scale Factor
tpcxai_database_base       = f'TPCXAI_{scale_factor}_QUICKSTART' # The Database we will create to contain the base static data
tpcxai_database_inc        = f'{tpcxai_database_base}_INC' # The Database we will create to contain the pseudo 'Live' incrementing data
databases = [tpcxai_database_base,tpcxai_database_inc]

# Schemas
tpcxai_config_schema       = 'CONFIG'   # This Config Schema containing database artifacts to manage the Quickstart databases
tpcxai_training_schema     = 'TRAINING' # The Training (Development) schema
tpcxai_scoring_schema      = 'SCORING'  # The Scoring (Test) schema
tpcxai_serving_schema      = 'SERVING'  # The Serving (Production) schema
schemas = [tpcxai_training_schema, tpcxai_scoring_schema, tpcxai_serving_schema,]
fq_schemas = []
for d in databases:
    for s in schemas:
        fq_schemas.append(f'''{d}.{s}''')        

# Warehouse
tpcxai_warehouse = f'TPCXAI_{scale_factor}_QUICKSTART_WH'  # The name of the Warehouse we will use for any Quickstart processing
initial_wh_size = 'XSMALL' 
```

```python
# Create Snowflake Session object
connection_parameters = connection_params.SnowflakeLoginOptions("ak32940")
session = Session.builder.configs(connection_parameters).create()
session.sql_simplifier_enabled = True
snowflake_environment = session.sql('SELECT current_user(), current_version()').collect()
snowpark_version = VERSION
```

```python
# Current Environment Details
print('\nConnection Established with the following parameters:')
print(f'User                        : {session.get_current_user()}')
print(f'Role                        : {session.get_current_role()}')
print(f'Database                    : {session.get_current_database()}')
print(f'Schema                      : {session.get_current_schema()}')
print(f'Warehouse                   : {session.get_current_warehouse()}')
print(f'Snowpark for Python version : {snowpark_version[0]}.{snowpark_version[1]}.{snowpark_version[2]} \n')
```

## CLEAN UP

```python
run_sql(f'''use role {aa_role}''', session)
```

```python
run_sql(f'''drop database {tpcxai_database_base}''', session)
```

```python
run_sql(f'''drop database {tpcxai_database_inc}''', session)
```

```python
run_sql(f'''drop warehouse {tpcxai_warehouse}''', session)
```

```python
session.close()
```

