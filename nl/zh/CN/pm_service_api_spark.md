---

copyright:
  years: 2016, 2017
lastupdated: "2017-06-23"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Spark 和 Python 模型的 Machine Learning 服务 API


Machine Learning 服务是一组可通过任何编程语言进行调用的 [REST API](https://watson-ml-api.mybluemix.net/)，支持将 Data Science Experience 开发的分析集成到应用程序中。将 Bluemix 应用程序绑定到 Machine Learning 服务实例，然后生成预测性分析，您的应用程序需要该分析来为用户提供更高的价值。在管理仪表板中管理模型，并使用该仪表板来创建与应用程序集成的联机、批量或流式部署。

通过 Watson Machine Learning 仪表板管理在 Machine Learning 服务实例上可用的 Data Science Experience 文件（Spark 和 Python 模型）：

*  导入样本模型

*  显示模型详细信息

*  将模型部署为：

   *  联机部署（评分）

   *  批量部署（支持 IBM Object Storage 和 DashDB）

   *  流式部署（支持 IBM MessageHub）

*  获取部署详细信息

*  删除部署


基于通过强大的 [REST API](https://watson-ml-api.mybluemix.net/) 部署在服务实例上的 Data Science Experience 文件（Spark 和 Python 模型）开发应用程序：

*  检索给定预测模型的元数据

*  部署模型和管理已部署的模型

*  检索给定部署的元数据

*  通过对已部署模型发出评分请求来生成预测性分析

您可以通过可用的 [REST API 的 Swagger 表示](https://watson-ml-api.mybluemix.net/)轻松探索 Machine Learning 服务功能。

请参阅以下各部分以获取 REST API 示例：

*  [联机部署和评分](pm_service_api_spark_online.html)

*  [使用 Object Storage 批量部署](pm_service_api_spark_batch.html)

*  [使用 MessageHub 流式部署](pm_service_api_spark_streaming.html)
