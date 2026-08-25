---
title: "Snowpark-optimized warehouses | Snowflake Documentation"
source: https://docs.snowflake.com/user-guide/warehouses-snowpark-optimized
cert_domain: domain-3-model-development
crawl_depth: 1
crawled: 2026-08-23
---

# Snowpark-optimized warehouses¶

Snowpark-optimized warehouses let you configure the available memory resources and CPU architecture on a single-node instance for your workloads.

## When to use a Snowpark-optimized warehouse¶

While [Snowpark](https://www.snowflake.com/en/data-cloud/snowpark/) workloads can be run on both standard and Snowpark-optimized warehouses, Snowpark-optimized warehouses are recommended for running Snowpark workloads such as code that has large memory requirements or dependencies on a specific CPU architecture. Example workloads include Machine Learning (ML) training use cases using a [stored procedure](/developer-guide/stored-procedure/stored-procedures-overview) on a single virtual warehouse node. Snowpark workloads, utilizing [UDF](/developer-guide/udf/udf-overview) or [UDTF](/developer-guide/udf/python/udf-python-tabular-functions), might also benefit from Snowpark-optimized warehouses. Workloads that don’t use Snowpark might not benefit from running on Snowpark-optimized warehouses.

Note

Initial creation and resumption of a Snowpark-optimized virtual warehouse might take longer than standard warehouses.

## Configuration options for Snowpark-optimized warehouses¶

[](/static/images/logo-snowflake-black.png) [Preview Feature](/release-notes/preview-features) — Open

The 1 TB resource constraints (MEMORY_64X and MEMORY_64X_x86) are available as a preview feature. The 1 TB constraints are available only on the Amazon Web Services (AWS) cloud platform.

All other MEMORY_* resource constraint sizes are generally available and are available for all cloud platforms.

The default configuration for a Snowpark-optimized warehouse provides 16x memory per node compared to a standard warehouse. You can optionally configure additional memory per node and specify CPU architecture using the `resource_constraint` property of the [CREATE WAREHOUSE](/sql-reference/sql/create-warehouse) or [ALTER WAREHOUSE](/sql-reference/sql/alter-warehouse) command. The following options are available:

Memory (up to)| CPU architecture| RESOURCE_CONSTRAINT values| Minimum warehouse size  
---|---|---|---  
16GB| Default or x86| MEMORY_1X, MEMORY_1X_x86| XSMALL  
256GB| Default or x86| MEMORY_16X, MEMORY_16X_x86| M  
1TB| Default or x86| MEMORY_64X, MEMORY_64X_x86| L  
  
## Creating a Snowpark-optimized warehouse¶

To create a new Snowpark-optimized warehouse, you can set the warehouse type property in the following interfaces.

Set the WAREHOUSE_TYPE property to `'SNOWPARK-OPTIMIZED'` when running the [CREATE WAREHOUSE](/sql-reference/sql/create-warehouse) command. For example:
[code]
    CREATE OR REPLACE WAREHOUSE snowpark_opt_wh WITH
      WAREHOUSE_SIZE = 'MEDIUM'
      WAREHOUSE_TYPE = 'SNOWPARK-OPTIMIZED';
    
[/code]

Create a large Snowpark-optimized warehouse `so_warehouse` with 256 GB of memory by specifying the resource constraint `MEMORY_16X_X86`:
[code]
    CREATE WAREHOUSE so_warehouse WITH
      WAREHOUSE_SIZE = 'LARGE'
      WAREHOUSE_TYPE = 'SNOWPARK-OPTIMIZED'
      RESOURCE_CONSTRAINT = 'MEMORY_16X_X86';
    
[/code]

Note

The default resource constraint is `MEMORY_16X`.

Set the `warehouse_type` property to `'SNOWPARK-OPTIMIZED'` when constructing a [Warehouse](/developer-guide/snowflake-python-api/reference/latest/_autosummary/snowflake.core.warehouse.Warehouse) object.

Then, pass this `Warehouse` object to the [WarehouseCollection.create](/developer-guide/snowflake-python-api/reference/latest/_autosummary/snowflake.core.warehouse.WarehouseCollection#snowflake.core.warehouse.WarehouseCollection.create) method to create the warehouse in Snowflake. For example:
[code]
    from snowflake.core import CreateMode
    from snowflake.core.warehouse import Warehouse
    
    my_wh = Warehouse(
      name="snowpark_opt_wh",
      warehouse_size="MEDIUM",
      warehouse_type="SNOWPARK-OPTIMIZED"
    )
    root.warehouses.create(my_wh, mode=CreateMode.or_replace)
    
[/code]

Note

Resource constraints are currently not supported in the Snowflake Python APIs.

## Modifying Snowpark-optimized warehouse properties¶

To modify warehouse properties including the warehouse type, you can use the following interfaces.

Note

You can change the warehouse type whether the warehouse is in the `STARTED` or `SUSPENDED` state. If you suspend a warehouse before changing the `warehouse_type` property, execute the following operation:
[code]
    ALTER WAREHOUSE snowpark_opt_wh SUSPEND;
    
[/code]
[code]
    root.warehouses["snowpark_opt_wh"].suspend()
    
[/code]

Use the [ALTER WAREHOUSE](/sql-reference/sql/alter-warehouse) command to modify the memory resources and CPU architecture for Snowpark-optimized warehouse `so_warehouse`:
[code]
    ALTER WAREHOUSE so_warehouse SET
      RESOURCE_CONSTRAINT = 'MEMORY_1X_x86';
    
[/code]

Resource constraints are currently not supported in the Snowflake Python APIs.

## Using Snowpark Python Stored Procedures to run ML training workloads¶

For information on Machine Learning Models and Snowpark Python, see [Training Machine Learning Models with Snowpark Python](/developer-guide/snowpark/python/python-snowpark-training-ml).

## Billing for Snowpark-optimized warehouses¶

For information on Snowpark-optimized warehouse credit consumption, see `Table 1` in the [Snowflake Service Consumption Table](https://www.snowflake.com/legal-files/CreditConsumptionTable.pdf).

Tip

For information about cost implications of changing the RESOURCE_CONSTRAINT property, see [considerations for changing RESOURCE_CONSTRAINT while a warehouse is running or suspended](/user-guide/warehouses-gen2#label-gen-2-standard-warehouses-altering).

## Region availability¶

Snowpark-optimized warehouses are available in all regions across AWS, Azure, and Google Cloud.

1 TB memory options are not currently available for the Microsoft Azure and Google Cloud Platform (GCP) [cloud platforms](/user-guide/intro-cloud-platforms). On the Amazon Web Services (AWS) cloud platform, the 1 TB memory option is also still a preview feature.
