---
title: "Develop and Manage ML Models with Feature Store and Model Registry"
source: https://quickstarts.snowflake.com/guide/develop-and-manage-ml-models-with-feature-store-and-model-registry/
cert_domain: domain-2-data-preparation-and-feature-engineering
crawl_depth: 1
crawled: 2026-08-23
---

Skip to content

Snowflake World Tour hits your city

See how leading teams deploy agents at scale. Find a stop near you.

[Register free](/en/world-tour/)

[Snowflake for Developers](https://www.snowflake.com/content/snowflake-site/global/en/developers)/[Guides](https://www.snowflake.com/content/snowflake-site/global/en/developers/guides)/Develop and Manage ML Models with Feature Store and Model Registry

Certified Solution

## Develop and Manage ML Models with Feature Store and Model Registry

ML functions

Charlie Hammond

[Fork Repo](https://github.com/Snowflake-Labs/sfquickstarts/tree/master/site/sfguides/src/develop-and-manage-ml-models-with-feature-store-and-model-registry)

## Overview

[Snowflake ML](/en/data-cloud/snowflake-ml/) is an integrated set of capabilities for end-to-end machine learning in a single platform on top of your governed data. Data scientists and ML engineers can easily and securely develop and productionize scalable features and models without any data movement, silos, or governance tradeoffs. The Snowpark ML Python library (the snowflake-ml-python package) provides APIs for developing and deploying your Snowflake ML pipelines.

This is part 3 of a 3-part introduction quickstart series to Snowflake Feature Store (check out part 1 [here](/en/developers/guides/intro-to-feature-store/) and part 2 [here](/en/developers/guides/overview-of-feature-store-api/)). This quickstart demonstrates an end-to-end ML experiment cycle including feature creation, training data generation, model training and inference. The workflow touches on key Snowflake ML features including [Snowflake Feature Store](https://docs.snowflake.com/en/developer-guide/snowpark-ml/feature-store/overview), [Dataset](https://docs.snowflake.com/en/developer-guide/snowpark-ml/dataset), [Snowflake ML APIs](https://docs.snowflake.com/en/developer-guide/snowpark-ml/modeling), and [Snowflake Model Registry](https://docs.snowflake.com/en/developer-guide/snowpark-ml/model-registry/overview).

### What You Will Learn

  * The key features of Snowflake Feature Store including [entities](https://docs.snowflake.com/en/developer-guide/snowflake-ml/feature-store/entities) and [feature views](https://docs.snowflake.com/en/developer-guide/snowflake-ml/feature-store/feature-views).
  * How to train a model using [Snowflake ML APIs](https://docs.snowflake.com/en/developer-guide/snowpark-ml/modeling)
  * How to log and reference models using [Snowflake Model Registry](https://docs.snowflake.com/en/developer-guide/snowpark-ml/model-registry/overview)



### What You’ll Need

  * A [Snowflake](https://app.snowflake.com/) Account



### What You’ll Build

  * An ML model using Snowflake Feature Store and Model Registry



## Setup Your Account

Complete the following steps to setup your account:

  * Navigate to Worksheets, click "+" in the top-right corner to create a new Worksheet, and choose "SQL Worksheet".
  * Paste and the following SQL in the worksheet
  * Adjust <YOUR_USER> to your user
  * Run all commands to create Snowflake objects


[code] 
    USE ROLE ACCOUNTADMIN;
    
    -- Using ACCOUNTADMIN, create a new role for this exercise and grant to applicable users
    CREATE OR REPLACE ROLE ML_MODEL_ROLE;
    GRANT ROLE ML_MODEL_ROLE to USER <YOUR_USER>;
    
    -- create our virtual warehouse
    CREATE OR REPLACE WAREHOUSE ML_MODEL_WH AUTO_SUSPEND = 60;
    
    GRANT ALL ON WAREHOUSE ML_MODEL_WH TO ROLE ML_MODEL_ROLE;
    
    -- Next create a new database and schema,
    CREATE OR REPLACE DATABASE ML_MODEL_DATABASE;
    CREATE OR REPLACE SCHEMA ML_MODEL_SCHEMA;
    
    GRANT OWNERSHIP ON DATABASE ML_MODEL_DATABASE TO ROLE ML_MODEL_ROLE COPY CURRENT GRANTS;
    GRANT OWNERSHIP ON ALL SCHEMAS IN DATABASE ML_MODEL_DATABASE TO ROLE ML_MODEL_ROLE COPY CURRENT GRANTS;
    
[/code]
[/code]

## Run the Notebook

  * Download the notebook from this [link](https://github.com/Snowflake-Labs/sfguide-develop-and-manage-ml-models-with-feature-store-and-model-registry/blob/main/notebooks/0_start_here.ipynb)
  * Download [feature-store-ui.png](https://github.com/Snowflake-Labs/sfguide-develop-and-manage-ml-models-with-feature-store-and-model-registry/blob/main/notebooks/feature-store-ui.png) and [model-registry-ui.png](https://github.com/Snowflake-Labs/sfguide-develop-and-manage-ml-models-with-feature-store-and-model-registry/blob/main/notebooks/model-registry-ui.png)
  * Change role to ML_MODEL_ROLE
  * Navigate to Projects > Notebooks in Snowsight
  * Click Import .ipynb from the + Notebook dropdown
  * Create a new notebok with the following settings 

    * Notebook Location: ML_MODEL_DATABASE, ML_MODEL_SCHEMA
    * Run on Warehouse
    * Warehouse: ML_MODEL_WH

  * Create Notebook
  * Click Packages in the top right, add `snowflake-ml-python` and `snowflake-snowpark-python`
  * Upload both image files by clicking the plus button on the file explorer in the left pane
  * Run cells in the notebook!



## Conclusion And Resources

Snowflake ML offers a comprehensive, integrated platform for end-to-end machine learning, allowing data scientists and ML engineers to develop and deploy scalable models seamlessly, all within a governed data environment. With the Snowpark ML Python library, you can build and manage your ML pipelines without the need for data movement or compromising on governance.

This Quickstart has demonstrated how to execute an entire ML experiment cycle—from feature creation to model training and inference—while highlighting key features such as the Snowflake Feature Store, Dataset, Snowpark ML Modeling, and the Snowflake Model Registry.

Ready to elevate your machine learning projects? Dive into the full potential of Snowflake ML and start transforming your data into actionable insights today. Check out the links below to get started and explore more advanced capabilities.

### What You Learned

  * The key features of Snowflake Feature Store including [entities](https://docs.snowflake.com/en/developer-guide/snowflake-ml/feature-store/entities) and [feature views](https://docs.snowflake.com/en/developer-guide/snowflake-ml/feature-store/feature-views).
  * How to train a model using [Snowflake ML APIs](https://docs.snowflake.com/en/developer-guide/snowpark-ml/modeling)
  * How to log and reference models using [Snowflake Model Registry](https://docs.snowflake.com/en/developer-guide/snowpark-ml/model-registry/overview)



### Related Quickstarts

  * Part 1: [Introduction to Snowflake Feature Store with Snowflake Notebooks](/en/developers/guides/intro-to-feature-store/)
  * Part 2: [Getting Started with Snowflake Feature Store API](/en/developers/guides/overview-of-feature-store-api/)



### Related Resources

  * [Snowflake Feature Store](https://docs.snowflake.com/en/developer-guide/snowflake-ml/feature-store/overview)

  * [Entities](https://docs.snowflake.com/en/developer-guide/snowflake-ml/feature-store/entities)

  * [Feature Views](https://docs.snowflake.com/en/developer-guide/snowflake-ml/feature-store/feature-views)

  * [Datasets](https://docs.snowflake.com/en/developer-guide/snowflake-ml/feature-store/modeling#generating-datasets-for-training)

  * [Snowflake ML Webpage](/en/data-cloud/snowflake-ml/)

  * [Fork Repo on GitHub](https://github.com/Snowflake-Labs/sfguide-getting-started-with-snowflake-notebook-container-runtime/blob/main/notebooks/0_start_here.ipynb?_fsi=EwgOAmF4&_fsi=EwgOAmF4)

  * [Download Reference Architecture](https://drive.google.com/file/d/1GA_pt6Pdy76tWkxyPFKL2xH_YG3v5ZRM/view?usp=sharing)

  * [Read Engineering Blog](/en/engineering-blog/machine-learning-container-runtime/)

  * [Watch the Demo](https://youtu.be/5zXP6Kj5gM4?list=TLGGiXdaWh2xmL4yMjA5MjAyNQ)




Updated Dec 20, 2025

This content is provided as is, and is not maintained on an ongoing basis. It may be out of date with current Snowflake instances

##### On this page

Overview

Setup Your Account

Run the Notebook

Conclusion And Resources

**Subscribe to our monthly newsletter**

Stay up to date on Snowflake’s latest products, expert insights and resources—right in your inbox!

Product

  * [Platform](https://www.snowflake.com/en/product/platform/)
  * [Snowflake CoWork](/en/product/snowflake-cowork/)
  * [Data Engineering](https://www.snowflake.com/en/product/data-engineering/)
  * [Analytics](https://www.snowflake.com/en/product/analytics/)
  * [AI](https://www.snowflake.com/en/product/ai/)
  * [Applications & Collaboration](https://www.snowflake.com/en/product/applications-and-collaboration/)
  * [Pricing](https://www.snowflake.com/en/pricing-options/)



Support

  * [Support](https://www.snowflake.com/en/support/)
  * [Priority Support](https://www.snowflake.com/en/legal/addenda/priority-support-services-description/)
  * [Status](https://status.snowflake.com/)



[Industries](/en/solutions/industries/)

  * [Advertising, Media & Entertainment](/en/solutions/industries/advertising-media-entertainment/)
  * [Financial Services](/en/solutions/industries/financial-services/)
  * [Healthcare & Life Sciences](/en/solutions/industries/healthcare-and-life-sciences/)
  * [Manufacturing](/en/solutions/industries/manufacturing/)
  * [Public Sector](/en/solutions/industries/public-sector/)
  * [Retail & Consumer Goods](/en/solutions/industries/retail-consumer-goods/)
  * [Telecom](/en/solutions/industries/telecom/)
  * [Technology](https://www.snowflake.com/en/solutions/industries/technology/)



Company

  * [About Snowflake](https://www.snowflake.com/en/company/overview/about-snowflake/)
  * [Leadership & Board](https://www.snowflake.com/en/company/overview/leadership-and-board/)
  * [Careers](https://careers.snowflake.com/us/en)
  * [Investor Relations](https://investors.snowflake.com/overview/default.aspx)
  * [Trust Center](https://trust.snowflake.com/)
  * [Brand Guidelines](https://www.snowflake.com/brand-guidelines/)
  * [Contact](https://www.snowflake.com/en/contact/)
  * [Newsroom](https://www.snowflake.com/en/news/)
  * [Environmental, Social & Governance](https://www.snowflake.com/en/company/overview/esg/)
  * [Snowflake Ventures](https://www.snowflake.com/en/company/overview/snowflake-ventures/)
  * [End Data Disparity](https://www.snowflake.com/en/company/overview/end-data-disparity/)
  * [Snowflake Summit 26](/en/summit/)



Learn

  * [Resource Library](https://snowflake.com/en/resources/)
  * [Live Demos](/en/webinars/demo/)
  * [Fundamentals](https://www.snowflake.com/en/fundamentals/)
  * [Training](https://www.snowflake.com/en/resources/learn/training/)
  * [Certifications](https://www.snowflake.com/en/resources/learn/certifications/)
  * [Snowflake University](https://learn.snowflake.com/en/)
  * [Developer Guides](https://www.snowflake.com/en/developers/guides)
  * [Documentation](https://docs.snowflake.com/)
  * [Data Governance](/en/data-governance/)



[](/en/)

  * © 2026 Snowflake Inc. All Rights Reserved
  * [Privacy Policy](https://www.snowflake.com/en/legal/privacy/privacy-policy/)
  * [Site Terms](https://snowflake.com/en/legal/snowflake-site-terms/)
  * [Communication Preferences](https://info.snowflake.com/Preference-center.html)
  *   * [Do Not Share My Personal Information](https://www.snowflake.com/en/legal/privacy/privacy-policy/#12)
  * [Legal](https://www.snowflake.com/en/legal/)



[](https://x.com/Snowflake "X \(Twitter\)")

[](https://www.linkedin.com/company/3653845 "LinkedIn")

[](https://www.facebook.com/snowflakedb/ "Facebook")

[](https://www.youtube.com/user/snowflakecomputing "YouTube")
