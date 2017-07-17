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

# 自訂模型的開發和持續性

## 使用自訂模型

### 情境名稱：產品線預測。

### 情境說明：

我們的客戶經營歐洲最著名的連鎖店之一。他們想要我們依據其產品線（例如個人配件、露營設備和戶外防護器材），找出其客戶的興趣所在。資料科學家開發出一套預測模型，並將其與您（開發人員）分享。您的工作是部署模型，然後對已配置的模型提出評分要求，以產生預測分析模型。

### 自訂模型的開發和持續性

1. 使用 Data Science Experience 可建立自訂模型。註冊之後，請登入並完成下列步驟。

2. 您第一次登入時，系統會要求您建立組織和空間。請按一下**繼續**並接受預設值。

3. 建立組織之後，移至**我的專案**，然後按一下**建立專案**。

4. 指定專案的名稱和說明，然後按一下**建立**。您指定的專案名稱也會用來做為「目標容器」的名稱。

5. 建立專案之後，您可以新增記事本，並開始開發自己的模型，也可以上傳下列其中一個範例記事本：

   *  [Developing Spark MLlib models with Python](https://apsportal.ibm.com/analytics/notebooks/89492fd6-a641-4819-9176-3d9381561df9/view?access_token=d80bef1a172d1d83d3721b101886337158457281774186f181a2e6a5b57f5ec7)

   *  [Developing Spark MLlib models with Scala](https://apsportal.ibm.com/analytics/notebooks/c8652d2c-bfc9-4354-8168-f1c9f7f8dfc2/view?access_token=02a83fea8450a452c8de76af98dae078459d0f56810ddef4f4c62d5bc4fc72cf)

   *  [Developing scikit-learn models with Python](https://apsportal.ibm.com/analytics/notebooks/5215a61a-16d7-4fa2-b060-e3e243ceebe3/view?access_token=70f48c95c5571a614ce97484d3f168b1d9b6aeebce015187d3d77ce6038f025e)



### 自訂模型的部署和評分

請參閱下列各節，以取得部署和評分模型的相關指示，或查看先前鏈結的記事本評分區段。

*  [部署線上模型](pm_service_api_spark_online.html)

*  [部署批次模型](pm_service_api_spark_batch.html)

*  [部署串流模型](pm_service_api_spark_streaming.html)
