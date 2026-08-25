---
title: "sfguide-getting-started-with-snowflake-feature-store/Step01_TPCXAI_UC01_Setup.ipynb at main · Snowflake-Labs/sfguide-getting-started-with-snowflake-feature-store · GitHub"
source: https://github.com/Snowflake-Labs/sfguide-getting-started-with-snowflake-feature-store/tree/main/Step01_TPCXAI_UC01_Setup.ipynb
cert_domain: domain-2-data-preparation-and-feature-engineering
crawl_depth: 1
crawled: 2026-08-23
note: fetched from raw.githubusercontent
---

Version: 0.0.2  Updated date: 07/05/2024
Conda Environment : py-snowpark_df_ml_fs-1.15.0_v1

# Getting Started with Snowflake Feature Store -  Customer Segmentation

## Setup Database Environment

This Notebook is used to setup the required database objects including the source data for this use-case.

We will create :
- a database that will store all our database artifacts and raw source data
- a database that will be setup to simulate ingesting of raw source data in an incrementing fashion
- schemas to simulate different environments for development (TRAINING*) and production (SERVING*)
- a data-scientist role that will have permissions to 
    - develop features and train models in our development schemas
    - productionize our feature-engineering and ML pipeline in the production schemas

In a 'live' environment you may have several roles with different permissions over Development and Production that are used to maintain separation of concerns.


```python
%load_ext autoreload
%autoreload 2
```

**Install SQLGlot** <br>
Install SQLGlot with pip install in the conda environment **py-snowpark_df_ml_fs** by running the following command in the same terminal window.  We will use this package to format the SQL produced from Snowpark so that it is human-readable in the Dynamic Tables that Feature Store creates.  Installing within the Notebook, as other users have reported issues trying to install directly within the OS.

```python
!python3 -m pip install "sqlglot[rs]" --no-deps
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

# S3 bucket - public access bucket containing source data files
s3_bucket = f's3://sfquickstarts/getting_started_with_snowflake_feature_store/'
# Stage
tpcxai_internal_stage = 'TPCXAI_STAGE'  # The Stage name we will use to represent the S3 bucket

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
print(f'User                        : {snowflake_environment[0][0]}')
print(f'Role                        : {session.get_current_role()}')
print(f'Database                    : {session.get_current_database()}')
print(f'Schema                      : {session.get_current_schema()}')
print(f'Warehouse                   : {session.get_current_warehouse()}')
print(f'Snowflake version           : {snowflake_environment[0][1]}')
print(f'Snowpark for Python version : {snowpark_version[0]}.{snowpark_version[1]}.{snowpark_version[2]} \n')
```

```python
run_sql(f'''use role {aa_role}''', session)
```

```python
# Setup master role and permissions
run_sql(f'''use role {aa_role}''', session)

# Role
run_sql(f'''create role if not exists {fs_qs_role}''', session)
run_sql(f'''grant role {fs_qs_role} to role SYSADMIN''', session)

# Warehouse
run_sql(f'''create warehouse if not exists {tpcxai_warehouse}  warehouse_size = {initial_wh_size}''', session)
run_sql(f'''grant all on warehouse {tpcxai_warehouse} to role {fs_qs_role}''', session)
run_sql(f'''use warehouse {tpcxai_warehouse}''', session)

# Tasks
run_sql(f'''grant execute managed task on account to role {fs_qs_role}''', session)
run_sql(f'''grant execute task on account to role {fs_qs_role}''', session)

```

```python
# Database -  BASE
run_sql(f'''use role {aa_role}''', session)

run_sql(f'''create database if not exists {tpcxai_database_base}''', session)
run_sql(f'''grant all on database {tpcxai_database_base} to role {fs_qs_role}''', session)
run_sql(f'''grant all on all schemas in database {tpcxai_database_base} to role {fs_qs_role}''', session)
run_sql(f'''grant all on future schemas in database {tpcxai_database_base} to role {fs_qs_role}''', session)

run_sql(f'''create database if not exists {tpcxai_database_inc}''', session)
run_sql(f'''grant all on database {tpcxai_database_inc} to role {fs_qs_role}''', session)
run_sql(f'''grant all on all schemas in database {tpcxai_database_inc} to role {fs_qs_role}''', session)
run_sql(f'''grant all on future schemas in database {tpcxai_database_inc} to role {fs_qs_role}''', session)

```

```python
# Objects in BASE database
run_sql(f'''use role {fs_qs_role}''', session)

run_sql(f'''create schema if not exists {tpcxai_database_base}.{tpcxai_training_schema}''', session)
run_sql(f'''use schema {tpcxai_database_base}.{tpcxai_training_schema}''', session)

run_sql(f'''create schema  if not exists {tpcxai_database_base}.{tpcxai_serving_schema}''', session)
run_sql(f'''use schema {tpcxai_database_base}.{tpcxai_serving_schema}''', session)

run_sql(f'''create schema  if not exists {tpcxai_database_base}.{tpcxai_scoring_schema}''', session)
run_sql(f'''use schema {tpcxai_database_base}.{tpcxai_scoring_schema}''', session)

run_sql(f'''create schema  if not exists {tpcxai_database_base}.{tpcxai_config_schema}''', session)
run_sql(f'''use schema {tpcxai_database_base}.{tpcxai_config_schema}''', session)

run_sql(f'''create file format if not exists {tpcxai_database_base}.{tpcxai_config_schema}.parquet_ff type = 'parquet' ''', session)

run_sql(f'''create stage if not exists {tpcxai_database_base}.{tpcxai_config_schema}.{tpcxai_internal_stage} 
        file_format = {tpcxai_database_base}.{tpcxai_config_schema}.parquet_ff 
        url = '{s3_bucket}' ''', session)

```

```python
run_sql(f'''use role {fs_qs_role}''', session)

# Calculate the DATE point difference between the source data and todays date.  
# This reference point will be used to select a subset of the data for pre-loading, and the remainder will be incrementally ingested via a scheduled task.
date_diff_to_source = session.sql('''select timestampdiff('days',  '2013-04-01', CURRENT_DATE() )::VARCHAR date_diff_to_source''').collect()[0][0]
print('Difference in Days between source data and current date :',date_diff_to_source)

## STATIC DATABASE SETUP - TPCXAI_SF001_QUICKSTART

# Iteration over the Schemas creating and loading the required tables in each
for s in [tpcxai_training_schema, tpcxai_serving_schema, tpcxai_scoring_schema]:

    # CUSTOMER
    run_sql(f'''CREATE OR REPLACE TABLE {tpcxai_database_base}.{s}.CUSTOMER
                  (C_CUSTOMER_SK INTEGER,
                   C_CUSTOMER_ID VARCHAR,
                   C_CURRENT_ADDR_SK INTEGER,
                   C_FIRST_NAME VARCHAR,
                   C_LAST_NAME VARCHAR,
                   C_PREFERRED_CUST_FLAG VARCHAR,
                   C_BIRTH_DAY INTEGER,
                   C_BIRTH_MONTH INTEGER,
                   C_BIRTH_YEAR INTEGER,
                   C_BIRTH_COUNTRY VARCHAR,
                   C_LOGIN VARCHAR,
                   C_EMAIL_ADDRESS VARCHAR,
                   C_CLUSTER_ID INTEGER
                  ) CLUSTER BY (C_CUSTOMER_SK);
    ''', session)
    run_sql(f'''COPY INTO {tpcxai_database_base}.{s}.CUSTOMER 
            FROM 
                 (select $1:C_CUSTOMER_SK::INTEGER,
                         $1:C_CUSTOMER_ID::VARCHAR,
                         $1:C_CURRENT_ADDR_SK::INTEGER,
                         $1:C_FIRST_NAME::VARCHAR,
                         $1:C_LAST_NAME::VARCHAR,
                         $1:C_PREFERRED_CUST_FLAG::VARCHAR,
                         $1:C_BIRTH_DAY::INTEGER,
                         $1:C_BIRTH_MONTH::INTEGER,
                         $1:C_BIRTH_YEAR::INTEGER,
                         $1:C_BIRTH_COUNTRY::VARCHAR,
                         $1:C_LOGIN::VARCHAR,
                         $1:C_EMAIL_ADDRESS::VARCHAR,                        
                         $1:C_CLUSTER_ID::INTEGER
                  from  @{tpcxai_database_base}.CONFIG.{tpcxai_internal_stage}/{s}/CUSTOMER) 
            FILE_FORMAT = (FORMAT_NAME = '{tpcxai_database_base}.{tpcxai_config_schema}.parquet_ff' ) ''', session) 
    
    # ORDERS
    run_sql(f'''CREATE OR REPLACE TABLE {tpcxai_database_base}.{s}.ORDERS
    (O_ORDER_ID INTEGER,
     O_CUSTOMER_SK INTEGER,
     ORDER_TS TIMESTAMP,
     WEEKDAY VARCHAR,
     ORDER_DATE DATE,
     STORE INTEGER,
     TRIP_TYPE INTEGER)
     CLUSTER BY (O_ORDER_ID, ORDER_DATE)
    ''', session)
    run_sql(f'''COPY INTO {tpcxai_database_base}.{s}.ORDERS 
            FROM 
                 (select $1:O_ORDER_ID::INTEGER,
                         $1:O_CUSTOMER_SK::INTEGER,
                         timestampadd('MINS', UNIFORM( -1440 , 0 , random() ) ,timestampadd('days',   {date_diff_to_source}, $1:"DATE"::DATE)) ORDER_TS,
                         decode(extract(dayofweek from ORDER_TS), 1, 'Monday', 2, 'Tuesday', 3, 'Wednesday', 4, 'Thursday',  5, 'Friday',  6, 'Saturday',  0, 'Sunday') WEEKDAY,
                         TO_DATE(ORDER_TS) ORDER_DATE,
                         $1:STORE::INTEGER,
                         $1:TRIP_TYPE::INTEGER
                  from  @{tpcxai_database_base}.CONFIG.{tpcxai_internal_stage}/{s}/ORDERS) 
            FILE_FORMAT = (FORMAT_NAME = '{tpcxai_database_base}.{tpcxai_config_schema}.parquet_ff' ) ''', session) 
    
    # LINEITEM
    run_sql(f'''CREATE OR REPLACE TABLE {tpcxai_database_base}.{s}.LINEITEM
    (LI_ORDER_ID INTEGER,
     LI_PRODUCT_ID INTEGER,
     QUANTITY INTEGER,
     PRICE DECIMAL(8,2))
    CLUSTER BY (LI_PRODUCT_ID, LI_ORDER_ID)
    ''', session)
    run_sql(f'''COPY INTO {tpcxai_database_base}.{s}.LINEITEM 
            FROM (select $1:LI_ORDER_ID::INTEGER,
                         $1:LI_PRODUCT_ID::INTEGER,
                         $1:QUANTITY::INTEGER,
                         $1:PRICE::DECIMAL(8,2)
                  from @{tpcxai_database_base}.CONFIG.{tpcxai_internal_stage}/{s}/LINEITEM) 
            FILE_FORMAT = (FORMAT_NAME = '{tpcxai_database_base}.{tpcxai_config_schema}.parquet_ff' ) ''', session) 
    
    # ORDER_RETURNS
    run_sql(f'''CREATE OR REPLACE TABLE {tpcxai_database_base}.{s}.ORDER_RETURNS
    (OR_ORDER_ID INTEGER,
     OR_PRODUCT_ID INTEGER,
     OR_RETURN_QUANTITY INTEGER)
     CLUSTER BY (OR_PRODUCT_ID, OR_ORDER_ID);
    ''', session)
    run_sql(f'''COPY INTO {tpcxai_database_base}.{s}.ORDER_RETURNS 
            FROM (select $1:OR_ORDER_ID::INTEGER,
                         $1:OR_PRODUCT_ID::INTEGER,
                         $1:OR_RETURN_QUANTITY::INTEGER
                  from  @{tpcxai_database_base}.CONFIG.{tpcxai_internal_stage}/{s}/ORDER_RETURNS) 
            FILE_FORMAT = (FORMAT_NAME = '{tpcxai_database_base}.{tpcxai_config_schema}.parquet_ff' ) ''', session) 
    


```

```python
## INCREMENTAL DATABASE SETUP - TPCXAI_SF001_QUICKSTART_INC

# Training schema
run_sql(f'''use role {fs_qs_role}''', session)
run_sql(f'''create schema if not exists {tpcxai_database_inc}.{tpcxai_training_schema}''', session)
run_sql(f'''grant usage on schema {tpcxai_database_inc}.{tpcxai_training_schema} to role {fs_qs_role}''', session)
run_sql(f'''grant create table on schema {tpcxai_database_inc}.{tpcxai_training_schema} to role {fs_qs_role}''', session)
run_sql(f'''grant create view on schema {tpcxai_database_inc}.{tpcxai_training_schema} to role {fs_qs_role}''', session)
run_sql(f'''grant create tag on schema {tpcxai_database_inc}.{tpcxai_training_schema} to role {fs_qs_role}''', session)
run_sql(f'''grant create dataset on schema {tpcxai_database_inc}.{tpcxai_training_schema} to {fs_qs_role}''', session)
run_sql(f'''grant select,references on all views in schema {tpcxai_database_inc}.{tpcxai_training_schema} to role {fs_qs_role}''', session)
# run_sql(f'''grant select,references on future views in schema {tpcxai_database_inc}.{tpcxai_training_schema} to role {fs_qs_role}''')
run_sql(f'''grant create dynamic table on schema {tpcxai_database_inc}.{tpcxai_training_schema} to role {fs_qs_role}''', session)
run_sql(f'''grant select,monitor on all dynamic tables in schema {tpcxai_database_inc}.{tpcxai_training_schema} to role {fs_qs_role}''', session)
#run_sql(f'''grant select,monitor on future dynamic tables in schema {tpcxai_database_inc}.{tpcxai_training_schema} to role {fs_qs_role}''')
run_sql(f'''grant usage on all datasets in schema {tpcxai_database_inc}.{tpcxai_training_schema} to role {fs_qs_role}''', session)
#run_sql(f'''grant usage on future datasets in schema {tpcxai_database_inc}.{tpcxai_training_schema} to role {fs_qs_role}''')

# Serving schema
run_sql(f'''use role {fs_qs_role}''', session)
run_sql(f'''create schema if not exists {tpcxai_database_inc}.{tpcxai_serving_schema}''', session)
run_sql(f'''grant usage on schema {tpcxai_database_inc}.{tpcxai_serving_schema} to role {fs_qs_role}''', session)
run_sql(f'''grant create table on schema {tpcxai_database_inc}.{tpcxai_serving_schema} to role {fs_qs_role}''', session)
run_sql(f'''grant create view on schema {tpcxai_database_inc}.{tpcxai_serving_schema} to role {fs_qs_role}''', session)
run_sql(f'''grant create tag on schema {tpcxai_database_inc}.{tpcxai_serving_schema} to role {fs_qs_role}''', session)
run_sql(f'''grant create dataset on schema {tpcxai_database_inc}.{tpcxai_serving_schema} to {fs_qs_role}''', session)
run_sql(f'''grant select,references on all views in schema {tpcxai_database_inc}.{tpcxai_serving_schema} to role {fs_qs_role}''', session)
#run_sql(f'''grant select,references on future views in schema {tpcxai_database_inc}.{tpcxai_serving_schema} to role {fs_qs_role}''')
run_sql(f'''grant create dynamic table on schema {tpcxai_database_inc}.{tpcxai_serving_schema} to role {fs_qs_role}''', session)
run_sql(f'''grant select,monitor on all dynamic tables in schema {tpcxai_database_inc}.{tpcxai_serving_schema} to role {fs_qs_role}''', session)
#run_sql(f'''grant select,monitor on future dynamic tables in schema {tpcxai_database_inc}.{tpcxai_serving_schema} to role {fs_qs_role}''')
run_sql(f'''grant usage on all datasets in schema {tpcxai_database_inc}.{tpcxai_serving_schema} to role {fs_qs_role}''', session)
#run_sql(f'''grant usage on future datasets in schema {tpcxai_database_inc}.{tpcxai_serving_schema} to role {fs_qs_role}''')

# Scoring schema
run_sql(f'''use role {fs_qs_role}''', session)
run_sql(f'''create schema if not exists {tpcxai_database_inc}.{tpcxai_scoring_schema}''', session)
run_sql(f'''grant usage on schema {tpcxai_database_inc}.{tpcxai_scoring_schema} to role {fs_qs_role}''', session)
run_sql(f'''grant create table on schema {tpcxai_database_inc}.{tpcxai_scoring_schema} to role {fs_qs_role}''', session)
run_sql(f'''grant create view on schema {tpcxai_database_inc}.{tpcxai_scoring_schema} to role {fs_qs_role}''', session)
run_sql(f'''grant create tag on schema {tpcxai_database_inc}.{tpcxai_scoring_schema} to role {fs_qs_role}''', session)
run_sql(f'''grant create dataset on schema {tpcxai_database_inc}.{tpcxai_scoring_schema} to {fs_qs_role}''', session)
run_sql(f'''grant select,references on all views in schema {tpcxai_database_inc}.{tpcxai_scoring_schema} to role {fs_qs_role}''', session)
#run_sql(f'''grant select,references on future views in schema {tpcxai_database_inc}.{tpcxai_scoring_schema} to role {fs_qs_role}''')
run_sql(f'''grant create dynamic table on schema {tpcxai_database_inc}.{tpcxai_scoring_schema} to role {fs_qs_role}''', session)
run_sql(f'''grant select,monitor on all dynamic tables in schema {tpcxai_database_inc}.{tpcxai_scoring_schema} to role {fs_qs_role}''', session)
#run_sql(f'''grant select,monitor on future dynamic tables in schema {tpcxai_database_inc}.{tpcxai_scoring_schema} to role {fs_qs_role}''')
run_sql(f'''grant usage on all datasets in schema {tpcxai_database_inc}.{tpcxai_scoring_schema} to role {fs_qs_role}''', session)
#run_sql(f'''grant usage on future datasets in schema {tpcxai_database_inc}.{tpcxai_scoring_schema} to role {fs_qs_role}''')

```

```python
run_sql(f'''use role {aa_role}''', session)
for s in [tpcxai_serving_schema, tpcxai_scoring_schema]:
    run_sql(f'''drop table {tpcxai_database_inc}.{s}.CUSTOMER''', session)
```

```python
run_sql(f'''use role {fs_qs_role}''', session)
# Set up Incremental SERVING & SCORING data maintenance
for s in [tpcxai_serving_schema, tpcxai_scoring_schema]:

    run_sql(f'''CREATE OR REPLACE TABLE {tpcxai_database_inc}.{s}.CUSTOMER
                  (C_CUSTOMER_SK INTEGER,
                   C_CUSTOMER_ID VARCHAR,
                   C_CURRENT_ADDR_SK INTEGER,
                   C_FIRST_NAME VARCHAR,
                   C_LAST_NAME VARCHAR,
                   C_PREFERRED_CUST_FLAG VARCHAR,
                   C_BIRTH_DAY INTEGER,
                   C_BIRTH_MONTH INTEGER,
                   C_BIRTH_YEAR INTEGER,
                   C_BIRTH_COUNTRY VARCHAR,
                   C_LOGIN VARCHAR,
                   C_EMAIL_ADDRESS VARCHAR,
                   C_CLUSTER_ID INTEGER
                  ) CLUSTER BY (C_CUSTOMER_SK)
    ''', session)

    run_sql(f'''insert into {tpcxai_database_inc}.{s}.CUSTOMER select * from {tpcxai_database_base}.{s}.CUSTOMER order by C_CUSTOMER_SK ''', session)
    
    run_sql(f'''CREATE OR REPLACE TABLE {tpcxai_database_inc}.{s}.ORDERS
    (O_ORDER_ID INTEGER,
     O_CUSTOMER_SK INTEGER,
     ORDER_TS TIMESTAMP,
     WEEKDAY VARCHAR,
     ORDER_DATE DATE,
     STORE INTEGER,
     TRIP_TYPE INTEGER)
     CLUSTER BY (O_ORDER_ID, ORDER_TS)
    ''', session)

    run_sql(f'''CREATE OR REPLACE TABLE {tpcxai_database_inc}.{s}.LINEITEM
    (LI_ORDER_ID INTEGER,
     LI_PRODUCT_ID INTEGER,
     QUANTITY INTEGER,
     PRICE DECIMAL(8,2))
    CLUSTER BY (LI_PRODUCT_ID, LI_ORDER_ID)
    ''', session)

    run_sql(f'''CREATE OR REPLACE TABLE {tpcxai_database_inc}.{s}.ORDER_RETURNS
    (OR_ORDER_ID INTEGER,
     OR_PRODUCT_ID INTEGER,
     OR_RETURN_QUANTITY INTEGER)
     CLUSTER BY (OR_PRODUCT_ID, OR_ORDER_ID)
    ''', session)

    # Streams
    run_sql(f''' create or replace stream {tpcxai_database_base}.{tpcxai_config_schema}.{s}_ORDER_LINEITEM_STREAM on table {tpcxai_database_inc}.{s}.ORDERS''', session) 
    run_sql(f''' create or replace stream {tpcxai_database_base}.{tpcxai_config_schema}.{s}_ORDER_ORDERRETURNS_STREAM on table {tpcxai_database_inc}.{s}.ORDERS''', session) 

    run_sql(f''' create or replace task {tpcxai_database_base}.{tpcxai_config_schema}.APPEND_{s}_LINEITEM_TASK
    schedule='1 MINUTE'
	USER_TASK_MANAGED_INITIAL_WAREHOUSE_SIZE='XSMALL'
	when SYSTEM$STREAM_HAS_DATA('{tpcxai_database_base}.{tpcxai_config_schema}.{s}_ORDER_LINEITEM_STREAM')
	as insert into {tpcxai_database_inc}.{s}.LINEITEM
select l.* 
from  {tpcxai_database_base}.{s}.LINEITEM l,
      {tpcxai_database_base}.{tpcxai_config_schema}.{s}_ORDER_LINEITEM_STREAM o
where l.LI_ORDER_ID = o.O_ORDER_ID
order by LI_ORDER_ID, LI_PRODUCT_ID''', session)

    run_sql(f''' alter task {tpcxai_database_base}.{tpcxai_config_schema}.APPEND_{s}_LINEITEM_TASK resume''', session)

    run_sql(f''' create or replace task {tpcxai_database_base}.{tpcxai_config_schema}.APPEND_{s}_ORDER_RETURNS_TASK
	schedule='1 MINUTE'
	USER_TASK_MANAGED_INITIAL_WAREHOUSE_SIZE='XSMALL'
	when SYSTEM$STREAM_HAS_DATA('{tpcxai_database_base}.CONFIG.{s}_ORDER_ORDERRETURNS_STREAM')
	as insert into {tpcxai_database_inc}.{s}.ORDER_RETURNS
select o_r.* 
from {tpcxai_database_base}.{s}.ORDER_RETURNS o_r,
     {tpcxai_database_base}.{tpcxai_config_schema}.{s}_ORDER_ORDERRETURNS_STREAM o
where o_r.OR_ORDER_ID = o.O_ORDER_ID
order by OR_ORDER_ID, OR_PRODUCT_ID ''', session)

    run_sql(f''' alter task {tpcxai_database_base}.{tpcxai_config_schema}.APPEND_{s}_ORDER_RETURNS_TASK resume ''', session)

    run_sql(f''' insert into {tpcxai_database_inc}.{s}.ORDERS
select * from {tpcxai_database_base}.{s}.ORDERS o
where o.ORDER_TS < current_timestamp() 
order by ORDER_TS, O_CUSTOMER_SK ''', session)

    run_sql(f''' create or replace task {tpcxai_database_base}.{tpcxai_config_schema}.APPEND_{s}_ORDER_TASK
	schedule='1 MINUTE'
	USER_TASK_MANAGED_INITIAL_WAREHOUSE_SIZE='XSMALL'
	as insert into {tpcxai_database_inc}.{s}.ORDERS
with o_max_timestamp as ( select max(ORDER_TS) max_ts
                           from {tpcxai_database_inc}.{s}.ORDERS )
     select O_ORDER_ID, O_CUSTOMER_SK, ORDER_TS, WEEKDAY, ORDER_DATE, STORE, TRIP_TYPE
       from {tpcxai_database_base}.{s}.ORDERS o,
            o_max_timestamp fmt
      where 
            o.ORDER_TS <= current_timestamp() 
        and o.ORDER_TS > fmt.max_ts
   order by ORDER_TS, O_CUSTOMER_SK ''', session)

    run_sql(f''' alter task {tpcxai_database_base}.{tpcxai_config_schema}.APPEND_{s}_ORDER_TASK resume ''', session)
```

```python
# Set up incremental database TRAINING schema that holds static, rather than incrementing database

run_sql(f''' create table TPCXAI_SF0001_QUICKSTART_INC.TRAINING.CUSTOMER      as select * from TPCXAI_SF0001_QUICKSTART.TRAINING.CUSTOMER order by C_CUSTOMER_SK ''', session)
run_sql(f''' create table TPCXAI_SF0001_QUICKSTART_INC.TRAINING.LINEITEM      as select * from TPCXAI_SF0001_QUICKSTART.TRAINING.LINEITEM order by LI_ORDER_ID ''', session)
run_sql(f''' create table TPCXAI_SF0001_QUICKSTART_INC.TRAINING.ORDERS        as select * from TPCXAI_SF0001_QUICKSTART.TRAINING.ORDERS order by ORDER_TS, O_ORDER_ID, O_CUSTOMER_SK ''', session)
run_sql(f''' create table TPCXAI_SF0001_QUICKSTART_INC.TRAINING.ORDER_RETURNS as select * from TPCXAI_SF0001_QUICKSTART.TRAINING.ORDER_RETURNS order by OR_ORDER_ID, OR_PRODUCT_ID ''', session)
```

## -------------------------------------------------------------------------------------

## CLEAN UP

```python
session.close()
```

