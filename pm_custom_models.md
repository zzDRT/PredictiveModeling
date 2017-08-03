---

copyright:
  years: 2016, 2017
lastupdated: "2017-08-03"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Development and persistence of the custom model

## Working with custom models

### Scenario name: Product line prediction.

### Scenario description:

Our client is running one of the most
famous chain stores in Europe. They would like us to figure out
their clients' interests in terms of their product line such as
Personal Accessories, Camping Equipment, and Outdoor Protection.
A Data Scientist develops a predictive model and shares it with
you (the developer). Your task is to deploy the model and
generate predictive analytics by making score requests against
the deployed model.

### Development and persistence of the custom model

Use Data Science Experience to create custom models. After signing up, sign in and complete the following steps.

1. Create an organization and a space. The first time you sign in, you'll be asked for it, 
   so click **Continue** and accept the default values.

2. After the organization is created, go to **My Projects** and click
   **create project**.

3. Specify a name and description for your project and click
   **Create**. The project name you specified will also be used as
   your Target Container's name.

4. After the project is created, you can either:
   *  **add notebooks** and start developing your own models, or you can upload one of the
   following sample notebooks:

    *  [Developing Spark MLlib models with Python](https://apsportal.ibm.com/analytics/notebooks/89492fd6-a641-4819-9176-3d9381561df9/view?access_token=d80bef1a172d1d83d3721b101886337158457281774186f181a2e6a5b57f5ec7)

    *  [Developing Spark MLlib models with Scala](https://apsportal.ibm.com/analytics/notebooks/c8652d2c-bfc9-4354-8168-f1c9f7f8dfc2/view?access_token=02a83fea8450a452c8de76af98dae078459d0f56810ddef4f4c62d5bc4fc72cf)

    *  [Developing scikit-learn models with Python](https://apsportal.ibm.com/analytics/notebooks/5215a61a-16d7-4fa2-b060-e3e243ceebe3/view?access_token=70f48c95c5571a614ce97484d3f168b1d9b6aeebce015187d3d77ce6038f025e)

   * **add models** and start developing your own models using model wizard



### Deployment and scoring of the custom model

See the following sections for instructions regarding deploying
and scoring models, or see the scoring section of the notebooks
linked to previously.

*  [Deploying online models](pm_service_api_spark_online.html)

*  [Deploying batch models](pm_service_api_spark_batch.html)

*  [Deploying streaming models](pm_service_api_spark_streaming.html)
