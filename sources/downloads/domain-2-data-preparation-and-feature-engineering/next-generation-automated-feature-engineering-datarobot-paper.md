---
title: "Next-Generation Automated Feature Engineering Datarobot Paper"
source: https://www.snowflake.com/wp-content/uploads/2021/03/Feature-Discovery-with-Snowflake.pdf
cert_domain: domain-2-data-preparation-and-feature-engineering
crawl_depth: 0
crawled: 2026-08-23
note: converted from PDF
---

DATA SHEE T



Next-Generation Automated
Feature Engineering
with DataRobot and Snowflake

Adopting AI across an organization has significant hurdles. AI is complex, takes time
                                                                                                                “We used the DataRobot
to create, and releasing poor quality models into production can pose an enormous risk
to the business. DataRobot’s Feature Discovery streamlines this process by offering                             Feature Discovery tool
automated feature engineering that enables the creation of valuable new features                                on our high-frequency
for your machine learning models. This capability is integrated with the Snowflake
Data Cloud making the process both faster and more cost-effective.
                                                                                                                physiological data, which
Snowflake and DataRobot have invested time and resources into advancing                                         helped us find some
the capabilities and benefits of one of the most critical tasks in AI - feature engineering.                    new features that ended
                                                                                                                up being high impact
The Best Features from Your Snowflake Data
                                                                                                                throughout all of our
Feature engineering is one of the most critical tasks in AI. The features you create often determine
the success or failure of your machine learning projects. The issue is that raw data rarely has all the right   subsequent analysis.
features your models need, and if you’re using multiple data sources you need to consolidate your data
                                                                                                                         Dr. Austin Chou, PhD
into a single table to train models and make predictions.
                                                                                                                        Lead Data Scientist, UCSF
This usually involves joining multiple tables and exploring many aggregations (i.e., sum, max, avg,
count, entropy, etc.) on different derivation windows (e.g., last 30 days, last week, etc.).
                                                                                                                    Brain and Spinal Injury Center
                                                                                                                 Zuckerberg San Francisco General
                                                                                                                      Hospital and Trauma Center




(e.g., Sum of order amounts, among last 30 days orders, by customer_id)




WHEN PERFORMED MANUALLY,
FEATURE ENGINEERING IS:



Time-consuming:                                                Error prone:
the more data sources you use,                                 An error such as
the more time you need                                         a missing filter can have
to explore transformations                                     a huge impact on the results
and evaluate the results.                                      and introduce target leakage.




                                                                                                                                              1
DATA SHEE T |           Feature Discovery with Snowflake


DataRobot’s Automated Feature Discovery simplifies and accelerates this feature
engineering process through the automation of expert data science best practices.



DataRobot’s Feature Discovery: Next-Generation
Automated Feature Engineering
Compared to usual automated feature engineering solutions, Feature Discovery can leverage data
from multiple datasets, not just one, and automatically discovers, tests, and creates hundreds
of valuable new features for your machine learning models, dramatically improving their accuracy.




Now Faster with Snowflake
Exploring multiple data sources has always required transferring large amounts
of data between systems which was resource-intensive and time consuming.

DataRobot’s new Snowflake integration pushes Feature Discovery operations into Snowflake
to minimize data movement, resulting in faster results and lower operating costs.




In many ways, Feature Discovery is an extension of DataRobot’s AutoML (Automated Machine Learning)
product in terms of the automation that it brings to the data science process. This integration now
allows users to get even more accurate models from their Snowflake Data Cloud.



DataRobot’s Feature Discovery
Automated feature engineering taken to a new level.




                                                                                           v02162021.1039   2
DATA SHEE T |          Feature Discovery with Snowflake




            VISUAL AND INTUITIVE                                                                             About DataRobot
  Feature Discovery unlocks the art of advanced feature engineering for data scientists,                     and Snowflake
  data engineers, and business analysts. Using DataRobot’s visual relationship editor,
  you can select all of the datasets you want to use in your project, then quickly declare
                                                                                                             Partnership
  relationships between the datasets with just a few clicks. DataRobot even suggests joins
  for you if you don’t know the relationships in advance. Feature Discovery makes it incredibly
  easy for anyone to define very complex data schemas upon which to perform automated
  feature engineering in minutes.




                                                                                                             Snowflake and DataRobot together
                                                                                                             offer an end-to-end AI experience
                                                                                                             that accelerates time to value by
                                                                                                             reducing complexity and removing
                                                                                                             the delay between data and AI
                                                                                                             insights.

                                                                                                             Specifically, Snowflake’s Data Cloud
                                                                                                             breaks down data silos, allowing
                                                                                                             users to work with any and all of
                                                                                                             their data, without limits on scale,
                                                                                                             performance or flexibility, and
                                                                                                             grants instant access to third-
            BUILT-IN AWARENESS OF TIME
                                                                                                             party data via the Snowflake Data
  DataRobot Feature Discovery is fully time-aware. If your datasets are temporal in nature,                  Marketplace. DataRobot takes
  you can set derivation windows to control how much history should be used when calculating                 advantage of this seamless access
  new features. For example, you can tell DataRobot to consider only 30 days of order history                to organized and documented data
  when predicting whether or not a customer will make a purchase when he visits a store.                     to massively accelerate the model
  Feature Discovery also has built-in guardrails that avoid common leakage problems, such                    development lifecycle, enabling the
  as ensuring that future data is excluded when generating new features.                                     creation of trusted and scalable
                                                                                                             models across the business,
                                                                                                             ultimately driving a significant
                                                                                                             competitive advantage.

                                                                                                             The combination of Snowflake and
                                                                                                             DataRobot accelerates the journey
                                                                                                             to become AI-driven.




                                                                                                             Contact Us
            PRACTICAL, EXPLAINABLE, AND TRACEABLE
                                                                                                             DataRobot
  Like every automated capability in the DataRobot AI platform, Feature Discovery is incredibly
                                                                                                             225 Franklin Street, 13th Floor
  transparent. You can visualize and explore every feature generated to understand predictive
                                                                                                             Boston, MA 02110, USA
  potential. Full lineage is also available for every feature and is created for traceability and
  auditing purposes. You can access detailed logs to understand exactly which features were                  www.datarobot.com
  generated, explored, tested, and discarded. You can also download the full training dataset,               info@datarobot.com
  with all the new derived features, for further analysis and use in other applications.




                                                                                                             © 2021 DataRobot, Inc. All rights reserved.
                                                                                                             DataRobot and the DataRobot logo are
                                                                                                             trademarks of DataRobot, Inc. All other marks
                                                                                                             are trademarks or registered trademarks of
                                                                                                             their respective holders.




                                                                                            v02162021.1039                                               3
