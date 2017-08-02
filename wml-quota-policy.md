---

copyright:
  years: 2016, 2017
lastupdated: "2017-08-02"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Quota Policy
The IBM Watson Machine Learning Engine enforces quotas on a per-instance basis to ensure appropriate resource allocation and usage. These policies vary according to account profiles, historical service usage, availability of resource, and are subject to change without notice.The following service quota limits are currently active.## Resource Usage
Resources are limited to the following maxiumum values:
-  **Max published models**: 200 (Free plan) 1000 (Standard & Professional plans)-  **Max Spark ML model size**: 200 MB-  **Max deployed models**: 5 (Free plan limit) 1000 (Standard & Professional plans)## Service Request Limits
You are limited in the number of request that you can make per minute to specific APIs.  If your application needs higher limits, you must [contact IBM Bluemix Support](https://support.ng.bluemix.net/) to increase your quota.## Deployment requests
The following limits apply to deployment API requests:
-  **Period**: 60 seconds-  **Default limit**: 10## Online prediction requestsThe following limits apply to [online predictions](pm_service_api_spark_building.html) requests:

-  **Period**: 60 seconds-  **Default limit**: 500## Resource management requests
The following limits apply to the combined total requests in this list:
[Token](https://watson-ml-api.mybluemix.net/#/Token): post, get, delete, put, patch requests
[Deployments](https://watson-ml-api.mybluemix.net/#/Deployments): post, get, delete, put, patch requests-  **Period**: 60 seconds-  **Default limit**: 100