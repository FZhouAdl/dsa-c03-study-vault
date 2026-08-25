---
title: "Cross-region inference | Snowflake Documentation"
source: https://docs.snowflake.com/user-guide/snowflake-cortex/cross-region-inference
cert_domain: domain-3-model-development
crawl_depth: 1
crawled: 2026-08-23
---

# Cross-region inference¶

Accessing frontier AI models and the full suite of Snowflake Cortex AI products requires cross-region inference. You choose the boundaries: route to any region for maximum access, or restrict to specific cloud providers and geographies.

Snowflake Cortex AI supports cross-region inference across AWS, Azure, and GCP regions. You control the routing behavior through a single account-level parameter, [CORTEX_ENABLED_CROSS_REGION](/sql-reference/parameters#label-cortex-enable-cross-region), choosing the option that best fits your performance, compliance, and data residency requirements.

## How it works¶

When cross-region inference is enabled, Snowflake automatically routes inference requests to available capacity within the bounds that you set.

Your customer data remains stored only in the region where your account is located. During cross-region inference, the inference payload—the input prompt you send and the output response you receive—is transmitted transiently to the processing region for the duration of processing. Your customer data is not persisted in the processing region.

## Choosing a routing option¶

Snowflake provides four routing options, from most flexible to most restrictive. Each option is a valid production configuration—choose the one that matches your requirements.

Type| Parameter value| Routing behavior| Best for  
---|---|---|---  
Global| `ANY_REGION`| Requests can be processed in any Snowflake-supported region, across any cloud provider.| Broadest model selection, highest throughput, maximum resilience, lowest cost.  
Cloud-specific| `AWS_GLOBAL`, `AZURE_GLOBAL`, `GCP_GLOBAL`, or comma-separated combinations| Requests stay within a designated cloud provider (for example, AWS or Azure).| Organizations that require data to remain within a specific cloud provider’s network while getting the lowest cost, higher throughput, and added resiliency.  
Regional| `AWS_US`, `AWS_EU`, `AZURE_US`, `AZURE_EU`, or comma-separated combinations| Requests stay within designated cloud provider regions (for example, AWS US or Azure EU).| Organizations that require data to remain within a specific cloud provider’s network and geography.  
Disabled| `DISABLED`| Requests are processed only in your account’s home region.| Strict data residency or geographic sovereignty requirements. Use cases not needing frontier AI models.  
  
### Any region¶

Setting the parameter to `ANY_REGION` provides access to the full set of supported models and Cortex products, with the highest available capacity. Snowflake routes requests to the optimal region automatically.
[code] 
    ALTER ACCOUNT SET CORTEX_ENABLED_CROSS_REGION = 'ANY_REGION';
    
[/code]

For new accounts created in new organizations within commercial regions after March 9, 2026, `ANY_REGION` is the default.

### Specify cloud regions¶

For organizations that need inference data to remain within a particular cloud provider’s network, you can specify one or more cloud regions. Requests are routed only to regions within the designated boundaries.
[code] 
    ALTER ACCOUNT SET CORTEX_ENABLED_CROSS_REGION = 'AWS_GLOBAL';
    
[/code]

For a full list of supported regions, see the [CORTEX_ENABLED_CROSS_REGION](/sql-reference/parameters#label-cortex-enable-cross-region) parameter reference.

### Account region only¶

To restrict inference processing to your account’s home region, set the parameter to `DISABLED`. This provides the strictest data residency posture but limits the models and features available to those deployed in your region.
[code] 
    ALTER ACCOUNT SET CORTEX_ENABLED_CROSS_REGION = 'DISABLED';
    
[/code]

## Data security and residency¶

Cross-region inference is designed with enterprise data security as a foundational requirement.

Data in transit is always encrypted. How your inference data travels depends on the regions involved:

  * **Within the same cloud provider** — Data stays entirely within the provider’s private backbone network and never traverses the public internet.
  * **Across cloud providers** — Data traverses the public internet using Mutual Transport Layer Security (mTLS), providing encrypted, mutually authenticated connections between Snowflake endpoints.



**No customer data is stored at the processing region.** The processing region handles the request and returns the result; no customer data is persisted.

**Billing is based on your account region.** Credits are consumed in your requesting region, regardless of where the request is processed. You do not incur data egress charges for cross-region inference.

## US Commercial Gov regions¶

Cross-region inference for Snowflake’s government-authorized, FIPS-compliant commercial environments is designed to maintain data-handling boundaries while providing access to supported AI models. When enabled, inference requests remain within the same cloud and compliance boundary, and processing occurs on FIPS-validated infrastructure such as AWS Bedrock FIPS endpoints. This approach allows customers in select U.S. government-authorized regions to use Snowflake AI capabilities securely and to meet your compliance requirements.

To enable this feature, set the `CORTEX_ENABLED_CROSS_REGION` parameter to `AWS_US` for workloads in a supported government-authorized region:
[code] 
    ALTER ACCOUNT SET CORTEX_ENABLED_CROSS_REGION = 'AWS_US';
    
[/code]

Cross-region inference is available for US Commercial Gov in these regions:

  * US East (Commercial Gov - N. Virginia)
  * US West (Commercial Gov - Oregon)



## US FedRAMP High and DoD gov regions¶

Cross-region inference for Snowflake’s government-authorized, FIPS-compliant environments is designed to maintain data-handling boundaries while providing access to supported AI models. When enabled, inference requests remain within the same compliance boundary, and processing occurs on FIPS-validated infrastructure such as AWS Bedrock FIPS endpoints. This approach allows customers in select U.S. government-authorized regions to use Snowflake AI capabilities securely and to meet your compliance requirements.

Cross-region inference is available in the following regions:

  * US Gov East 1 (FedRAMP High Plus) on AWS
  * US Gov West 1 (FedRAMP High Plus) on AWS
  * US Gov Virginia (FedRAMP High Plus) on Azure
  * US Gov West 1 (DoD) on AWS



To enable this feature, set the `CORTEX_ENABLED_CROSS_REGION` parameter to `AWS_US` for workloads in a supported government-authorized region:
[code] 
    ALTER ACCOUNT SET CORTEX_ENABLED_CROSS_REGION = 'AWS_US';
    
[/code]

If you set the parameter to `ANY_REGION`, requests from these regions are processed within the United States.

DoD traffic is isolated in a separate environment. FedRAMP High Plus and DoD regions don’t route inference requests to each other.

Important

When inference is routed from Azure to AWS, data traverses the public internet using Mutual Transport Layer Security (mTLS) 1.3. Data in transit is always encrypted.

## Access control requirements¶

The `CORTEX_ENABLED_CROSS_REGION` parameter can only be set at the account level, not at the user or session levels. Only the ACCOUNTADMIN role can modify this parameter using the [ALTER ACCOUNT](/sql-reference/sql/alter-account) command. This parameter cannot be set by the ORGADMIN role.
[code] 
    ALTER ACCOUNT SET CORTEX_ENABLED_CROSS_REGION = 'ANY_REGION';
    
[/code]

## Cost considerations¶

You are charged credits for the use of LLM as listed in the [Snowflake Service Consumption Table](https://www.snowflake.com/legal-files/CreditConsumptionTable.pdf). Credits are considered consumed in the requesting region. For example, if you call an LLM Function from the `us-east-2` region and the request is processed in the `us-west-2` region, the credits are considered consumed in the `us-east-2` region.

You do not incur data egress charges for using cross-region inference.

## Additional considerations¶

  * Latency between regions depends on the cloud provider infrastructure and network status. Snowflake recommends that you test your specific use case with cross-region inference enabled.
  * Cross-region inference for [Cortex Search](/user-guide/snowflake-cortex/cortex-search/cortex-search-overview) is not supported in [all regions](/user-guide/snowflake-cortex/cortex-search/cortex-search-overview#label-cortex-search-overview-regional-availability).



## Next steps¶

  * For details on the cross-region inference parameter, see the [CORTEX_ENABLED_CROSS_REGION](/sql-reference/parameters#label-cortex-enable-cross-region) section of the SQL parameter reference.
