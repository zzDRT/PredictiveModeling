---

copyright:
  years: 2016, 2017
lastupdated: "2017-07-30"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Service API


The Machine Learning service is a set of [REST APIs](https://watson-ml-api.mybluemix.net/) called from
any programming language, permitting integration of Data Science
Experience developed analytics in your applications. Bind your
Bluemix applications to the Machine Learning service instance and
generate the predictive analytics that your applications need to
deliver higher value to your users. Manage models in the
[administration dashboard](pm_service_ui_spark.html), and use the dashboard to create online,
batch, or streaming deployments integrated with your
applications.

Develop applications against the Data Science Experience files
(Spark and Python models) that are deployed on a service instance
through the powerful [REST APIs](https://watson-ml-api.mybluemix.net/):

*  Retrieve the metadata for a given predictive model
*  Deploy models and manage deployed models
    *  Online deployment (scoring)
    *  Batch deployment (supporting IBM Object Storage and DashDB)
    *  Streaming deployment (supporting IBM MessageHub)
*  Retrieve the metadata for a given deployment
*  Generate predictive analytics by making score requests against
   deployed models

You can easily explore the Machine Learning service capability
via the available [Swagger representation of the REST API](https://watson-ml-api.mybluemix.net/).

See the following sections for REST API examples of:

*  [Online deployment and scoring](pm_service_api_spark_online.html)
*  [Batch deployment with Object Storage](pm_service_api_spark_batch.html)
*  [Streaming deployment with MessageHub](pm_service_api_spark_streaming.html)
