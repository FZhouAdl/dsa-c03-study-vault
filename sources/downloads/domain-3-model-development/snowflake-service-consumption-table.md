---
title: "Snowflake Service Consumption Table"
source: https://www.snowflake.com/legal-files/CreditConsumptionTable.pdf
cert_domain: domain-3-model-development
crawl_depth: 0
crawled: 2026-08-23
note: converted from PDF
---

                                                          Snowflake Service Consumption Table
Effective: August 18, 2026
Consumption
Generally. The Snowflake Service is a cloud data platform provided by Snowflake (“Snowflake”, “we”, “us”, “our”) to Snowflake customers (each a
“Customer”, “you”, “your”) as a service which consumes resources for distinct functions as set forth herein and is available in several different editions
(“Editions”) and hosted in different geographical regions (“Regions”) by certain third-party cloud providers (“Cloud Providers”), as described more fully in
your agreement with Snowflake (“Agreement”) and the Documentation. Customer Data is uploaded into Customer’s account on the Snowflake Service
(“Account”) and may be stored in a central repository in each Account (“Storage”). Customer can create and manage virtual warehouses (each, a “Virtual
Warehouse”) or Snowpark container services compute nodes (“Compute Nodes”), or enable Snowflake to determine the optimal compute configuration
(“Adaptive Warehouse”), as needed to combine and process Customer Data from Storage (respectively, “Virtual Warehouse Services,” “SPCS
Compute,” and “Adaptive Compute Services”). As set forth in the tables below “Azure,” “AWS,” and “GCP” each refer to the Cloud Providers, Microsoft
Corporation, Amazon Web Services, Inc., and Google LLC, respectively. Any ($) values set forth in the below tables are in U.S. dollars (USD).
Platform Credits and AI Credits

           Compute (Virtual Warehouse Services, SPCS Compute, Snowflake Openflow, and Postgres Compute). Snowflake bills for compute using
           purchasable credits, as described herein (“Platform Credits”).1 Virtual Warehouses and Compute Nodes (which includes Postgres Compute)
           use Platform Credits at certain rates based on the effective size2 of the Virtual Warehouse or Compute Node while the applicable Virtual
           Warehouse or Compute Node is in operation, as set forth in the Platform Credit Tables below, as determined by Customer. When a Virtual
           Warehouse or Compute Node is suspended, it does not use any Platform Credits. Unless otherwise noted, when a Virtual Warehouse or a
           Postgres Compute Node is started or resumed, a minimum of one minute’s worth of Platform Credits will be consumed; when an Interactive
           Warehouse (a type of Virtual Warehouse) is started or resumed, a minimum of sixty (60) minutes’ worth of Platform Credits will be consumed;
           when a Compute Node (except for Postgres Compute) is started or resumed, a minimum of five minutes’ worth of Platform Credits will be
           consumed. Thereafter, Virtual Warehouses and Compute Nodes will be charged on a per second basis, rounded up to the nearest whole second.
           For Snowflake Openflow Compute, Platform Credits will be charged on a per second basis with a 60-second minimum.

           Compute (Adaptive Compute Services). For Adaptive Compute Services, Snowflake will configure an Adaptive Warehouse based on the
           customer-provided settings, as described in the Documentation. Unless otherwise noted, Platform Credits will be consumed when queries run
           on Adaptive Warehouse.
           Cloud Services.3 Operating the Snowflake Service requires the use of the Snowflake cloud services layer (“Cloud Services”). Cloud Services is
           charged based on the amount of Cloud Services compute resources used while operating the Snowflake Service. You will be charged 4.4 Credits
           per hour of Cloud Services use. Your daily use of Cloud Services will not incur any charges if the daily Platform Credits charged for such use is
           less than or equal to 10% of the daily Platform Credits consumed for use of Virtual Warehouse Services (“Cloud Services Adjustment”). Cloud
           Services usage associated with: (i) Serverless Features (as described below); or (ii) SPCS Compute do not contribute to the Cloud Services
           Adjustment.
           Serverless Features. Snowflake offers a number of Serverless Features as described in the Documentation. Serverless Features operate on
           Snowflake-managed compute resources which are charged by the number of Compute-Hours used to operate the Serverless Feature. One
           “Compute-Hour” is defined as the computing resources comparable to running an XS Virtual Warehouse for one hour. In the event Snowflake
           uses computing resources that are equivalently larger or smaller than an XS Virtual Warehouse, the Platform Credit consumption rate will be
           adjusted accordingly. For example, if a Serverless Feature uses computing resources comparable to an S Virtual Warehouse for 30 minutes,
           such use is measured as one Compute-Hour. Compute-Hours are calculated on a per second basis, rounded up to the nearest whole second.
           For Serverless Features, Snowflake-managed compute is billed at the rate of one Platform Credit per Compute-Hour, multiplied by the applicable
           feature multiplier in Table 5. Some Serverless Features also incur Cloud Service charges, which are also multiplied by the applicable feature
           multiplier in Table 5. Some Serverless Features also incur Unit Charges. The Serverless Feature charges for Snowflake-managed compute
           resources, Cloud Services, and Unit Charges appear in the usage statement as a single line item for that Serverless Feature.
           AI Features. Snowflake additionally offers artificial intelligence features that run on Snowflake-managed compute resources (“Snowflake AI
           Features”). Snowflake AI Features bill at the rates set forth in Table 6. “Token” refers to each smallest unit of text processed by the model
           powering the Snowflake AI Feature. The conversion rate of raw input or output text into Tokens can vary depending on the Cortex model, as
           further described in the Documentation. Where a feature bills on Tokens, the charge is based on the total number of Tokens processed, which
           may include only input Tokens or both input and output Tokens, as described in the Documentation. In some cases, Snowflake AI Features bill
           based on the amount of compute time spent to complete a job, or on the units processed. Snowflake AI Features may bill based on purchasable
           units, as described in Table 2 (“AI Credits”). When a price is listed per-quantity, such as AI Credits per 1 million tokens or per 1,000 messages,
           you will be billed proportionally for all units used, not only in whole-quantity increments. The amount of compute time billed may be normalized
           based on the types of compute used.
           Data Sharing Features and Rebate Program. As described in the Documentation, Snowflake offers you the ability to use Data Sharing Features
           to (1) share the data in your account with other Snowflake customers and/or users, including via read-only accounts (“Reader Accounts”) and
           (2) access or use data from other Snowflake customers (provided your usage data may be provided to such Snowflake customers). You will pay
           for all usage by Reader Accounts. Your Platform Credit usage may be offset by Platform Credits issued under the Snowflake Collaboration
           Rebate Program (“Rebate Program”). Please read the applicable Collaboration Rebate Terms made available at
           https://www.snowflake.com/en/legal/ (the “Rebate Terms”) that describe the Rebate Program. The Rebate Terms may have changed since you
           last read them. By accepting any benefits under the Rebate Program, including any Platform Credits you earn through the Rebate Program, you
           accept the Rebate Terms. If you do not agree to be bound by the Rebate Terms, you may not participate in the Rebate Program and must opt
           out of the Rebate Program by contacting Snowflake at legalnotices@snowflake.com.
Storage. Storage pricing is based on the average terabytes per month of all Customer Data stored in your Snowflake Account. The average terabytes per
month is calculated by taking periodic snapshots of all Customer Data. The daily average will be displayed in the Snowflake Service. The monthly charge
is based on the average calculated across the number of calendar days in that month. If Customer Data stored is compressed, the compressed file size is

1 For clarity, where Customer’s Order Form, Agreement or Documentation refers to the term “Snowflake Credits” or “Credits”, such reference shall be

interpreted to refer to Platform Credits.
2 If you have the ability to create multi-cluster Virtual Warehouses, each Virtual Warehouse can run multiple, equal-size clusters concurrently and

independently. The rate of Platform Credits charged for a multi-cluster Virtual Warehouse is calculated by multiplying the rate for the size of the Virtual
Warehouse and the number of running clusters.
3 Accounts in Capacity will be charged for Cloud Services only if it has a Subscription Term Start Date on or after February 1, 2020.



                                                                                                                                                              1
used in the calculation of the total storage used. The applicable storage pricing for each feature is set forth in Tables 3(a), 3(b), 3(c), 3(d), 3(e), and 3(f)
below, as described further in the Documentation.
Data Transfer (Ingress or Egress): As more fully described in the Documentation, the Snowflake Service allows Customer to transfer Customer Data in
and out of its existing Account (“Data Transfer”). Depending on the Cloud Provider and what Region the Data Transfer is executed from, charges may be
incurred for the Data Transfer. In addition, SPCS Compute can incur Data Transfer costs when transferring data between compute components (“SPCS
Data Transfer”). On a daily basis, any charges for SPCS Data Transfer will be adjusted down by an amount equal to the lower of: (i) your cost incurred
that day for SPCS Data Transfers; or (ii) 10% of your cost incurred that day for SPCS Compute. The charges applicable to a Data Transfer for each Cloud
Provider are set forth in Tables 4(a), 4(b), and 4(c) below. As described in the Documentation, some features (e.g., External Functions) may have
alternate data transfer charges, as set forth below.


 Table 1(a): Table for Virtual Warehouse Services, Standard Warehouse

                                                                 Platform Credits Per Hour
      XS               S                  M              L           XL            2XL                 3XL              4XL             5XL4            6XL4
       1               2                  4              8             16               32                64            128             256             512


 Table 1(b): Table for Virtual Warehouse Services, Gen 2 Warehouse

    Cloud Provider                                                             Platform Credits Per Hour
                                  XS              S               M                 L             XL                  2XL               3XL             4XL
           AWS                   1.35             2.7            5.4             10.8               21.6              43.2             86.4            172.8
           Azure                 1.25             2.5             5               10                 20               40                80              160
           GCP                   1.35             2.7            5.4             10.8               21.6              43.2             86.4            172.8


 Table 1(c): Table for Virtual Warehouse Services, Snowpark Optimized Warehouses

                                                                 Platform Credits Per Hour
   Resource Constraints                   XS       S          M           L          XL               2XL           3XL        4XL             5XL        6XL
 MEMORY_1X                               1.00     2.00       4.00        8.00      16.00             32.00         64.00      128.00            -          -
 MEMORY_1X_x86                           1.10     2.20       4.40        8.80      17.60             35.20         70.40      140.80            -          -
 MEMORY_16X                                -        -        6.00       12.00      24.00             48.00         96.00      192.00          384.00     768.00
 MEMORY_16X_x86                            -        -        6.25       12.50      25.00             50.00        100.00      200.00            -          -
 MEMORY_64X5                               -        -          -        15.00      30.00             60.00        120.00      240.00            -          -
 MEMORY_64X_x865                           -        -          -        16.00      32.00             64.00        128.00      256.00            -          -



 Table 1(d): Table for Virtual Warehouse Services, Interactive Warehouse

                                                                 Platform Credits Per Hour
        XS                   S                    M                 L                XL                        2XL               3XL                   4XL
        0.6                 1.2                   2.4                 4.8                9.6                   19.2              38.4                  76.8


 Table 1(e): Table for Adaptive Compute Services5

                                        Feature                                                               Platform Credits
 Adaptive Warehouse                                                              Platform Credit consumption scales based on factors like compute usage,
                                                                                 software optimizations, and active queries. It is managed by the customer-
                                                                                 provided settings for the Adaptive Warehouse, as described in the
                                                                                 Documentation.



 Table 1(f): Table for First Generation SPCS Instances

                                                                                               Platform Credits Per Hour
                   Instance Family                              SPCS Compute                       Openflow Compute – Snowflake Deployment (SPCS)
 CPU_X64_XS                                                             0.06                                                    -
 CPU_X64_S                                                              0.11                                                  0.11
 CPU_X64_M                                                              0.22                                                    -
 CPU_X64_SL                                                             0.41                                                  0.41
 CPU_X64_L                                                              0.83                                                  0.83
 HIGHMEM_X64_S                                                          0.28                                                    -
 HIGHMEM_X64_M                                                          1.11                                                    -
 HIGHMEM_X64_SL                                                         2.93                                                    -
 HIGHMEM_X64_L                                                          4.44                                                    -
 GPU_NV_XS                                                              0.25                                                    -


4 5XL and 6XL standard Virtual Warehouse sizes, if available, may not be GA in all Regions.


5 This feature is available as a Preview, subject to the Preview Terms at https://www.snowflake.com/en/legal/.



                                                                                                                                                                  2
Table 1(f): Table for First Generation SPCS Instances

                                                                              Platform Credits Per Hour
              Instance Family                             SPCS Compute             Openflow Compute – Snowflake Deployment (SPCS)
GPU_GCP_NV_L4_1_24G                                            0.43                                          -
GPU_NV_S                                                       0.57                                          -
GPU_NV_SM                                                      1.70                                          -
GPU_GCP_NV_L4_4_24G                                            1.94                                          -
GPU_NV_M                                                       2.68                                          -
GPU_NV_2M                                                      3.50                                          -
GPU_NV_3M                                                      3.55                                          -
GPU_GCP_NV_A100_8_40G                                         11.68                                          -
GPU_NV_SL                                                     13.50                                          -
GPU_NV_L                                                      14.12                                          -


Table 1(g): Table for SPCS Compute
                                                                         Platform Credits Per Hour
        Instance Family
                                                 AWS                              Azure                                GCP
GEN_ARM_G1_2                                      0.057                            N/A                                 N/A
GEN_ARM_G1_4                                      0.114                            N/A                                 N/A
GEN_ARM_G1_8                                      0.228                            N/A                                 N/A
GEN_ARM_G1_16                                     0.456                            N/A                                 N/A
GEN_ARM_G1_32                                     0.912                            N/A                                 N/A
GEN_X64_G2_2                                      0.064                           0.064                                N/A
GEN_X64_G2_4                                      0.128                           0.128                                N/A
GEN_X64_G2_8                                      0.256                           0.256                                N/A
GEN_X64_G2_16                                      N/A                            0.512                                N/A
GEN_X64_G2_32                                     1.024                           1.024                                N/A
GPU_L40S_G1_8                                     1.424                            N/A                                 N/A
GPU_L40S_G1_16                                    1.908                            N/A                                 N/A
GPU_L40S_G1_48                                    6.663                            N/A                                 N/A
GPU_L40S_G1_192                                  19.133                            N/A                                 N/A
GPU_R6K_G1_8                                      2.136                            N/A                                 N/A
GPU_R6K_G1_16                                     2.539                            N/A                                 N/A
GPU_R6K_G1_32                                     3.345                            N/A                                 N/A
GPU_R6K_G1_48                                     5.262                            N/A                                2.857
GPU_R6K_G1_96                                    10.523                            N/A                                5.715
GPU_R6K_G1_192                                   21.047                            N/A                                11.430
GPU_R6K_G1_384                                     N/A                             N/A                                22.860
GPU_A100_G1_12                                     N/A                             N/A                                2.333
GPU_A100_G1_48                                     N/A                             N/A                                9.330
MEM_X64_G2_8                                      0.336                           0.287                                N/A
MEM_X64_G2_32                                     1.344                           1.148                                N/A
MEM_X64_G2_64                                     2.688                           2.296                                N/A
MEM_X64_G2_96                                      N/A                            3.444                                N/A
MEM_X64_G2_192                                    8.065                            N/A                                 N/A
GPU_L4_G1_4                                       0.511                            N/A                                 N/A
GPU_L4_G1_16                                      0.840                            N/A                                 N/A
GPU_L4_G1_96                                      4.239                            N/A                                 N/A
GPU_L4_G1_192                                     8.478                            N/A                                 N/A



Table 1(h): Table for Snowflake Openflow

Deployment Type                                                                                      Price
Openflow Compute – BYOC Deployment                                                 0.0225 Platform Credits per vCPU per Hour
Openflow Compute – Snowflake Deployment (SPCS)                               See “Table for First Generation SPCS Instances” above



Table 1(i): Table for Snowflake Postgres Compute5

                          Postgres Compute (Platform Credits per Hour)   Postgres Compute - High Availability (Platform Credits per Hour)
Instance Family                  AWS                       Azure                     AWS                               Azure
STANDARD_M                      0.0356                     0.0376                   0.0712                             0.0752

STANDARD_L                      0.0712                     0.0752                   0.1424                             0.1504

STANDARD_XL                     0.1424                     0.1504                   0.2848                             0.3008

STANDARD_2X                     0.2848                     0.3008                   0.5696                             0.6016



                                                                                                                                            3
 Table 1(i): Table for Snowflake Postgres Compute5

                            Postgres Compute (Platform Credits per Hour)            Postgres Compute - High Availability (Platform Credits per Hour)
 Instance Family                    AWS                         Azure                              AWS                                Azure
 STANDARD_4XL                      0.5696                       0.6016                            1.1392                             1.2032

 STANDARD_8XL                      1.1392                       1.2032                            2.2784                             2.4064

 STANDARD_12XL                     1.7088                       1.8048                            3.4176                             3.6096

 STANDARD_24XL                     3.4176                       3.6096                            6.8352                             7.2192

 HIGHMEM_L                         0.1024                       0.1088                            0.2048                             0.2176

 HIGHMEM_XL                        0.2048                       0.2176                            0.4096                             0.4352

 HIGHMEM_2XL                       0.4096                       0.4352                            0.8192                             0.8704

 HIGHMEM_4XL                       0.8192                       0.8704                            1.6384                             1.7408

 HIGHMEM_8XL                       1.6384                       1.7408                            3.2768                             3.4816

 HIGHMEM_12XL                      2.4576                       2.6112                            4.9152                             5.2224

 HIGHMEM_16XL                      3.2768                       3.4816                            6.5536                             6.9632

 HIGHMEM_24XL                      4.9152                       5.2224                            9.8304                             10.4448

 HIGHMEM_32XL                      6.5536                       6.9632                            13.1072                            13.9264

 HIGHMEM_48XL                      9.8304                      10.4448                            19.6608                            20.8896

 BURST_XS                          0.0068                          -                              0.0136                                -

 BURST_S                           0.0136                       0.0144                            0.0272                             0.0288

 BURST_M                           0.0272                       0.0288                            0.0544                             0.0576


Pricing. The Snowflake Service may be subscribed to on an On Demand basis, where usage is invoiced in arrears every month, or on a Capacity basis,
where a set dollar amount of usage is purchased up front. Pricing depends on whether you are in On Demand or Capacity.
Tables 2 and 3 below set forth Snowflake’s current On Demand Platform Credit Pricing, AI Credit Pricing (On Demand and Capacity), and Storage Pricing
(On Demand and Capacity) for a number of different Storage types.

 Table 2(a): On Demand Platform Credit Pricing

  Cloud Provider                    Region                                                   Snowflake Service Edition
                                                                  Standard               Enterprise           Business Critical               VPS6
        AWS              US East (Northern Virginia)                $2.00                   $3.00                   $4.00                     $6.00
        AWS                   US West (Oregon)                      $2.00                   $3.00                   $4.00                     $6.00
        AWS                        EU Dublin                        $2.60                   $3.90                   $5.20                     $7.80
        AWS                      EU Frankfurt                       $2.60                   $3.90                   $5.20                     $7.80
        AWS                       AP Sydney                         $2.75                   $4.05                   $5.50                     $8.25
        AWS                     AP Singapore                        $2.50                   $3.70                   $5.00                     $7.50
        AWS                     Canada Central                      $2.25                   $3.50                   $4.50                     $6.75
        AWS                    US East 2 (Ohio)                     $2.00                   $3.00                   $4.00                     $6.00
        AWS                AP Northeast 1 (Tokyo)                   $2.85                   $4.30                   $5.70                     $8.55
        AWS                       AP Mumbai                         $2.00                   $3.00                   $4.00                     $6.00
        AWS              US East 1 Commercial Gov                      -                      -                     $4.80                     $7.20
        AWS                    Europe (London)                      $2.70                   $4.00                   $5.40                     $8.10
        AWS                  Asia Pacific (Seoul)                   $2.75                   $4.05                   $5.50                     $8.25
        AWS                     US Gov West 1                          -                      -                     $5.60                     $8.40
                       US Gov West 1 (Fedramp High
        AWS                                                            -                      -                     $5.60                     $8.40
                                 Plus)
        AWS                  Europe (Stockholm)                     $2.40                   $3.60                   $4.80                     $7.20
        AWS                  Asia Pacific (Osaka)                   $2.85                   $4.30                   $5.70                     $8.55
                         South America East 1 (São
        AWS                                                         $3.10                   $4.65                   $6.20                     $9.30
                                 Paulo)


6 VPS is only available if specified on an Order Form. A separate annual fee per deployment is required and will be set forth in the applicable Order Form.

After expiration of the Subscription Term specified in such Order Form and while Customer is in On Demand, the annual deployment fee(s) specified in
the Order Form shall be charged for each deployment pro-rata on a monthly basis in arrears until such Order Form is terminated in accordance with its
terms or the parties execute a subsequent Order Form. Cloud Services are not charged in VPS accounts except as part of Serverless Features.

                                                                                                                                                          4
 Table 2(a): On Demand Platform Credit Pricing

  Cloud Provider                    Region                                                  Snowflake Service Edition
                                                                Standard                Enterprise           Business Critical               VPS6
        AWS                        EU (Paris)                      $2.60                    $3.90                  $5.20                     $7.80
        AWS                 Asia Pacific (Jakarta)                 $2.50                    $3.70                  $5.00                     $7.50
                       US Gov East 1 (Fedramp High
        AWS                                                          -                        -                    $5.60                     $8.40
                                  Plus)
        AWS                       EU (Zurich)                      $3.10                    $4.65                  $6.20                     $9.30
        AWS                US Gov West 1 (DoD)                       -                        -                    $5.60                     $8.40
                        US West (Commercial Gov -
        AWS                                                          -                        -                    $4.80                     $7.20
                                 Oregon)
        AWS                  Africa (Cape Town)                    $2.80                    $4.20                  $5.60                     $8.40
        AWS                Asia Pacific (Malaysia)                 $2.40                    $3.60                  $4.80                     $7.20
        AWS                Asia Pacific (Thailand)                 $2.40                    $3.60                  $4.80                     $7.20
        AWS              Asia Pacific (New Zealand)                $2.75                    $4.05                  $5.50                     $8.25
       Azure                East US 2 (Virginia)                   $2.00                    $3.00                  $4.00                     $6.00
       Azure              West US 2 (Washington)                   $2.00                    $3.00                  $4.00                     $6.00
       Azure            West Europe (Netherlands)                  $2.60                    $3.90                  $5.20                     $7.80
                         Australia East (New South
       Azure                                                       $2.75                    $4.05                  $5.50                     $8.25
                                   Wales)
       Azure             Canada Central (Toronto)                  $2.25                    $3.50                  $4.50                     $6.75
       Azure            Southeast Asia (Singapore)                 $2.50                    $3.70                  $5.00                     $7.50
       Azure                 Switzerland North                     $3.10                    $4.65                  $6.20                     $9.30
       Azure                  US Gov Virginia                        -                        -                    $5.60                     $8.40
       Azure                  Central US (Iowa)                    $2.00                    $3.00                  $4.00                     $6.00
       Azure               North Europe (Ireland)                  $2.60                    $3.90                  $5.20                     $7.80
       Azure                 Japan East (Tokyo)                    $2.85                    $4.30                  $5.70                     $8.55
       Azure                 UAE North (Dubai)                     $2.70                    $4.00                  $5.40                     $8.10
       Azure             South Central US (Texas)                  $2.00                    $3.00                  $4.00                     $6.00
       Azure                Central India (Pune)                   $2.00                    $3.00                  $4.00                     $6.00
       Azure                 UK South (London)                     $2.70                    $4.00                  $5.40                     $8.10
                        US Gov Virginia (Fed Ramp
       Azure                                                         -                        -                    $5.60                     $8.40
                               High Plus)
       Azure                     Mexico Central                    $2.00                    $3.00                  $4.00                     $6.00
       Azure                     Korea Central                     $2.75                    $4.05                  $5.50                     $8.25
       Azure                     Sweden Central                    $2.40                    $3.60                  $4.80                     $7.20
       Azure                 East US (Virginia)                    $2.00                    $3.00                  $4.00                     $6.00
        GCP                  US Central 1 (Iowa)                   $2.00                    $3.00                  $4.00                     $6.00
        GCP                US East 4 (N. Virginia)                 $2.00                    $3.00                  $4.00                     $6.00
        GCP             Europe West 4 (Netherlands)                $2.60                    $3.90                  $5.20                     $7.80
        GCP               Europe West 2 (London)                   $2.70                    $4.00                  $5.40                     $8.10
        GCP              Europe West 3 (Frankfurt)                 $2.60                    $3.90                  $5.20                     $7.80
                           Middle East Central 2
        GCP                                                        $3.25                    $4.90                  $6.50                     $9.75
                                (Dammam)
                           Australia Southeast 2
        GCP                                                        $2.75                    $4.05                  $5.50                     $8.25
                               (Melbourne)

While in On Demand, your Platform Credit pricing is as set forth in Table 2(a) (On Demand Platform Credit Pricing). Capacity Platform Credit Pricing is
based on applying the Platform Credit Discount in your Order Form to the On Demand Platform Credit Price for the applicable Cloud Provider, Region, and
Edition. For clarity, the Platform Credit Discount does not apply to AI Credits. Purchased Capacity may be applied toward any of your Accounts on the
Snowflake Service. Snowflake will provide you with monthly statements of usage, in arrears.



 Table 2(b): AI Credit Pricing

                                                                             Capacity AI Credit Pricing by ACV Range
                                     On           Tier 1        Tier 2         Tier 3         Tier 4         Tier 5              Tier 6         Tier 7
              Type                 Demand           USD           USD            USD              USD            USD             USD              USD
                                   Pricing          $0 -      $1,200,000 -   $3,000,000 -     $5,000,000 -   $10,000,000 -   $20,000,000 -    $40,000,000+
                                                 $1,199,999    $2,999,999     $4,999,999       $9,999,999     $19,999,999     $39,999,999
            Global                  $2.00          $2.00        $1.96           $1.96             $1.94         $1.92            $1.90           $1.88
           Regional                 $2.20          $2.20        $2.16           $2.16             $2.13         $2.11            $2.09           $2.07

                                                                                                                                                         5
While in On Demand, your Snowflake AI Credit pricing is as set forth in the “On Demand Pricing” column in Table 2(b) (AI Credit Pricing). Capacity AI Credit
Pricing is based on the annualized contract value of the applicable Order Form, except as otherwise indicated on such Order Form. The annualized contract
values listed underneath each tier in Tables 2(b) and 3(a) (“ACV Ranges”) are for illustrative purposes only, and if the designated tier(s) for AI Credit and/or
Storage for the ACV Ranges listed in any such tables conflicts with the respective tier(s) indicated on an applicable Order Form, then the indicated tier(s)
on the applicable Order Form will control. For clarity and the avoidance of doubt, neither Additional Capacity Orders, nor unused but paid for Capacity from
prior Order Forms made available for use (i.e., rolled over) during the Subscription Term of the applicable Order Form, shall be included for purposes of
calculating the ACV Range in Tables 2(b) and 3(a). Additionally, Cortex Inference may not be eligible for Capacity AI Credit Pricing.




 Table 3(a): Standard Storage Pricing

                                                                            Capacity Storage Pricing by ACV Range (TB/mo)
                                      On           Tier 1          Tier 2         Tier 3         Tier 4       Tier 5                Tier 6           Tier 7
                                   Demand
   Cloud
                   Region          Storage          USD              USD             USD             USD             USD             USD              USD
  Provider
                                    Pricing         $0 -         $1,200,000 -    $3,000,000 -    $5,000,000 -    $10,000,000 -   $20,000,000 -    $40,000,000+
                                   (TB/mo)       $1,199,999       $2,999,999      $4,999,999      $9,999,999      $19,999,999     $39,999,999

                     US East
    AWS            (Northern        $23.00         $23.00          $21.47          $19.94          $18.40           $16.86          $15.34          $13.80
                    Virginia)
                    US West
    AWS                             $23.00         $23.00          $21.47          $19.94          $18.40           $16.86          $15.34          $13.80
                    (Oregon)
    AWS            EU Dublin        $23.00         $23.00          $21.47          $19.94          $18.40           $16.86          $15.34          $13.80
    AWS         EU Frankfurt        $24.50         $24.50          $22.87          $21.24          $19.60           $17.96          $16.34          $14.70
    AWS           AP Sydney         $25.00         $25.00          $23.33          $21.68          $20.00           $18.33          $16.68          $15.00
    AWS        AP Singapore         $25.00         $25.00          $23.33          $21.68          $20.00           $18.33          $16.68          $15.00
                     Canada
    AWS                             $25.00         $25.00          $23.33          $21.68          $20.00           $18.33          $16.68          $15.00
                     Central
                   US East 2
    AWS                             $23.00         $23.00          $21.47          $19.94          $18.40           $16.86          $15.34          $13.80
                      (Ohio)
                AP Northeast
    AWS                             $25.00         $25.00          $23.33          $21.68          $20.00           $18.33          $16.68          $15.00
                    1 (Tokyo)
    AWS          AP Mumbai          $23.00         $23.00          $21.47          $19.94          $18.40           $16.86          $15.34          $13.80
                   US East 1
    AWS          Commercial         $23.00         $23.00          $21.47          $19.94          $18.40           $16.86          $15.34          $13.80
                       Gov
                     Europe
    AWS                             $24.00         $24.00          $22.40          $20.81          $19.20           $17.59          $16.01          $14.40
                    (London)
                 Asia Pacific
    AWS                             $25.00         $25.00          $23.33          $21.68          $20.00           $18.33          $16.68          $15.00
                     (Seoul)
                US Gov West
    AWS                             $39.00         $39.00          $36.40          $33.81          $31.20           $28.59          $26.01          $23.40
                        1
                US Gov West
    AWS           1 (Fedramp        $39.00         $39.00          $36.40          $33.81          $31.20           $28.59          $26.01          $23.40
                  High Plus)
                     Europe
    AWS                             $23.00         $23.00          $21.47          $19.94          $18.40           $16.86          $15.34          $13.80
                 (Stockholm)
                 Asia Pacific
    AWS                             $25.00         $25.00          $23.33          $21.68          $20.00           $18.33          $16.68          $15.00
                     (Osaka)
                      South
    AWS         America East        $40.50         $40.50          $37.80          $35.11          $32.40           $29.69          $27.01          $24.30
                1 (São Paulo)
    AWS            EU (Paris)       $24.00         $24.00          $22.40          $20.81          $19.20           $17.59          $16.01          $14.40
                 Asia Pacific
    AWS                             $25.00         $25.00          $23.33          $21.68          $20.00           $18.33          $16.68          $15.00
                    (Jakarta)
               US Gov East 1
    AWS            (Fedramp         $39.00         $39.00          $36.40          $33.81          $31.20           $28.59          $26.01          $23.40
                  High Plus)
    AWS           EU (Zurich)       $26.95         $26.95          $25.15          $23.37          $21.56           $19.75          $17.98          $16.17
                US Gov West
    AWS                             $39.00         $39.00          $36.40          $33.81          $31.20           $28.59          $26.01          $23.40
                     1 (DoD)
                    US West
    AWS         (Commercial         $23.00         $23.00          $21.47          $19.94          $18.40           $16.86          $15.34          $13.80
               Gov - Oregon)
                 Africa (Cape
    AWS                             $27.40         $27.40          $25.57          $23.76          $21.92           $20.08          $18.28          $16.44
                      Town)
                 Asia Pacific
    AWS                             $22.50         $22.50          $21.00          $19.51          $18.00           $16.49          $15.01          $13.50
                   (Malaysia)
                 Asia Pacific
    AWS                             $22.50         $22.50          $21.00          $19.51          $18.00           $16.49          $15.01          $13.50
                   (Thailand)
                 Asia Pacific
    AWS                             $26.25         $26.25          $24.50          $22.76          $21.00           $19.24          $17.51          $15.75
               (New Zealand)
                   East US 2
   Azure                            $23.00         $23.00          $21.47          $19.94          $18.40           $16.86          $15.34          $13.80
                    (Virginia)
                  West US 2
   Azure                            $23.00         $23.00          $21.47          $19.94          $18.40           $16.86          $15.34          $13.80
                (Washington)
   Azure        West Europe         $23.00         $23.00          $21.47          $19.94          $18.40           $16.86          $15.34          $13.80

                                                                                                                                                              6
 Table 3(a): Standard Storage Pricing

                                                                          Capacity Storage Pricing by ACV Range (TB/mo)
                                     On          Tier 1          Tier 2         Tier 3         Tier 4       Tier 5              Tier 6         Tier 7
                                  Demand
   Cloud
                   Region         Storage         USD             USD             USD             USD            USD             USD             USD
  Provider
                                   Pricing        $0 -        $1,200,000 -    $3,000,000 -    $5,000,000 -   $10,000,000 -   $20,000,000 -   $40,000,000+
                                  (TB/mo)      $1,199,999      $2,999,999      $4,999,999      $9,999,999     $19,999,999     $39,999,999

               (Netherlands)
               Australia East
   Azure         (New South        $25.00        $25.00         $23.33          $21.68          $20.00         $18.33          $16.68          $15.00
                      Wales)
                    Canada
   Azure             Central       $25.00        $25.00         $23.33          $21.68          $20.00         $18.33          $16.68          $15.00
                   (Toronto)
                  Southeast
   Azure               Asia        $25.00        $25.00         $23.33          $21.68          $20.00         $18.33          $16.68          $15.00
                (Singapore)
                Switzerland
   Azure                           $28.80        $28.80         $26.88          $24.97          $23.04         $21.11          $19.21          $17.28
                       North
                    US Gov
   Azure                           $39.00        $39.00         $36.40          $33.81          $31.20         $28.59          $26.01          $23.40
                    Virginia
                  US Central
   Azure                           $23.00        $23.00         $21.47          $19.94          $18.40         $16.86          $15.34          $13.80
                      (Iowa)
               North Europe
   Azure                           $23.00        $23.00         $21.47          $19.94          $18.40         $16.86          $15.34          $13.80
                    (Ireland)
                 Japan East
   Azure                           $25.00        $25.00         $23.33          $21.68          $20.00         $18.33          $16.68          $15.00
                     (Tokyo)
                  UAE North
   Azure                           $25.40        $25.40         $23.71          $22.02          $20.32         $18.62          $16.94          $15.24
                     (Dubai)
               South Central
   Azure                           $23.00        $23.00         $21.47          $19.94          $18.40         $16.86          $15.34          $13.80
                 US (Texas)
                Central India
   Azure                           $23.00        $23.00         $21.47          $19.94          $18.40         $16.86          $15.34          $13.80
                      (Pune)
                   UK South
   Azure                           $24.00        $24.00         $22.40          $20.81          $19.20         $17.59          $16.01          $14.40
                   (London)
                    US Gov
                Virginia (Fed
   Azure                           $39.00        $39.00         $36.40          $33.81          $31.20         $28.59          $26.01          $23.40
                 Ramp High
                       Plus)
                     Mexico
   Azure                           $23.00        $23.00         $21.47          $19.94          $18.40         $16.86          $15.34          $13.80
                     Central
   Azure       Korea Central       $25.00        $25.00         $23.33          $21.68          $20.00         $18.33          $16.68          $15.00
                    Sweden
   Azure                           $23.00        $23.00         $21.47          $19.94          $18.40         $16.86          $15.34          $13.80
                     Central
                    East US
   Azure                           $23.00        $23.00         $21.47          $19.94          $18.40         $16.86          $15.34          $13.80
                   (Virginia)
                US Central 1
    GCP                            $20.00        $20.00         $20.00          $20.00          $20.00         $20.00          $20.00          $20.00
                      (Iowa)
                US East 4 (N.
    GCP                            $23.00        $23.00         $23.00          $23.00          $23.00         $23.00          $23.00          $23.00
                    Virginia)
               Europe West 4
    GCP                            $20.00        $20.00         $20.00          $20.00          $20.00         $20.00          $20.00          $20.00
               (Netherlands)
               Europe West 2
    GCP                            $23.00        $23.00         $23.00          $23.00          $23.00         $23.00          $23.00          $23.00
                   (London)
               Europe West 3
    GCP                            $23.00        $23.00         $23.00          $23.00          $23.00         $23.00          $23.00          $23.00
                  (Frankfurt)
                Middle East
    GCP            Central 2       $30.00        $30.00         $30.00          $30.00          $30.00         $30.00          $30.00          $30.00
                 (Dammam)
                   Australia
    GCP         Southeast 2        $23.00        $23.00         $23.00          $23.00          $23.00         $23.00          $23.00          $23.00
                (Melbourne)

For any Order Form designated as a “Qualifying Order” by its express written terms, as of the Order Form Effective Date of such Qualifying Order, the
“Capacity Storage Price” for Accounts procured thereunder shall be the applicable adjusted capacity storage price indicated in the table above, based upon
the indicated “Capacity Storage Tier” and the respective Snowflake Service Edition(s), Region(s) and Cloud Provider(s) procured under such Qualifying
Order. Absent any express terms to the contrary in the applicable Order Form or Agreement, the Capacity Storage Price for Accounts procured under Order
Forms not expressly designated as a “Qualifying Order” shall be based on the “Tier 1” rates in the table above.
For clarity, any references to “storage” (e.g., “Capacity Storage Price,” “Capacity Storage Tier(s),” or “Storage Discount(s)”) indicated on an Order Form
shall be understood in reference to the Standard Storage Pricing set forth in Table 3(a), and not in reference to any other storage type set forth below.




                                                                                                                                                        7
Table 3(b): Hybrid Tables Storage Pricing

 Cloud
                            Region                          On Demand & Capacity Hybrid Tables Storage Pricing (GB/mo)
Provider

 AWS             US East (Northern Virginia)                                          $0.34
 AWS                  US West (Oregon)                                                $0.34
 AWS                      EU Dublin                                                   $0.34
 AWS                     EU Frankfurt                                                 $0.36
 AWS                      AP Sydney                                                   $0.37
 AWS                     AP Singapore                                                 $0.37
 AWS                    Canada Central                                                $0.37
 AWS                   US East 2 (Ohio)                                               $0.34
 AWS               AP Northeast 1 (Tokyo)                                             $0.37
 AWS                      AP Mumbai                                                   $0.34
 AWS             US East 1 Commercial Gov                                             $0.34
 AWS                   Europe (London)                                                $0.35
 AWS                 Asia Pacific (Seoul)                                             $0.37
 AWS                    US Gov West 1                                                 $0.58
 AWS         US Gov West 1 (Fedramp High Plus)                                        $0.58
 AWS                Europe (Stockholm)                                                $0.34
 AWS                Asia Pacific (Osaka)                                              $0.37
 AWS          South America East 1 (São Paulo)                                        $0.60
 AWS                      EU (Paris)                                                  $0.35
 AWS                Asia Pacific (Jakarta)                                            $0.37
 AWS         US Gov East 1 (Fedramp High Plus)                                        $0.58
 AWS                      EU (Zurich)                                                 $0.40
 AWS                US Gov West 1 (DoD)                                               $0.58
 AWS         US West (Commercial Gov - Oregon)                                        $0.34
 AWS                 Africa (Cape Town)                                               $0.40
 AWS               Asia Pacific (Malaysia)                                            $0.37
 AWS               Asia Pacific (Thailand)                                            $0.37
 AWS             Asia Pacific (New Zealand)                                           $0.43
 Azure               East US 2 (Virginia)                                             $0.34
 Azure            West US 2 (Washington)                                              $0.34
 Azure           West Europe (Netherlands)                                            $0.34
 Azure        Australia East (New South Wales)                                        $0.37
 Azure            Canada Central (Toronto)                                            $0.37
 Azure           Southeast Asia (Singapore)                                           $0.37
 Azure                Switzerland North                                               $0.43
 Azure                Central US (Iowa)                                               $0.34
 Azure             North Europe (Ireland)                                             $0.34
 Azure               Japan East (Tokyo)                                               $0.37
 Azure                UAE North (Dubai)                                               $0.38
 Azure            South Central US (Texas)                                            $0.34
 Azure              Central India (Pune)                                              $0.37
 Azure               UK South (London)                                                $0.35
 Azure                  Mexico Central                                                $0.34
 Azure                   Korea Central                                                $0.37
 Azure                 Sweden Central                                                 $0.34
 Azure                East US (Virginia)                                              $0.34


Table 3(c): SPCS Block Storage Pricing


 Cloud                                           Compute                        Block Storage     Block Storage    Block Storage
Provider                   Region                Instance     Block Storage      IOPS (1,000        Throughput       Snapshot
                                                   Type      Volume (TB/mo)       IOPS-mo)         (GB/sec-mo)        (TB/mo)
  AWS            US East (Northern Virginia)     CPU/GPU         $81.92             $5.00             $40.96           $51.20
  AWS                US West (Oregon)            CPU/GPU         $81.92             $5.00             $40.96           $51.20
  AWS                    EU Dublin               CPU/GPU         $90.12             $5.50             $45.06           $51.20
  AWS                  EU Frankfurt              CPU/GPU         $97.49             $6.00             $48.75           $55.30
  AWS                   AP Sydney                CPU/GPU         $98.31             $6.00             $49.16           $56.30
  AWS                  AP Singapore              CPU/GPU         $98.31             $6.00             $49.16           $51.20
  AWS                 Canada Central             CPU/GPU         $90.12             $5.50             $45.06           $56.30
  AWS                US East 2 (Ohio)            CPU/GPU         $81.92             $5.00             $40.96           $51.20
  AWS             AP Northeast 1 (Tokyo)         CPU/GPU         $98.31             $6.00             $49.16           $51.20
  AWS                   AP Mumbai                CPU/GPU         $81.92             $5.00             $40.96           $51.20
  AWS            US East 1 Commercial Gov        CPU/GPU         $81.92             $5.00             $40.96           $51.20
  AWS                Europe (London)             CPU/GPU         $95.03             $5.80             $47.52           $54.30
  AWS               Asia Pacific (Seoul)         CPU/GPU         $93.39             $5.70             $46.70           $51.20
  AWS                 US Gov West 1              CPU/GPU         $98.31             $6.00             $49.16           $67.60
  AWS        US Gov West 1 (Fedramp High Plus)   CPU/GPU         $98.31             $6.00             $49.16           $67.60


                                                                                                                                   8
Table 3(c): SPCS Block Storage Pricing


 Cloud                                                    Compute                            Block Storage       Block Storage        Block Storage
Provider                       Region                     Instance     Block Storage          IOPS (1,000          Throughput           Snapshot
                                                            Type      Volume (TB/mo)           IOPS-mo)           (GB/sec-mo)            (TB/mo)
 AWS                  Europe (Stockholm)                  CPU/GPU          $85.61                $5.20               $42.81               $48.60
 AWS                  Asia Pacific (Osaka)                CPU/GPU          $98.31                $6.00               $49.16               $51.20
 AWS           South America East 1 (São Paulo)           CPU/GPU         $155.65                $9.50               $77.83               $69.60
 AWS                       EU (Paris)                     CPU/GPU          $95.03                $5.80               $47.52               $54.30
 AWS                 Asia Pacific (Jakarta)               CPU/GPU          $98.31                $6.00               $49.16               $51.20
 AWS          US Gov East 1 (Fedramp High Plus)           CPU/GPU          $98.31                $6.00               $49.16               $67.60
 AWS                      EU (Zurich)                     CPU/GPU         $116.95                $7.00               $58.48               $60.40
 AWS                 US Gov West 1 (DoD)                  CPU/GPU          $98.31                $6.00               $49.16               $67.60
 AWS          US West (Commercial Gov - Oregon)           CPU/GPU          $81.92                $5.00               $40.96               $51.20
 AWS                  Africa (Cape Town)                  CPU/GPU         $107.21                $6.50               $53.25               $60.93
 AWS                 Asia Pacific (Malaysia)              CPU/GPU          $88.47                $5.40               $44.24               $46.08
 AWS                 Asia Pacific (Thailand)              CPU/GPU          $88.47                $5.40               $44.24               $46.08
 AWS              Asia Pacific (New Zealand)              CPU/GPU         $103.22                $6.30               $51.61               $59.08
 Azure                East US 2 (Virginia)                CPU/GPU          $82.23                $5.11               $41.12               $51.20
 Azure              West US 2 (Washington)                CPU/GPU          $82.20                $5.11               $41.12               $51.20
 Azure            West Europe (Netherlands)               CPU/GPU          $97.18                $5.84               $48.59               $51.20
 Azure         Australia East (New South Wales)           CPU/GPU          $82.23                $5.11               $41.12               $56.30
 Azure             Canada Central (Toronto)               CPU/GPU          $90.40                $5.84               $44.86               $56.30
 Azure            Southeast Asia (Singapore)              CPU/GPU          $98.68                $5.84               $49.34               $51.20
 Azure                 Switzerland North                  CPU/GPU         $117.37                $7.30               $58.31               $56.30
 Azure                  US Gov Virginia                   CPU/GPU          $98.68                $5.84               $49.34              $107.00
 Azure                 Central US (Iowa)                  CPU/GPU          $92.70                $5.84               $46.35               $51.20
 Azure               North Europe (Ireland)               CPU/GPU          $82.23                $5.11               $41.12               $51.20
 Azure                Japan East (Tokyo)                  CPU/GPU          $98.68                $5.84               $49.34               $51.20
 Azure                 UAE North (Dubai)                  CPU/GPU          $99.42                $5.84               $49.34               $61.40
 Azure             South Central US (Texas)               CPU/GPU          $82.23                $5.11               $41.12               $51.20
 Azure                Central India (Pune)                CPU/GPU          $82.23                $5.11               $41.12               $51.20
 Azure                UK South (London)                   CPU/GPU          $94.94                $5.84               $47.10               $54.30
                US Gov Virginia (Fed Ramp High
 Azure                        Plus)                       CPU/GPU             $98.68                $5.84            $49.34              $107.00
 Azure                   Mexico Central                   CPU/GPU             $90.11                $7.30            $45.06               $56.32
 Azure                   Korea Central                    CPU/GPU             $93.39                $5.70            $47.10               $51.20
 Azure                  Sweden Central                    CPU/GPU             $82.94                $5.20            $41.98               $51.20
 Azure                 East US (Virginia)                 CPU/GPU             $82.94                $5.20            $41.98               $51.20
 GCP                  US Central 1 (Iowa)                   CPU               $81.92                $5.00            $40.96               $51.20
 GCP                  US Central 1 (Iowa)                   GPU               $81.92                  -             $122.88               $51.20
 GCP                 US East 4 (N. Virginia)                CPU               $81.92                $5.00            $40.96               $51.20
 GCP                 US East 4 (N. Virginia)                GPU               $81.92                  -             $122.88               $51.20
 GCP             Europe West 4 (Netherlands)                CPU               $86.02                $5.00            $43.01               $54.27
 GCP             Europe West 4 (Netherlands)                GPU               $86.02                  -             $129.02               $54.27
 GCP                Europe West 2 (London)                  CPU               $93.18                $6.00            $47.10               $58.37
 GCP                Europe West 2 (London)                  GPU               $93.18                  -             $140.29               $58.37
 GCP               Europe West 3 (Frankfurt)                CPU               $96.26                $6.00            $48.13               $60.42
 GCP               Europe West 3 (Frankfurt)                GPU               $96.26                  -             $145.41               $60.42
 GCP           Middle East Central 2 (Dammam)               CPU              $131.07                $8.00            $65.54               $81.92
 GCP           Middle East Central 2 (Dammam)               GPU              $131.07                  -             $196.61               $81.92
 GCP           Australia Southeast 2 (Melbourne)            CPU              $106.50                $7.00            $53.23               $66.56
 GCP           Australia Southeast 2 (Melbourne)            GPU              $106.50                  -             $159.74               $66.56


Table 3(d): Egress Cost Optimizer (“ECO”)

           Cloud Provider                                                         ECO Cache (TB/mo)

             Cloudflare                                                                    $16.90


Table 3(e): Archive Storage and Data Retrieval Pricing

                                                                               Cool Tier                                  Cold Tier

                                                                                       Retrieval (per TB                         Retrieval (per TB
Cloud Provider                       Region                    Storage (TB/mo)                               Storage (TB/mo)
                                                                                       data processed)                           data processed)
     AWS                    US East (Northern Virginia)              $4.00                  $30.00                $1.00                 $2.50
     AWS                        US West (Oregon)                     $4.00                  $30.00                $1.00                 $2.50

                                                                                                                                                      9
Table 3(e): Archive Storage and Data Retrieval Pricing

                                                                       Cool Tier                              Cold Tier

                                                                             Retrieval (per TB                      Retrieval (per TB
Cloud Provider                    Region                  Storage (TB/mo)                        Storage (TB/mo)
                                                                             data processed)                        data processed)
     AWS                        EU Dublin                      $4.00               $30.00             $1.00               $3.00
     AWS                      EU Frankfurt                     $5.00               $30.00             $1.00               $5.00
     AWS                       AP Sydney                       $5.00               $30.00             $1.00               $5.00
     AWS                      AP Singapore                     $5.00               $30.00             $1.00               $5.00
     AWS                     Canada Central                    $5.00               $30.00             $1.00               $5.00
     AWS                    US East 2 (Ohio)                   $4.00               $30.00             $1.00               $2.50
     AWS                AP Northeast 1 (Tokyo)                 $5.00               $30.00             $1.00               $5.00
     AWS                       AP Mumbai                       $5.00               $30.00             $1.00               $5.00
     AWS               US East 1 Commercial Gov                $4.00               $30.00             $1.00               $2.50
     AWS                    Europe (London)                    $5.00               $30.00             $1.00               $5.00
     AWS                   Asia Pacific (Seoul)                $5.00               $30.00             $1.00               $5.00
     AWS                     US Gov West 1                     $6.40               $30.00             $1.20               $3.40
     AWS           US Gov West 1 (Fedramp High Plus)           $6.40               $30.00             $1.20               $3.40
     AWS                  Europe (Stockholm)                   $4.00               $30.00             $1.00               $3.00
     AWS                  Asia Pacific (Osaka)                 $5.00               $30.00             $1.00               $5.00
     AWS            South America East 1 (São Paulo)           $8.30               $30.00             $1.40               $8.00
     AWS                        EU (Paris)                     $5.00               $30.00             $1.00               $5.00
     AWS                  Asia Pacific (Jakarta)               $5.00               $30.00             $1.00               $5.00
     AWS           US Gov East 1 (Fedramp High Plus)           $6.40               $30.00             $1.20               $3.40
     AWS                       EU (Zurich)                     $5.50               $30.00             $1.00               $5.00
     AWS                  US Gov West 1 (DoD)                  $6.40               $30.00             $1.20               $3.40
     AWS           US West (Commercial Gov - Oregon)           $4.00               $30.00             $1.00               $2.50
     AWS                   Africa (Cape Town)                  $5.00               $30.00             $1.00               $5.00
     AWS                 Asia Pacific (Malaysia)               $4.50               $30.00             $1.80               $4.50
     AWS                  Asia Pacific (Thailand)              $4.50               $30.00             $1.80               $4.50
     AWS                Asia Pacific (New Zealand)             $5.25               $30.00             $2.10               $5.25
     Azure                  East US 2 (Virginia)               $4.00               $30.00               -                   -
     Azure               West US 2 (Washington)                $4.00               $30.00               -                   -
     Azure             West Europe (Netherlands)               $5.00               $30.00               -                   -
     Azure           Australia East (New South Wales)          $5.00               $30.00               -                   -
     Azure              Canada Central (Toronto)               $5.00               $30.00               -                   -
     Azure             Southeast Asia (Singapore)              $5.00               $30.00               -                   -
     Azure                   Switzerland North                 $5.71               $42.90               -                   -
     Azure                    US Gov Virginia                  $6.40               $30.00               -                   -
     Azure                   US Central (Iowa)                 $4.92               $36.90               -                   -
     Azure                North Europe (Ireland)               $4.00               $30.00               -                   -
     Azure                  Japan East (Tokyo)                 $5.00               $30.00               -                   -
     Azure                   UAE North (Dubai)                 $5.00               $30.00               -                   -
     Azure              South Central US (Texas)               $4.80               $36.00               -                   -
     Azure                 Central India (Pune)                $5.00               $30.00               -                   -
     Azure                  UK South (London)                  $5.00               $30.00               -                   -
     Azure         US Gov Virginia (Fed Ramp High Plus)        $6.40               $30.00               -                   -
     Azure                     Mexico Central                  $4.95               $33.00               -                   -
     Azure                     Korea Central                   $5.00               $30.00               -                   -
     Azure                    Sweden Central                   $4.00               $30.00               -                   -
     Azure                   East US (Virginia)                $4.00               $30.00               -                   -
     GCP                    US Central 1 (Iowa)                $4.00               $20.00             $1.20               $50.00
     GCP                  US East 4 (N. Virginia)              $6.00               $20.00             $2.50               $50.00
     GCP                 Europe West 2 (London)                $7.00               $20.00             $2.50               $50.00
     GCP                Europe West 3 (Frankfurt)              $6.00               $20.00             $2.50               $50.00
     GCP               Europe West 4 (Netherlands)             $4.00               $20.00             $1.20               $50.00
     GCP             Middle East Central 2 (Dammam)            $6.00               $20.00             $2.70               $50.00
     GCP             Australia Southeast 2 (Melbourne)         $6.00               $20.00             $2.50               $50.00




                                                                                                                                   10
    Table 3(f): Snowflake Postgres Storage7

                                                                                                              Postgres Storage - High Availability
    Cloud Provider                    Region                          Postgres Storage (TB/mo)
                                                                                                                           (TB/mo)
        AWS                US East (Northern Virginia)                          $117.76                                    $235.52
        AWS                     US West (Oregon)                                $117.76                                    $235.52
        AWS                         EU Dublin                                   $129.55                                    $259.10
        AWS                        EU Frankfurt                                 $140.15                                    $280.30
        AWS                         AP Sydney                                   $141.32                                    $282.64
        AWS                        AP Singapore                                 $141.32                                    $282.64
        AWS                       Canada Central                                $129.55                                    $259.10
        AWS                      US East 2 (Ohio)                               $117.76                                    $235.52
        AWS                  AP Northeast 1 (Tokyo)                             $141.32                                    $282.64
        AWS                         AP Mumbai                                   $117.76                                    $235.52
        AWS                US East 1 Commercial Gov                             $117.76                                    $235.52
        AWS                      Europe (London)                                $136.60                                    $273.20
        AWS                    Asia Pacific (Seoul)                             $134.25                                    $268.50
        AWS                       US Gov West 1                                 $141.32                                    $282.64
        AWS            US Gov West 1 (Fedramp High Plus)                        $141.32                                    $282.64
        AWS                   Europe (Stockholm)                                $123.06                                    $246.12
        AWS                   Asia Pacific (Osaka)                              $141.32                                    $282.64
        AWS             South America East 1 (São Paulo)                        $223.74                                    $447.48
        AWS                         EU (Paris)                                  $136.60                                    $273.20
        AWS                   Asia Pacific (Jakarta)                            $141.32                                    $282.64
        AWS            US Gov East 1 (Fedramp High Plus)                        $141.32                                    $282.64
        AWS                         EU (Zurich)                                 $168.11                                    $336.22
        AWS                   US Gov West 1 (DoD)                               $141.32                                    $282.64
        AWS            US West (Commercial Gov -Oregon)                         $117.76                                    $235.52
        AWS                    Africa (Cape Town)                               $154.11                                    $308.22
        AWS                  Asia Pacific (Malaysia)                            $127.18                                    $254.36
        AWS                  Asia Pacific (Thailand)                            $127.18                                    $254.36
        AWS                Asia Pacific (New Zealand)                           $148.38                                    $296.76
        Azure                  East US 2 (Virginia)                             $118.21                                    $236.42
        Azure               West US 2 (Washington)                              $118.16                                    $236.32
        Azure              West Europe (Netherlands)                            $139.70                                    $279.40
        Azure           Australia East (New South Wales)                        $118.21                                    $236.42
        Azure               Canada Central (Toronto)                            $129.95                                    $259.90
        Azure              Southeast Asia (Singapore)                           $141.85                                    $283.70
        Azure                   Switzerland North                               $168.71                                    $337.42
        Azure                    US Gov Virginia                                $141.85                                    $283.70
        Azure                   Central US (Iowa)                               $133.26                                    $266.52
        Azure                North Europe (Ireland)                             $118.21                                    $236.42
        Azure                  Japan East (Tokyo)                               $141.85                                    $283.70
        Azure                   UAE North (Dubai)                               $142.91                                    $285.82
        Azure               South Central US (Texas)                            $118.21                                    $236.42
        Azure                 Central India (Pune)                              $118.21                                    $236.42
        Azure                  UK South (London)                                $136.47                                    $272.94
        Azure         US Gov Virginia (Fed Ramp High Plus)                      $141.85                                    $283.70
        Azure                     Mexico Central                                $129.54                                    $259.08
        Azure                    Sweden Central                                 $118.78                                    $237.56
        Azure                      Korea Central                                $134.25                                    $268.50
        Azure                   East US (Virginia)                              $119.23                                    $238.46


    Table 3(g): Cloud Storage Requests5

                                                                      Class 1 Requests (per 1 million              Class 2 Requests (per 1 million
    Cloud Provider                    Region
                                                                                requests)                                    requests)
         AWS                US East (Northern Virginia)                             $5.00                                      $0.40
         AWS                    US West (Oregon)                                    $5.00                                      $0.40
         AWS                        EU Dublin                                       $5.00                                      $0.40
         AWS                      EU Frankfurt                                      $5.40                                      $0.43
         AWS                       AP Sydney                                        $5.50                                      $0.44
         AWS                      AP Singapore                                      $5.00                                      $0.40
         AWS                     Canada Central                                     $5.50                                      $0.44
         AWS                    US East 2 (Ohio)                                    $5.00                                      $0.40
         AWS                 AP Northeast 1 (Tokyo)                                 $4.70                                      $0.37
         AWS                       AP Mumbai                                        $5.00                                      $0.40
         AWS                US East 1 Commercial Gov                                $5.00                                      $0.40
         AWS                    Europe (London)                                     $5.30                                      $0.42
         AWS                   Asia Pacific (Seoul)                                 $4.50                                      $0.35

7
  As further described in the Documentation, some regions listed are available as Preview, subject to the Preview Terms at
https://www.snowflake.com/en/legal/.
                                                                                                                                                     11
Table 3(g): Cloud Storage Requests5

                                                            Class 1 Requests (per 1 million          Class 2 Requests (per 1 million
Cloud Provider                    Region
                                                                      requests)                                requests)
    AWS                       US Gov West 1                              $5.00                                      $0.40
    AWS            US Gov West 1 (Fedramp High Plus)                     $5.00                                      $0.40
    AWS                    Europe (Stockholm)                            $5.00                                      $0.40
    AWS                    Asia Pacific (Osaka)                          $4.70                                      $0.37
    AWS             South America East 1 (São Paulo)                     $7.00                                      $0.56
    AWS                         EU (Paris)                               $5.30                                      $0.42
    AWS                   Asia Pacific (Jakarta)                         $5.00                                      $0.40
    AWS            US Gov East 1 (Fedramp High Plus)                     $5.00                                      $0.40
    AWS                        EU (Zurich)                               $5.40                                      $0.43
    AWS                   US Gov West 1 (DoD)                            $5.00                                      $0.40
    AWS            US West (Commercial Gov - Oregon)                     $5.00                                      $0.40
    AWS                    Africa (Cape Town)                            $6.00                                      $0.40
    AWS                   Asia Pacific (Malaysia)                        $4.50                                      $0.36
    AWS                   Asia Pacific (Thailand)                        $4.50                                      $0.36
    AWS                Asia Pacific (New Zealand)                        $5.78                                      $0.46
    Azure                  East US 2 (Virginia)                          $8.13                                      $0.52
    Azure                West US 2 (Washington)                          $8.13                                      $0.52
    Azure              West Europe (Netherlands)                         $8.78                                      $0.56
    Azure           Australia East (New South Wales)                     $8.94                                      $0.57
    Azure               Canada Central (Toronto)                         $8.94                                      $0.57
    Azure              Southeast Asia (Singapore)                        $8.13                                      $0.52
    Azure                   Switzerland North                            $8.78                                      $0.56
    Azure                    US Gov Virginia                            $10.20                                      $0.52
    Azure                   Central US (Iowa)                            $8.13                                      $0.52
    Azure                 North Europe (Ireland)                         $8.13                                      $0.52
    Azure                  Japan East (Tokyo)                            $8.13                                      $0.52
    Azure                   UAE North (Dubai)                            $8.12                                      $0.64
    Azure               South Central US (Texas)                         $8.13                                      $0.52
    Azure                  Central India (Pune)                          $8.94                                      $0.57
    Azure                  UK South (London)                             $9.59                                      $0.61
    Azure         US Gov Virginia (Fed Ramp High Plus)                  $10.20                                      $0.52
    Azure                     Mexico Central                             $8.94                                      $0.58
    Azure                     Korea Central                              $8.13                                      $0.52
    Azure                    Sweden Central                              $8.12                                      $0.52
    Azure                   East US (Virginia)                           $8.13                                      $0.52
    GCP                    US Central 1 (Iowa)                           $5.00                                      $0.40
    GCP                   US East 4 (N. Virginia)                        $5.00                                      $0.40
    GCP                  Europe West 2 (London)                          $5.00                                      $0.40
    GCP                 Europe West 3 (Frankfurt)                        $5.00                                      $0.40
    GCP               Europe West 4 (Netherlands)                        $5.00                                      $0.40
    GCP             Middle East Central 2 (Dammam)                       $5.00                                      $0.40
    GCP             Australia Southeast 2 (Melbourne)                    $5.00                                      $0.40


Table 4(a): AWS Data Transfer Pricing

                                                    Non-SPCS and            SPCS and
                                                   Non-Postgres Data      Postgres Data        Data Transfer To        Data Transfer To
  Cloud                                            Transfer To Same     Transfer To Same         Same Cloud             Different Cloud
                  Data Transfer Source Region
 Provider                                           Cloud Provider,      Cloud Provider,      Provider, Different         Provider or
                                                   Same Region (per     Same Region (per       Region (per TB)         Internet (per TB)
                                                          TB)                  TB)
   AWS             US East (Northern Virginia)           $0.00                   $3.07              $20.00                  $90.00
   AWS                 US West (Oregon)                  $0.00                   $3.07              $20.00                  $90.00
   AWS                     EU Dublin                     $0.00                   $3.07              $20.00                  $90.00
   AWS                    EU Frankfurt                   $0.00                   $3.07              $20.00                  $90.00
   AWS                     AP Sydney                     $0.00                   $3.07             $140.00                  $140.00
   AWS                   AP Singapore                    $0.00                   $3.07              $90.00                  $120.00
   AWS                  Canada Central                   $0.00                   $3.07              $20.00                  $90.00
   AWS                  US East 2 (Ohio)                 $0.00                   $3.07              $20.00                  $90.00
   AWS              AP Northeast 1 (Tokyo)               $0.00                   $3.07              $90.00                  $114.00
   AWS                    AP Mumbai                      $0.00                   $3.07              $60.00                  $90.00
   AWS             US East 1 Commercial Gov              $0.00                   $3.07              $20.00                  $90.00


                                                                                                                                       12
Table 4(a): AWS Data Transfer Pricing

                                                     Non-SPCS and             SPCS and
                                                    Non-Postgres Data       Postgres Data           Data Transfer To     Data Transfer To
   Cloud                                            Transfer To Same      Transfer To Same            Same Cloud          Different Cloud
                  Data Transfer Source Region
 Provider                                            Cloud Provider,       Cloud Provider,         Provider, Different      Provider or
                                                    Same Region (per      Same Region (per          Region (per TB)      Internet (per TB)
                                                           TB)                   TB)
   AWS                  Europe (London)                   $0.00                  $3.07                   $20.00               $90.00
   AWS                 Asia Pacific (Seoul)               $0.00                  $3.07                   $80.00              $126.00
   AWS                   US Gov West 1                    $0.00                  $7.17                   $30.00              $155.00
                 US Gov West 1 (Fedramp High
   AWS                                                    $0.00                  $7.17                   $30.00              $155.00
                           Plus)
   AWS                 Europe (Stockholm)                 $0.00                  $3.07                   $20.00               $90.00
   AWS                 Asia Pacific (Osaka)               $0.00                  $3.07                   $90.00              $114.00
   AWS          South America East 1 (São Paulo)          $0.00                  $3.07                  $138.00              $150.00
   AWS                      EU (Paris)                    $0.00                  $3.07                   $20.00               $90.00
   AWS                Asia Pacific (Jakarta)              $0.00                  $3.07                  $100.00              $132.00
   AWS         US Gov East 1 (Fedramp High Plus)          $0.00                  $7.17                   $30.00              $155.00
   AWS                     EU (Zurich)                    $0.00                  $3.07                   $20.00               $90.00
   AWS                US Gov West 1 (DoD)                 $0.00                  $7.17                   $30.00              $155.00
                   US West (Commercial Gov -
   AWS                                                    $0.00                  $3.07                   $20.00               $90.00
                            Oregon)
   AWS                 Africa (Cape Town)                 $0.00                  $3.07                  $147.00              $154.00
   AWS               Asia Pacific (Malaysia)              $0.00                  $3.07                   $80.00              $108.00
   AWS               Asia Pacific (Thailand)              $0.00                  $3.07                   $80.00              $108.00
   AWS             Asia Pacific (New Zealand)             $0.00                  $3.07                   $98.00              $114.00


Table 4(b): Azure Data Transfer Pricing

                                                                                Destination Region (per TB)
   Cloud                                                                 Same Cloud Provider                                   Different
                 Data Transfer Source Region                        SPCS Data                                                   Cloud
 Provider                                                                                 Same               Different
                                                   Same Region    Transfer, Same                                              Provider or
                                                                                         Continent           Continent         Internet
                                                                      Region
   Azure              East US 2 (Virginia)            $0.00             $0.00             $20.00               $50.00           $87.50
   Azure           West US 2 (Washington)             $0.00             $0.00             $20.00               $50.00           $87.50
   Azure          West Europe (Netherlands)           $0.00             $0.00             $20.00               $50.00           $87.50
   Azure       Australia East (New South Wales)       $0.00             $0.00             $80.00               $80.00           $120.00
   Azure           Canada Central (Toronto)           $0.00             $0.00             $20.00               $50.00           $87.50
   Azure          Southeast Asia (Singapore)          $0.00             $0.00             $80.00               $80.00           $120.00
   Azure              Switzerland North               $0.00             $0.00             $20.00               $50.00           $87.50
   Azure               US Gov Virginia                $0.00             $0.00             $20.00               $50.00           $87.50
   Azure               US Central (Iowa)              $0.00             $0.00             $20.00               $50.00           $87.50
   Azure            North Europe (Ireland)            $0.00             $0.00             $20.00               $50.00           $87.50
   Azure              Japan East (Tokyo)              $0.00             $0.00             $80.00               $80.00           $120.00
   Azure              UAE North (Dubai)               $0.00             $0.00             $80.00               $80.00           $120.00
   Azure           South Central US (Texas)           $0.00             $0.00             $20.00               $50.00           $87.50
   Azure             Central India (Pune)             $0.00             $0.00             $40.00               $50.00           $87.50
   Azure              UK South (London)               $0.00             $0.00             $20.00               $50.00           $87.50
               US Gov Virginia (Fed Ramp High                           $0.00
   Azure                                              $0.00                               $20.00               $50.00           $87.50
                            Plus)
   Azure                Mexico Central                $0.00             $0.00             $20.00               $50.00           $87.00
   Azure                Korea Central                 $0.00             $0.00             $80.00               $80.00           $120.00
   Azure               Sweden Central                 $0.00             $0.00             $20.00               $50.00           $87.00
   Azure               East US (Virginia)             $0.00             $0.00             $20.00               $50.00           $87.00




                                                                                                                                            13
 Table 4(c): GCP Data Transfer Pricing

   Cloud        Data Transfer                                                    To Same Cloud Provider (TB)
  Provider         Source
                   Region          Same         SPCS Data         North        Europe     Asia     Indonesia     Middle     Oceania      Africa      South
                                   Region        Transfer,       America                                          East                              America
                                               Same Region
                US Central 1
    GCP                             $0            $6.22           $20           $50      $80        $100         $110        $100        $110        $140
                   (Iowa)
                US East 4 (N.
    GCP                             $0            $6.22           $20           $50      $80        $100         $110        $100        $110        $140
                  Virginia)
               Europe West 4
    GCP                             $0            $6.22           $50           $20      $80        $100         $110        $100        $110        $140
               (Netherlands)
               Europe West 2
    GCP                             $0            $6.22           $50           $20      $80        $100         $110        $100        $110        $140
                 (London)
               Europe West 3
    GCP                             $0            $6.22           $50           $20      $80        $100         $110        $100        $110        $140
                (Frankfurt)
                 Middle East
    GCP           Central 2         $0            $6.22           $110         $110      $110       $110          $80        $110        $110        $140
                 (Dammam)
                 Australia
    GCP         Southeast 2         $0            $6.22           $100         $100      $100           $80      $110         $80        $140        $140
                (Melbourne)


 Table 4(c): GCP Data Transfer Pricing cont’d

                                                                          To Different Cloud Provider or Internet (TB)
   Cloud        Data Transfer
                Source Region        North                                       Australia, Indonesia, Korea, South          Middle East9
  Provider                                         Europe         Asia8                                                                           China10
                                    America                                           America, Saudi Arabia                   and Africa
                 US Central 1
    GCP                              $120          $120          $120                            $190                           $150              $230
                    (Iowa)
                 US East 4 (N.
    GCP                              $120          $120          $120                            $190                           $150              $230
                   Virginia)
                Europe West 4
    GCP                              $120          $120          $120                            $190                           $150              $230
                (Netherlands)
                Europe West 2
    GCP                              $120          $120          $120                            $190                           $150              $230
                  (London)
                Europe West 3
    GCP                              $120          $120          $120                            $190                           $150              $230
                 (Frankfurt)
                 Middle East
    GCP           Central 2          $190          $190          $190                            $190                           $190              $230
                 (Dammam)
                  Australia
    GCP          Southeast 2         $190          $190          $190                            $190                           $190              $230
                 (Melbourne)


 Table 4(d): Pricing for Specific Endpoint Types

                Endpoint Type                                    Pricing per TB of data processed through the endpoint you configure

   AWS API Gateway, Private Endpoints                                                                   $10

Data processing for Outbound Privatelink is billed in tiers, based on the quantity of data processed each month. The first petabyte (PB) of data processed
will be billed at the price listed below. The next four PBs will be billed at the price listed below. Any subsequent data processed in excess of 5PB will be
billed at the price listed below.



 Table 4(e): Outbound Privatelink Pricing

                                                                                                                 Data Processed (per TB)
                                                                   Private Endpoint (per
  Cloud Provider                      Region                                                                                          Anything over 5
                                                                 endpoint per 1,000 hours)          First 1 PB         Next 4 PB
                                                                                                                                            PB
       AWS                 US East (Northern Virginia)                       $10.00                     $10.24           $6.14            $4.09
       AWS                     US West (Oregon)                              $10.00                     $10.24           $6.14            $4.09
       AWS                         EU Dublin                                 $11.00                     $10.24           $6.14            $4.09



8 Excluding Korea and Indonesia

9 Excluding Saudi Arabia

10 Excluding Hong Kong


                                                                                                                                                            14
 Table 4(e): Outbound Privatelink Pricing

                                                                                                             Data Processed (per TB)
                                                                    Private Endpoint (per
  Cloud Provider                      Region                                                                                      Anything over 5
                                                                  endpoint per 1,000 hours)     First 1 PB         Next 4 PB
                                                                                                                                        PB
       AWS                        EU Frankfurt                              $12.00               $10.24              $6.14            $4.09
       AWS                         AP Sydney                                $13.00               $10.24              $6.14            $4.09
       AWS                        AP Singapore                              $13.00               $10.24              $6.14            $4.09
       AWS                       Canada Central                             $11.00               $10.24              $6.14            $4.09
       AWS                      US East 2 (Ohio)                            $10.00               $10.24              $6.14            $4.09
       AWS                  AP Northeast 1 (Tokyo)                          $14.00               $10.24              $6.14            $4.09
       AWS                         AP Mumbai                                $10.00               $10.24              $6.14            $4.09
       AWS                US East 1 Commercial Gov                          $10.00               $10.24              $6.14            $4.09
       AWS                      Europe (London)                             $11.00               $10.24              $6.14            $4.09
       AWS                    Asia Pacific (Seoul)                          $13.00               $10.24              $6.14            $4.09
       AWS                       US Gov West 1                              $12.50               $10.24              $6.14            $4.09
                        US Gov West 1 (Fedramp High
       AWS                                                                  $12.50               $10.24              $6.14                $4.09
                                      Plus)
       AWS                   Europe (Stockholm)                             $10.50               $10.24              $6.14                $4.09
       AWS                   Asia Pacific (Osaka)                           $14.00               $10.24              $6.14                $4.09
       AWS             South America East 1 (São Paulo)                     $21.00               $10.24              $6.14                $4.09
       AWS                         EU (Paris)                               $11.00               $10.24              $6.14                $4.09
       AWS                   Asia Pacific (Jakarta)                         $13.00               $10.24              $6.14                $4.09
       AWS            US Gov East 1 (Fedramp High Plus)                     $12.50               $10.24              $6.14                $4.09
       AWS                         EU (Zurich)                              $13.20               $10.24              $6.14                $4.09
       AWS                   US Gov West 1 (DoD)                            $12.50               $10.24              $6.14                $4.09
                         US West (Commercial Gov -
       AWS                                                                  $10.00               $10.24              $6.14                $4.09
                                    Oregon)
       AWS                    Africa (Cape Town)                            $13.09               $10.24              $6.14                 $4.09
       AWS                  Asia Pacific (Malaysia)                         $11.70               $10.24              $6.14                 $4.09
       AWS                  Asia Pacific (Thailand)                         $11.70               $10.24              $6.14                 $4.09
       AWS                Asia Pacific (New Zealand)                        $13.65               $10.24              $6.14                 $4.09
       Azure                  East US 2 (Virginia)                          $10.00               $10.24              $6.14                 $4.09
       Azure               West US 2 (Washington)                           $10.00               $10.24              $6.14                 $4.09
       Azure              West Europe (Netherlands)                         $10.00               $10.24              $6.14                 $4.09
       Azure           Australia East (New South Wales)                     $10.00               $10.24              $6.14                 $4.09
       Azure               Canada Central (Toronto)                         $10.00               $10.24              $6.14                 $4.09
       Azure             Southeast Asia (Singapore)                         $10.00               $10.24              $6.14                 $4.09
       Azure                   Switzerland North                            $10.00               $10.24              $6.14                 $4.09
       Azure                    US Gov Virginia                             $13.00               $12.80             $12.80                $12.80
       Azure                   US Central (Iowa)                            $10.00               $10.24              $6.14                 $4.09
       Azure                North Europe (Ireland)                          $10.00               $10.24              $6.14                 $4.09
       Azure                  Japan East (Tokyo)                            $10.00               $10.24              $6.14                 $4.09
       Azure                   UAE North (Dubai)                            $10.00               $10.24              $6.14                 $4.09
       Azure               South Central US (Texas)                         $10.00               $10.24              $6.14                 $4.09
       Azure                 Central India (Pune)                           $10.00               $10.24              $6.14                 $4.09
       Azure                  UK South (London)                             $10.00               $10.24              $6.14                 $4.09
                       US Gov Virginia (Fed Ramp High
       Azure                                                                $13.00               $12.80             $12.80                $12.80
                                      Plus)
       Azure                     Mexico Central                             $10.00               $10.24              $6.14                 $4.09
       Azure                      Korea Central                             $10.00               $10.24              $6.14                 $4.09
       Azure                    Sweden Central                              $10.00               $10.24              $6.14                 $4.09
       Azure                   East US (Virginia)                           $10.00               $10.24              $6.14                 $4.09
       GCP                    US Central 1 (Iowa)                           $10.00               $30.72             $26.62                $24.57
       GCP                  US East 4 (N. Virginia)                         $10.00               $30.72             $26.62                $24.57
       GCP               Europe West 4 (Netherlands)                        $10.00               $30.72             $26.62                $24.57
       GCP                 Europe West 2 (London)                           $10.00               $30.72             $26.62                $24.57
       GCP                Europe West 3 (Frankfurt)                         $10.00               $30.72             $26.62                $24.57
       GCP             Middle East Central 2 (Dammam)                       $10.00               $30.72             $26.62                $24.57
       GCP            Australia Southeast 2 (Melbourne)                     $10.00               $30.72             $26.62                $24.57



 Table 5: Serverless Feature Table

                                                          Feature Multipliers
                Feature                   Snowflake-managed                                                      Unit Charges
                                                                        Cloud Services
                                              compute
 Archive Storage Retrieval File
                                                     -                          -                      0.05 Platform Credits per 1000 files
 Processing5
 Archive Storage Write5                              -                          -                      0.05 Platform Credits per 1000 files
 Automated Refresh and Data                        1.25                         -                     0.06 Platform Credits per 1000 files11



11 File charge doesn't apply to Iceberg tables, only to External Tables and Directory Tables.


                                                                                                                                                   15
 Table 5: Serverless Feature Table

                                                            Feature Multipliers
                Feature                     Snowflake-managed                                                             Unit Charges
                                                                            Cloud Services
                                                compute
 Registration
 Backup                                                2                           1                                             -
 Clustering Classic                                    2                           1                                             -
 Copy Files5                                           2                           -                                             -
 Data Quality Monitoring12                             2                           1                                             -
 Failsafe Recovery                                    0.9                          1                                             -
 Logging13                                           1.25                          -                        0.28 Platform Credits per 1000 file batches
 Materialized Views maintenance                        2                           1                                             -
 Open Catalog14                                        -                           -                        0.5 Platform Credits per 1 million requests
 Optima Clustering15                                   -                           -                      0.007 Platform Credits per uncompressed GB
 Query Acceleration                                    1                           -                                             -
 Replication Classic                                   2                          0.35                                           -
                                                                                                                5 Platform Credits per TB Replicated
 Replication - Optimized Refresh                       -                           -
                                                                                                     0.2 Platform Credits per 10K Changed Objects > 25M obj.
 Search Optimization Service                           2                           1                                             -
 Sensitive Data Classification                        0.9                          1                                             -
 Serverless Alerts                                    0.9                          1                                             -
 Serverless Tasks                                     0.9                          1                                             -
 Serverless Tasks Flex                                0.5                          1                                             -
 Snowflake App Runtime5                              0.27                          -                                             -
 Snowpipe                                              -                           -                             0.0037 Platform Credits per GB16
 Snowpipe Streaming17                                  -                           -                      0.0037 Platform Credits per uncompressed GB
 Snowpipe Streaming Classic18                          1                           -                     0.01 Platform Credits per client instance per hour
 Storage Lifecycle Policy
                                                     0.50                          1                                             -
 Execution5
 Table Optimization                                  0.75                          1                                             -
 Telemetry Data Ingest19                               -                           -                             0.0212 Platform Credits per GB
 Trust Center                                          1                           1                                             -



 Table 6(a): Snowflake AI Features Table, Cortex AI Functions

                                                                                  Snowflake-managed compute (AI Credits per one million Tokens)
                             Cortex Feature
                                                                                             Input                                      Output
 AI_COMPLETE – claude-4-sonnet                                                               1.80                                        9.00
 AI_COMPLETE -- claude-fable-55                                                              6.00                                        30.00
 AI_COMPLETE – claude-haiku-4-5                                                              0.60                                        3.00
 AI_COMPLETE – claude-opus-4-5                                                               3.00                                        15.00
 AI_COMPLETE – claude-opus-4-6                                                               3.00                                        15.00
 AI_COMPLETE – claude-opus-4-75                                                              3.00                                        15.00
 AI_COMPLETE – claude-opus-4-85                                                              3.00                                        15.00
 AI_COMPLETE – claude-opus-55                                                                3.00                                        15.00
 AI_COMPLETE – claude-sonnet-4-5                                                             1.80                                        9.00
 AI_COMPLETE – claude-sonnet-4-6                                                             1.80                                        9.00



12 The ROW_COUNT DMF (as described further in the Documentation) uses a Snowflake-managed compute multiplier of 0.9.

13 Charges for Logging will soon be replaced with charges for Telemetry Data Ingest.

14 This feature is available free of charge for a limited period of time.

15 As described in the Documentation, charges for Optima Clustering may be reduced based on data overlap.

16 Text-based files (e.g. CSV, XML, JSON) will be charged for their uncompressed size. Binary-based files (e.g., Parquet, Avro, ORC) will be charged for

their observed size regardless of compression.
17 Charges for Snowpipe Streaming only apply to the new high performance architecture.

18 Charges for Snowpipe Streaming Classic only apply to the classic architecture (ingest-java SDK versions 4.X or older).

19 Charges for Telemetry Data Ingest will soon replace charges for Logging.


                                                                                                                                                              16
 AI_COMPLETE – claude-sonnet-520                                                          1.20                                       6.00
 AI_COMPLETE – gemini-2-5-flash5                                                          0.18                                       1.50
 AI_COMPLETE – gemini-2-5-flash-lite5                                                     0.06                                       0.24
 AI_COMPLETE – gemini-3.1-flash-lite5                                                     0.15                                       0.90
 AI_COMPLETE – gemini-3.1-pro5                                                            1.20                                       7.20
 AI_COMPLETE – gemini-3.1-pro-long-context5                                               2.40                                      10.80
 AI_COMPLETE – gemini-3.5-flash5                                                          0.90                                       5.40
 AI_COMPLETE – gemma-4-26b-a4b                                                            0.078                                      0.24
 AI_COMPLETE – gemma-4-31b                                                                0.084                                      0.24
 AI_COMPLETE – gemma-4-e2b                                                                0.024                                     0.048
 AI_COMPLETE – grok-4.205                                                                 0.75                                       1.50
 AI_COMPLETE – grok-4.20-long-context5                                                    1.50                                       3.00
 AI_COMPLETE – grok-4.35                                                                  0.75                                       1.50
 AI_COMPLETE – grok-4.3-long-context5                                                     1.50                                       3.00
 AI_COMPLETE – llama3.1-70b                                                               0.432                                     0.432
 AI_COMPLETE – llama3.1-8b                                                                0.132                                     0.132
 AI_COMPLETE – llama3.3-70b                                                               0.432                                     0.432
 AI_COMPLETE – llama4-maverick                                                            0.144                                     0.582
 AI_COMPLETE – ministral-3-8b                                                             0.09                                       0.09
 AI_COMPLETE – mistral-large2                                                             1.20                                       3.60
 AI_COMPLETE – mistral-large35                                                            0.30                                       0.90
 AI_COMPLETE – mistral-7b                                                                 0.09                                       0.12
 AI_COMPLETE – mixtral-8x7b                                                               0.27                                       0.42
 AI_COMPLETE – openai-gpt-4.1                                                             1.20                                       4.80
 AI_COMPLETE – openai-gpt-55                                                              0.75                                       6.00
 AI_COMPLETE – openai-gpt-5-mini5                                                         0.15                                       1.20
 AI_COMPLETE – openai-gpt-5-nano5                                                         0.033                                      0.24
 AI_COMPLETE – openai-gpt-5.1                                                             0.75                                       6.00
 AI_COMPLETE – openai-gpt-5.2                                                             1.05                                       8.40
 AI_COMPLETE – openai-gpt-5.4                                                             1.50                                       9.00
 AI_COMPLETE – openai-gpt-5.4-long-context                                                3.00                                      13.50
 AI_COMPLETE – openai-gpt-5.4-mini5                                                       0.45                                       2.70
 AI_COMPLETE – openai-gpt-5.4-nano5                                                       0.12                                       0.75
 AI_COMPLETE – openai-gpt-5.55                                                            3.00                                      18.00
 AI_COMPLETE – openai-gpt-5.5-long-context5                                               6.00                                      27.00
 AI_COMPLETE – openai-gpt-5.6-luna5                                                       0.12                                       0.72
 AI_COMPLETE – openai-gpt-5.6-luna-long-context5                                          0.24                                       1.08
 AI_COMPLETE – openai-gpt-5.6-sol5                                                        3.00                                      18.00
 AI_COMPLETE – openai-gpt-5.6-terra5                                                      1.20                                       7.20
 AI_COMPLETE – openai-gpt-5.6-terra-long-context5                                         2.40                                      10.80
 AI_COMPLETE – pixtral-large                                                              1.20                                       3.60
 AI_COMPLETE – qwen3-32b5                                                                 0.09                                       0.36
 AI_COMPLETE – qwen3-next-80b-a3b5                                                        0.09                                       0.72
 AI_COMPLETE – qwen3-vl-235b-a22b5                                                        0.32                                       1.60
 AI_COMPLETE – twelvelabs-pegasus-1-2                                                    See “Snowflake AI Features Table, Other” below

                                                                              Snowflake-managed compute (AI Credits per one million Tokens)

 AI_AGG                                                                                                         1.85
 AI_CLASSIFY                                                                                                    1.62
 AI_EMBED – voyage-multimodal-3                                                                                 0.06
 AI_EMBED – multilingual-e5-large                                                                               0.05
 AI_EMBED – nv-embed-qa-4                                                                                       0.05
 AI_EMBED – snowflake-arctic-embed-l-v2.0                                                                       0.05
 AI_EMBED – voyage-multilingual-2                                                                               0.07
 AI_EMBED – e5-base-v2                                                                                          0.03


20 This feature is available under promotional pricing for a limited time. Prices will increase by 50% on September 1, 2026, or such other date as

Snowflake determines, as reflected in the Snowflake Service Consumption Table or otherwise communicated by Snowflake.
                                                                                                                                                     17
AI_EMBED – snowflake-arctic-embed-m                                                                 0.03
AI_EMBED – snowflake-arctic-embed-m-v1.5                                                            0.03
AI_EXTRACT – arctic-extract                                                                         5.55
AI_FILTER                                                                                           1.62
AI_GUARDRAILS                                                                                       0.35
AI_MULTI_EMBED – twelvelabs-marengo-embed-3-0                                   See “Snowflake AI Features Table, Other” below
AI_PARSE_DOCUMENT – Layout                                                      See “Snowflake AI Features Table, Other” below
AI_PARSE_DOCUMENT – OCR                                                         See “Snowflake AI Features Table, Other” below
AI_REDACT                                                                                           0.69
AI_SENTIMENT                                                                                        1.60
AI_SUMMARIZE5                                                                                       1.60
AI_SUMMARIZE_AGG                                                                                    1.85
AI_TRANSCRIBE                                                                                       0.97
AI_TRANSLATE                                                                                        1.63
Extract Answer                                                                                      0.11
Guard                                                                                               0.25
Sentiment                                                                                           0.09
Summarize                                                                                           0.13

                                                        Legacy Cortex Features

AI_COMPLETE – llama3-70b                                                                            1.21
AI_COMPLETE – llama3-8b                                                                             0.19
AI_EXTRACT – arctic-tilt-entity                                                                     9.50
AI_EXTRACT – arctic-tilt-table                                                                      28.40



Table 6(b): Snowflake AI Features Table, Cortex Inference with Prompt Caching

                                                       Snowflake-managed compute (AI Credits per one million Tokens)
                 Model
                                             Input                     Output                   Cache Write                Cache Read
claude-4-sonnet                               1.50                      7.50                        1.875                        0.15
claude-fable-55                               5.00                      25.00                        6.25                        0.50
claude-sonnet-4-5                             1.50                      7.50                        1.875                        0.15
claude-sonnet-4-6                             1.50                      7.50                        1.875                        0.15
claude-sonnet-520                             1.00                      5.00                         1.25                        0.10
claude-haiku-4-5                              0.50                      2.50                        0.625                        0.05
claude-opus-4-5                               2.50                      12.50                       3.125                        0.25
claude-opus-4-6                               2.50                      12.50                       3.125                        0.25
claude-opus-4-75                              2.50                      12.50                       3.125                        0.25
claude-opus-4-85                              2.50                      12.50                       3.125                        0.25
claude-opus-55                                2.50                      12.50                       3.125                        0.25
grok-4.205                                   0.625                      1.25                          -                           0.1
grok-4.20-long-context5                       1.25                       2.5                          -                           0.2
grok-4.35                                    0.625                      1.25                          -                           0.1
grok-4.3-long-context5                        1.25                       2.5                          -                           0.2
openai-gpt-4.1                                1.00                      4.00                          -                          0.25
openai-gpt-55                                0.625                      5.00                          -                          0.065
openai-gpt-5-mini5                           0.125                      1.00                          -                          0.015
openai-gpt-5-nano5                            0.03                      0.22                          -                          0.005
openai-gpt-5.1                               0.625                      5.00                          -                          0.065
openai-gpt-5.2                               0.875                      7.00                          -                          0.09
openai-gpt-5.4                                1.25                      7.50                          -                          0.125
openai-gpt-5.4-long-context                   2.50                      11.25                         -                          0.25
openai-gpt-5.4-mini5                         0.375                      2.25                          -                          0.038
openai-gpt-5.4-nano5                          0.10                      0.625                         -                          0.01
openai-gpt-5.55                               2.50                      15.00                         -                          0.25
openai-gpt-5.5-long-context5                  5.00                      22.50                         -                          0.50
openai-gpt-5.6-luna5                          0.10                      0.60                        0.125                        0.01


                                                                                                                                         18
Table 6(b): Snowflake AI Features Table, Cortex Inference with Prompt Caching

                                                        Snowflake-managed compute (AI Credits per one million Tokens)
                 Model
                                              Input                    Output                Cache Write                Cache Read
openai-gpt-5.6-luna-long-context5              0.20                     0.90                     0.25                      0.02
openai-gpt-5.6-sol5                            2.50                     15.00                         -                    0.25
openai-gpt-5.6-terra5                          1.00                     6.00                     1.25                      0.10
openai-gpt-5.6-terra-long-context5             2.00                     9.00                     2.50                      0.20



Table 6(c): Snowflake AI Features Table, Cortex Inference

                                                               Snowflake-managed compute (AI Credits per one million Tokens)
                         Model
                                                                     Input                                     Output
llama3.1-70b                                                          0.36                                      0.36
llama3.1-8b                                                           0.11                                      0.11
llama3.2-1b                                                           0.05                                      0.05
llama3.2-3b                                                          0.075                                      0.075
llama3.3-70b                                                          0.36                                      0.36
llama4-maverick                                                       0.12                                      0.485
mistral-large2                                                        1.00                                      3.00
mistral-7b                                                           0.075                                      0.10



Table 6(d): Snowflake AI Features Table, Snowflake CoWork, Cortex Agents, & Cortex Analyst via Snowflake CoWork or Cortex Agents

                                                        Snowflake-managed compute (AI Credits per one million Tokens)
                 Model
                                              Input                   Output                 Cache Write                Cache Read
claude-fable-55                                6.50                    32.50                    8.13                       0.65
claude-haiku-4-5                               0.65                     3.25                    0.81                       0.06
claude-opus-4-5                                3.25                    16.26                    4.07                       0.33
claude-opus-4-6                                3.25                    16.26                    4.07                       0.33
claude-opus-4-75                               3.25                    16.26                    4.07                       0.33
claude-opus-4-85                               3.25                    16.26                    4.07                       0.33
claude-opus-55                                 3.25                    16.26                    4.07                       0.33
claude-sonnet-4-5                              1.95                     9.76                    2.44                       0.20
claude-sonnet-4-6                              1.95                     9.76                    2.44                       0.20
claude-sonnet-520                              1.30                     6.50                    1.625                      0.13
gemini-2-5-flash5                              0.18                     1.48                      -                        0.02
gemini-3.1-flash-lite5                        0.163                    0.975                      -                       0.017
gemini-3.1-pro5                                1.30                     7.81                      -                        0.13
gemini-3.5-flash5                              0.98                     5.85                      -                        0.10
openai-gpt-4.1                                 1.30                     5.20                      -                        0.33
openai-gpt-55                                  0.81                     6.51                      -                        0.08
openai-gpt-5-mini5                             0.16                     1.30                      -                        0.02
openai-gpt-5.1                                 0.81                     6.51                      -                        0.08
openai-gpt-5.2                                 1.14                     9.11                      -                        0.11
openai-gpt-5.4                                 1.63                     9.76                      -                        0.16
openai-gpt-5.55                                3.25                    19.50                      -                        0.33
openai-gpt-5.5-long-context5                   6.50                    29.25                      -                        0.65
openai-gpt-5.6-luna5                           0.13                     0.78                    0.163                     0.013
openai-gpt-5.6-luna-long-context5              0.26                     1.17                    0.325                     0.026
openai-gpt-5.6-sol5                            3.25                    19.50                      -                       0.325
openai-gpt-5.6-terra5                          1.30                     7.80                    1.625                      0.13
openai-gpt-5.6-terra-long-context5             2.60                    11.70                    3.25                       0.26




                                                                                                                                     19
 Table 6(e): Snowflake AI Features Table, Snowflake CoCo

                                                                Snowflake-managed compute (AI Credits per one million Tokens)
                 Model
                                                     Input                      Output                    Cache Write                   Cache Read
 claude-fable-55                                     5.50                        27.50                         6.88                        0.55
 claude-haiku-4-5                                    0.55                         2.75                        0.688                        0.055
 claude-opus-4-5                                     2.75                        13.75                         3.44                        0.28
 claude-opus-4-6                                     2.75                        13.75                         3.44                        0.28
 claude-opus-4-75                                    2.75                        13.75                         3.44                        0.28
 claude-opus-4-85                                    2.75                        13.75                         3.44                        0.28
 claude-opus-55                                      2.75                        13.75                         3.44                        0.28
 claude-sonnet-4-5                                   1.65                         8.25                         2.07                        0.17
 claude-sonnet-4-6                                   1.65                         8.25                         2.07                        0.17
 claude-sonnet-520                                   1.10                         5.50                        1.375                        0.114
 gemini-3.1-flash-lite5                              0.138                       0.825                          -                          0.014
 openai-gpt-5.2                                      0.97                         7.70                          -                          0.10
 openai-gpt-5.4                                      1.38                         8.25                          -                          0.14
 openai-gpt-5.55                                     2.75                        16.50                          -                          0.28
 openai-gpt-5.5-long-context5                        5.50                        24.75                          -                          0.55
 openai-gpt-5.6-luna5                                0.11                         0.66                        0.138                        0.011
 openai-gpt-5.6-luna-long-context5                   0.22                         0.99                        0.275                        0.022
 openai-gpt-5.6-sol5                                 2.75                        16.50                          -                          0.275
 openai-gpt-5.6-terra5                               1.10                         6.60                        1.375                        0.11
 openai-gpt-5.6-terra-long-context5                  2.20                         9.90                         2.75                        0.22



 Table 6(f): Snowflake AI Features Table, Fine-tuning5

                                                                                          Snowflake-managed compute
                          Feature                                             Training                               Cortex Complete (Inference)
                                                             (Platform Credits per one million Tokens)           (AI Credits per one million Tokens)
 AI EXTRACT – arctic-extract – finetuned                                         0                                              9.140
 Cortex Fine-tuning – llama3.1-70b                                              3.40                                             2.42
 Cortex Fine-tuning – llama3.1-8b                                               0.64                                             0.38
 Cortex Fine-tuning – mistral-7b                                                0.64                                             0.24
 Cortex Fine-tuning – mixtral-8x7b                                              3.40                                             0.44
                                                                     Legacy Features
 Cortex Fine-tuning – llama3-70b                                                3.40                                             2.42
 Cortex Fine-tuning – llama3-8b                                                 0.64                                             0.38



 Table 6(g): Snowflake AI Features Table, Other

                               Feature                                                           Snowflake-managed compute
                                                                         0.294 AI Credits per 1,000 video seconds
 AI_COMPLETE – twelvelabs-pegasus-1-2                                    4.5 AI Credits per one million output tokens
                                                                         0.420 AI Credits per 1,000 video seconds
                                                                         0.084 AI Credits per 1,000 audio seconds
                                                                         0.060 AI Credits per 1,000 image requests
 AI_MULTI_EMBED – twelvelabs-marengo-embed-3-0                           0.042 AI Credits per 1,000 text requests
 AI_PARSE_DOCUMENT – Layout                                              3.66 AI Credits per 1,000 pages
 AI_PARSE_DOCUMENT– OCR                                                  0.68 AI Credits per 1,000 pages
 Batch Cortex Search5                                                    0.12 AI Credits per GB/hr of indexed data
 Cortex Analyst                                                          67 Platform Credits per 1,000 messages21
 Cortex Search                                                           6.3 AI Credits per GB/mo of indexed data

A Provisioned Throughput reservation allows you to reserve continuous access to certain Snowflake AI Features for a specified, fixed term (the “Provisioned
Throughput”), subject to Snowflake’s approval. Each Provisioned Throughput must specify: (i) the reserved Snowflake AI Feature; (ii) the term; (iii) and the
number of requested provisioned throughput units (“PTUs”), as described further in the Documentation. You will consume Platform Credits for the
Provisioned Throughput throughout the term regardless of your actual usage of the reserved Snowflake AI Feature. Provisioned Throughput is non-
cancellable, non-transferable, non-resellable, non-exchangeable, non-modifiable, non-refundable and non-renewable. Provisioned Throughput does not


21 This pricing will only be applicable when using the Cortex Analyst API.



                                                                                                                                                         20
renew automatically.


     Table 6(h): Snowflake AI Features Table, Provisioned Throughput

          Cloud Provider                 Snowflake-managed compute (Platform Credits per PTU per hour)22                          Term Length (months)
     AWS                                                                    0.08                                                              1
     Azure                                                                  0.10                                                              1



     Table 7: Openflow Connector for Oracle

                                                                                                    Unit Price per Licensed Core23
                                    Fee Type
                                                                                       36-Month Commitment24             12-Month Commitment25
     License                                                                               $70 per month                         $2,520
     Support & Maintenance                                                                 $40 per month                      $600 per year


     Table 8: Organization Usage

                       Records Powering Views (per Month)                                                Platform Credits per month
     <1 million                                                                                                        0
     1 million £ Records < 10 million                                                                                  2
     10 million £ Records 50 million                                                                                  11
     50 million £ Records < 250 million                                                                               50
     250 million £ Records < 500 million                                                                             115
     500 million £ Records <1 billion                                                                                230
     1 billion £ Records < 2.5 billion                                                                               290
     2.5 billion £ Records < 5 billion                                                                               575
     5 billion £ Records < 10 billion                                                                               1150
     10 billion £ Records < 20 billion                                                                              2300
     20 billion £ Records                                                                                           3500


Additional Accounts. You may request additional Accounts through the Snowflake Service or by filing a support ticket. Notwithstanding the foregoing, you
may be required to sign an Order Form to add new VPS accounts.

Non-U.S. Order Forms. Customer is responsible for all Taxes associated with its purchases or use of, or access to, the Snowflake offerings. If Snowflake
has the legal obligation to pay or collect such Taxes, Snowflake will invoice Customer and Customer will pay that amount unless Customer provides
Snowflake with a valid tax exemption certificate authorized by the appropriate taxing authority. Taxes will not be deducted from payments to Snowflake,
except as required by applicable law, in which case Customer will increase the amount payable as necessary so that, after making all required deductions
and withholdings, Snowflake receives and retains (free from any liability for Taxes) an amount equal to the amount it would have received had no such
deductions or withholdings been made. Upon Snowflake’s request, Customer will provide to Snowflake its proof of withholding tax remittance to the
respective tax authority. Where applicable, Customer will provide its VAT/GST Registration Number(s) on the Order Form to confirm the business use of
the purchased services. As used here, "Taxes" means taxes, levies, duties or similar governmental assessments of any nature, including, for example, any
sales, use, GST, value-added, withholding, or similar taxes, whether domestic or foreign, or assessed by any jurisdiction, but excluding any taxes based on
net income, property, or employees of Snowflake.

Currency Conversions. Customer’s consumption pursuant to this Snowflake Service Consumption Table will be measured in U.S. Dollars (“USD”). If an
Order Form requires payment in a currency other than USD (“Foreign Currency”) and specifies the Capacity Platform Credit Price, AI Credit Price, and the
Capacity Storage Price in USD, Customer’s invoice(s) will be payable to Snowflake in such Foreign Currency pursuant to the payment obligations set forth
in the Order Form and further subject to the following:
             1.         If Customer is in On Demand:
                        a.        For each invoice, Customer’s consumption for the invoiced period will be converted from USD to the relevant Foreign
                        Currency at the Spot Rate26 applicable for the last day of the invoiced period.
                        b.         Customer will receive a monthly statement reflecting its consumption for the immediately preceding month in USD.
             2.         If Customer has available Capacity:
                        a.         Customer’s total Capacity commitment specified in the Order Form will be converted from Foreign Currency to USD at the




22    Each Provisioned Throughput reservation is subject to minimum PTU quantities and incremental quantities, as described in the Documentation.
23 A “Licensed Core” is a single processing core in a CPU, multiplied by the Licensing Factor, where such processing core is: (i) running an Oracle

database; and (ii) connected to Openflow Connector for Oracle. The “Licensing Factor” means the then-applicable “Core Processor Licensing Factor,” as
described in the “Oracle Processor Core Factor Table” made available at https://www.oracle.com/contracts/docs/processor-core-factor-table-070634.pdf
(or successor URL as Oracle America, Inc. may designate), as may be updated from time to time.
24 Subject to the terms of the Openflow Connector for Oracle Addendum, (i) license and S&M fees for the initial 36-month term are billed monthly, and (ii)

after month 36, the license fee is paid in full and S&M fees auto-renew in 12-month increments, billed monthly.
25 Subject to the terms of the Openflow Connector for Oracle Addendum, (i) license fee is billed upfront, and (ii) S&M fee is billed upfront at the start of the

12-month commitment, which auto-renews in 12-month increments.
26 The “Spot Rate” means the real-time currency conversion spot rate existing in the United States as reported by Oanda.com (or such other reputable

foreign exchange rate platform Snowflake may use in its sole discretion).

                                                                                                                                                             21
                     Spot Rate applicable for the Processing Date27 under such Order Form (“USD Total Capacity Balance”).
                     b.        In the event Customer has eligible Capacity Rollover in EUR from a Prior Order Form, any such Capacity Rollover will be
                     converted to USD at the Spot Rate applicable for the Processing Date of the renewal Order Form and such amount shall be added to
                     the USD Total Capacity Balance.
                     c.        In the event of an Additional Capacity Order to an existing Capacity Order Form (“Underlying Order Form”), Customer’s
                     total Capacity commitment specified in such Additional Capacity Order will be converted from Foreign Currency to USD at the Spot
                     Rate then-applicable for the Underlying Order Form and will be added to the aggregate USD Total Capacity Balance covering both
                     the Underlying Order Form and the Additional Capacity Order.
                     d.         Each invoice issued to Customer under the Order Form constitutes a “Billing Event” and the following shall apply with
                     respect to each Billing Event:
                                i.        Customer’s Capacity payment obligation amount in connection with each Billing Event will be converted from
                                Foreign Currency to USD at the Spot Rate applicable for the invoice date of each Billing Event; and
                                ii.        At the time of each Billing Event, the USD Total Capacity Balance will be adjusted up or down based on the Spot
                                Rate applicable for the invoice date of each such Billing Event (the “FX Adjustment”) to establish an updated USD Total
                                Capacity Balance. For clarity, Customer’s actual consumption percentage against the USD Total Capacity Balance will
                                move up or down as a result of each FX Adjustment.
                     e.         Customer’s consumption will be drawn against the USD Total Capacity Balance (subject to any FX Adjustment, as
                     applicable).
                     f.         Customer will receive a monthly statement reflecting its USD Total Capacity Balance (including any Free Usage), any
                     applicable FX Adjustment and Customer’s consumption for the immediately preceding month in USD.
          3.        For Free Usage. In the event of any Free Usage, Customer’s Free Usage will be converted at the Spot Rate applicable for the
          Processing Date.
For the avoidance of doubt, the above terms apply whether Capacity is purchased directly from Snowflake or through a Snowflake-authorized reseller. For
Order Forms (including Underlying Order Forms) placed through resellers, the date of the “Billing Event,” “Processing Date” and/or “FX Adjustment” will be
determined based on the date that the reseller is billed or places the corresponding order, as applicable.
Changes to this Snowflake Service Consumption Table. This Snowflake Service Consumption Table may be updated from time to time. Changes shall
be effective on the date that Snowflake announces they are effective. This Snowflake Service Consumption Table applies to Previews, provided that
Previews may be subject to different pricing, as may be set forth in the applicable Documentation.
Any capitalized terms used but not defined herein shall have the meaning set forth in the Agreement or the Documentation, as applicable.




27 The “Processing Date” means the later of: (a) the Subscription Term Start Date or the (b) the date the Order Form is processed by Snowflake.

Notwithstanding the foregoing, any Additional Capacity Order will have the same Processing Date as the Underlying Order Form.

                                                                                                                                                       22
