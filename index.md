---

copyright:
  years: 2016, 2017
lastupdated: "2017-08-02"

---

<!-- Common attributes used in the template are defined as follows: -->
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Getting started with Watson Machine Learning
{: #WMLgettingstarted}

Use IBM® Watson™ Machine Learning to integrate predictive analytics with your applications. Data scientists use machine learning to develop predictive models, whereas developers use machine learning to create applications that make smarter decisions, solve tough problems, and improve user outcomes.
{: shortdesc}

## Prerequisites

To use Watson Machine Learning, from the Bluemix catalog, you must create the [service instance here](https://console.bluemix.net/catalog/services/ibm-watson-machine-learning/).

## Steps

1. [Create and store a model](pm_custom_models.html).
2. [Deploy a  model](pm_service_api_spark_online.html).
3. Use the deployed model ```scoring endpoint``` in your application to [get predictions.](pm_service_api_spark_building.html)

## About

The Watson Machine Learning service is a set of REST APIs that can be
called from any programming language.

The focus of the Watson Machine Learning service is deployment, but note
that IBM SPSS Modeler or Data Science Experience is required for
authoring and working with models and pipelines. Both SPSS
Modeler and Data Science Experience (leveraging Spark MLlib and Python scikit-learn) offer various modeling methods that are taken from machine
learning, artificial intelligence, and statistics.

## Related Links

Ready to get started? To create an instance of a service or bind
an application, see [Using the service with Spark and Python models](using_pm_service_dsx.html) or
[Using the service with SPSS models](using_pm_service.html).

If you are interested in exploring the API, see [Service API for Spark and Python models](pm_service_api_spark.html) or [Service
API for SPSS models](pm_service_api_spss.html).

For details about SPSS Modeler and the modeling algorithms it
provides, see [IBM Knowledge Center](https://www.ibm.com/support/knowledgecenter/SS3RA7).

For details about IBM Data Science Experience and the modeling
algorithms it provides, see [https://datascience.ibm.com](https://datascience.ibm.com).
