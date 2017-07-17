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

# Spark 及 Python 模型的 Machine Learning 服務 API


Machine Learning 服務是一組 [REST API](https://watson-ml-api.mybluemix.net/)，可以從任何程式設計語言呼叫，允許在應用程式中整合 Data Science Experience 開發的分析。將 Bluemix 應用程式連結到 Machine Learning 服務實例，並產生應用程式需要的預測分析，來為使用者提供更高的價值。您可以在管理儀表板中管理模型，並使用儀表板來建立與應用程式整合的線上、批次或串流部署。

管理 Data Science Experience 檔案（Spark 及 Python 模型，可透過 Watson Machine Learning Dashboard，在 Machine Learning 服務的實例上取得）：

*  匯入範例模型

*  顯示模型詳細資料

*  將模型部署為：

   *  線上部署（評分）

   *  批次部署（支援 IBM Object Storage 和 DashDB）

   *  串流部署（支援 IBM MessageHub）

*  取得部署詳細資料

*  刪除部署


透過功能強大的 [REST API](https://watson-ml-api.mybluemix.net/)，依據部署在服務實例上的 Data Science Experience 檔案（Spark 及 Python 模型）來開發應用程式：

*  針對給定的預測模型來擷取 meta 資料

*  部署模型以及管理所部署的模型

*  針對給定的部署來擷取 meta 資料

*  針對所部署的模型提出評分要求，以產生預測分析

您可以透過可用的 [REST API 的 Swagger 表示法](https://watson-ml-api.mybluemix.net/)，輕鬆探索 Machine Learning 服務功能。

請參閱下列各節，以取得 REST API 的範例：

*  [線上部署和評分](pm_service_api_spark_online.html)

*  [使用 Object Storage 進行批次部署](pm_service_api_spark_batch.html)

*  [使用 MessageHub 進行串流部署](pm_service_api_spark_streaming.html)
