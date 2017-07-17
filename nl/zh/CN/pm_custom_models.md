---

copyright:
  years: 2016, 2017
lastupdated: "2017-06-21"

---
<!-- Copyright info and last updated date at top of file: REQUIRED
    The copyright and lastupdated info is YAML content that must occur at the top of the MD file, before attributes are listed.
    It must be --- surrounded by 3 dashes ---
    The value "years" can contain just one year or a two years separated by a comma. (years: 2014, 2016)
    The value "lastupdated" must be followed by a machine date in quotes in the following format: "YYYY-MM-DD"
    The value for "years" must be indented 2 spaces under "copyright", followed by "lastupdated" which should start on its own non-indented line.

-->

<!-- Common attributes used in the template are defined as follows: -->
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# 开发和持久存储定制模型

## 使用定制模型

### 场景名称：产品系列预测。

### 场景描述：

客户在欧洲运营着最著名的连锁店之一。他们希望我们弄清楚顾客对其产品系列（例如，“个人辅助用具”、“露营装备”和“户外防护用品”）的兴趣。为此，数据研究员开发了一个预测模型并将其与您（开发者）共享。您的任务是部署模型，并通过对已部署的模型发出评分请求来生成预测性分析。

### 开发和持久存储定制模型

1. 使用 Data Science
Experience 创建定制模型。注册后，登录并完成以下步骤。

2. 第一次登录时，系统将要求您创建组织和空间。单击**继续**并接受缺省值。

3. 在创建组织后，转至**我的项目**，然后单击**创建项目**。

4. 指定项目的名称和描述，然后单击**创建**。指定的项目名称还将用作“目标容器”的名称。

5. 创建项目后，您可以添加配置页，并开始开发自己的模型，也可以上传以下某个样本配置页：

   *  [使用 Python 开发 Spark MLlib 模型](https://apsportal.ibm.com/analytics/notebooks/89492fd6-a641-4819-9176-3d9381561df9/view?access_token=d80bef1a172d1d83d3721b101886337158457281774186f181a2e6a5b57f5ec7)

   *  [使用 Scala 开发 Spark MLlib 模型](https://apsportal.ibm.com/analytics/notebooks/c8652d2c-bfc9-4354-8168-f1c9f7f8dfc2/view?access_token=02a83fea8450a452c8de76af98dae078459d0f56810ddef4f4c62d5bc4fc72cf)

   *  [使用 Python 开发 scikit-learn 模型](https://apsportal.ibm.com/analytics/notebooks/5215a61a-16d7-4fa2-b060-e3e243ceebe3/view?access_token=70f48c95c5571a614ce97484d3f168b1d9b6aeebce015187d3d77ce6038f025e)



### 定制模型的部署和评分

请参阅以下各部分，以获取有关模型部署和评分的指示信息，或者参阅先前链接到的配置页的评分部分。

*  [部署联机模型](pm_service_api_spark_online.html)

*  [部署批处理模型](pm_service_api_spark_batch.html)

*  [部署流式模型](pm_service_api_spark_streaming.html)
