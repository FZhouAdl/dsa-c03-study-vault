---
title: "Introduction to Snowflake Feature Store with Snowflake Notebooks"
source: https://quickstarts.snowflake.com/guide/intro-to-feature-store/
cert_domain: domain-2-data-preparation-and-feature-engineering
crawl_depth: 1
crawled: 2026-08-23
---

Skip to content

Snowflake World Tour hits your city

See how leading teams deploy agents at scale. Find a stop near you.

[Register free](/en/world-tour/)

[Snowflake for Developers](https://www.snowflake.com/content/snowflake-site/global/en/developers)/[Guides](https://www.snowflake.com/content/snowflake-site/global/en/developers/guides)/Introduction to Snowflake Feature Store with Snowflake Notebooks

Quickstart

## Introduction to Snowflake Feature Store with Snowflake Notebooks

Charlie Hammond

[Fork Repo](https://github.com/Snowflake-Labs/sfguide-intro-to-feature-store-using-snowflake-notebooks)

## Overview

The Snowflake Feature Store offers a comprehensive solution for data scientists and machine learning (ML) engineers to create, manage, and utilize ML features within their data science workflows in [Snowflake ML](/en/data-cloud/snowflake-ml/). Features, which are enriched or transformed data, serve as essential inputs for machine learning models. For instance, a feature might extract the day of the week from a timestamp to help the model identify weekly patterns, such as predicting that sales are typically 20% lower on Wednesdays. Other features often involve data aggregation or time-shifting. The process of defining these features, known as feature engineering, is crucial to developing high-quality ML applications.

This is part 1 of a 3-part introduction quickstart series to Snowflake Feature Store (part 2 [here](/en/developers/guides/overview-of-feature-store-api/) and part 3 [here](/en/developers/guides/develop-and-manage-ml-models-with-feature-store-and-model-registry/)). In this quickstart, you will learn how to build the key components of a feature store workflow, including entities, feature views, and datasets. Entities represent the real-world objects or concepts that your features describe, such as customers or products. Feature views provide a structured way to define and store these features, allowing for consistent and efficient retrieval. Finally, datasets are collections of features that are prepared for model training or inference. By the end of this quickstart, you'll have a solid understanding of how to create and manage these components within the Snowflake Feature Store, setting the foundation for building robust and scalable machine learning pipelines.

### Prerequisites

  * Access to a Snowflake account with Accountadmin.
  * Access to run Notebooks in Snowflake
  * Foundational knowledge of Data Science workflows



### What You Will Learn

  * The key features of Snowflake Feature Store including [entities](https://docs.snowflake.com/en/developer-guide/snowflake-ml/feature-store/entities), [feature views](https://docs.snowflake.com/en/developer-guide/snowflake-ml/feature-store/feature-views), and [datasets](https://docs.snowflake.com/en/developer-guide/snowflake-ml/feature-store/modeling#generating-datasets-for-training).



### What You’ll Need

  * A [Snowflake](https://app.snowflake.com/) Account



### What You’ll Build

  * An Snowflake Feature Store pipeline that generates a dataset for model training



## Setup Your Account

Complete the following steps to setup your account:

  * Navigate to Worksheets, click "+" in the top-right corner to create a new Worksheet, and choose "SQL Worksheet".
  * Paste and the following SQL in the worksheet
  * Run all commands to create Snowflake objects


[code] 
    USE ROLE ACCOUNTADMIN;
    USE DATABASE SNOWFLAKE;
    
    -- Using ACCOUNTADMIN, create a new role for this exercise and grant to applicable users
    CREATE OR REPLACE ROLE FEATURE_STORE_LAB_USER;
    BEGIN
        LET current_user_name := CURRENT_USER();
        EXECUTE IMMEDIATE 'GRANT ROLE FEATURE_STORE_LAB_USER TO USER ' || current_user_name;
    END;
    
    -- create our virtual warehouse
    CREATE OR REPLACE WAREHOUSE FEATURE_STORE_WH AUTO_SUSPEND = 60;
    
    GRANT ALL ON WAREHOUSE FEATURE_STORE_WH TO ROLE FEATURE_STORE_LAB_USER;
    
    -- use our feature_store_wh virtual warehouse 
    USE WAREHOUSE FEATURE_STORE_WH;
    
    -- Next create a new database and schema,
    CREATE OR REPLACE DATABASE FEATURE_STORE_DATABASE;
    CREATE OR REPLACE SCHEMA FEATURE_STORE_SCHEMA;
    
    GRANT OWNERSHIP ON DATABASE FEATURE_STORE_DATABASE TO ROLE FEATURE_STORE_LAB_USER COPY CURRENT GRANTS;
    GRANT OWNERSHIP ON ALL SCHEMAS IN DATABASE FEATURE_STORE_DATABASE  TO ROLE FEATURE_STORE_LAB_USER COPY CURRENT GRANTS;
    
    -- Setup is now complete
    
[/code]
[/code]

## Run the Notebook

  * Download the notebook from this [link](https://github.com/Snowflake-Labs/sfguide-intro-to-feature-store-using-snowflake-notebooks/blob/main/notebooks/0_start_here.ipynb)
  * Change role to FEATURE_STORE_LAB_USER
  * Navigate to Projects > Notebooks in Snowsight
  * Click Import .ipynb from the + Notebook dropdown
  * Create a new notebok with the following settings 

    * Notebook Location: FEATURE_STORE_DATABASE, FEATURE_STORE_SCHEMA
    * Run on Warehouse
    * Warehouse: FEATURE_STORE_WH

  * Create Notebook
  * Click Packages in the top right, add `snowflake-ml-python`
  * Run cells in the notebook!



## Conclusion And Resources

The Snowflake Feature Store provides a powerful, all-in-one solution for data scientists and ML engineers to create, manage, and utilize machine learning features effectively. In this quickstart, you’ve learned the essentials of building a feature store workflow, including entities, feature views, and datasets. Now, take the next step and apply these concepts to build robust, scalable ML pipelines within Snowflake. Check out the links below and start building you Feature Stores today!

### What You Learned

  * **Understanding the Snowflake Feature Store** : Gained insight into how the Snowflake Feature Store helps manage and utilize ML features in data science workflows.
  * **Building Key Components** : 

    * **Entities** : Defined real-world objects or concepts that features describe, such as customers or products.
    * **Feature Views** : Learned how to structure and store features for consistent and efficient retrieval.
    * **Datasets** : Prepared collections of features for model training or inference.

  * **Workflow Integration** : Developed an understanding of how these components work together to create a robust and scalable ML pipeline.



### Related Quickstarts

  * Part 2: [Getting Started with Snowflake Feature Store API](/en/developers/guides/overview-of-feature-store-api/)
  * Part 3: [Develop and Manage ML Models with Feature Store and Model Registry](/en/developers/guides/develop-and-manage-ml-models-with-feature-store-and-model-registry/)



### Related Resources

  * [Snowflake Feature Store](https://docs.snowflake.com/en/developer-guide/snowflake-ml/feature-store/overview)
  * [Entities](https://docs.snowflake.com/en/developer-guide/snowflake-ml/feature-store/entities)
  * [Feature Views](https://docs.snowflake.com/en/developer-guide/snowflake-ml/feature-store/feature-views)
  * [Datasets](https://docs.snowflake.com/en/developer-guide/snowflake-ml/feature-store/modeling#generating-datasets-for-training).
  * [Snowflake ML Webpage](/en/data-cloud/snowflake-ml/)



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
