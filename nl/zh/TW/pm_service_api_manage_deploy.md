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

# 部署或重新整理預測模型


PUT http://{PA Bluemix load balancer
URL}/pm/v1/model/{contextId}?accesskey={access_key for this bound
application}

使用這個 API 呼叫來上傳檔案，該檔案包含您想要部署的 IBM SPSS Modeler 開發的評分分支。它可用於應用程式中評分資料。每一個模型檔會有一個 context ID 作為方便的別名，用來參照後續的服務呼叫中已部署的模型。如果某個 context ID 的模型存在，它會被這個 PUT 呼叫取代，這個方法重新整理您應用程式中使用的預測分析。

要求範例：

```
    Content-Type: multipart/form-data
    Parameters:
        Form parameters:
            model_file: the model file to upload
        Path parameters:
            contextId: the unique identifier to assign to your model or a reference to the deployed model to refresh
        Query Parameters:
            accesskey: access_key from env.VCAP_SERVICES
```
{: codeblock}

當部署成功時回應：

```
    Content-Type: application/json
    Status code: 200
    body:
        {
           "flag":true, 
           "message":"detailed information"  
         }
```
{: codeblock}

當部署失敗時回應：

```
    Content-Type: application/json
    Status code: 200
    body:
        {
           "flag":false, 
           "message":"reason"
        }
```
{: codeblock}
