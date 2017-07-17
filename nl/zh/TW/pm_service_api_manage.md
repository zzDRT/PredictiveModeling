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

# 管理已部署的 SPSS 模型


*  [部署或重新整理預測模型](#deploying-or-refreshing-a-predictive-model)

*  [擷取所有目前已部署的模型清單](#retrieving-a-list-of-all-currently-deployed-models)

*  [下載特定已部署的模型檔的副本](#downloading-a-copy-of-a-specific-deployed-model-file)

*  [刪除已部署的預測模型](#deleting-a-deployed-predictive-model)

## 部署或重新整理預測模型

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

## 擷取所有目前已部署的模型清單

GET http://{PA Bluemix load balancer
URL}/pm/v1/model?accesskey={access_key for this bound
application}

擷取目前已部署在此服務實例上的所有模型之摘要。

要求範例：

```
    Content-Type: */*
    Parameters:
        Query Parameters:
            accesskey: access_key from env.VCAP_SERVICES
```
{: codeblock}

當已部署模型的摘要要求成功時回應：

```
    Content-Type: application/json
    Status code: 200
    body: an empty array if no models have been deployed or a summary of the deployed models...
        [
            {
                "stream":"neural_net1.str",
                "id":"1"
            },
            {
                "stream":"neural_net2.str",
                "id":"2"
            },
            ...
        ]
```
{: codeblock}

當已部署模型的摘要要求失敗時回應：

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

## 下載特定已部署的模型檔的副本

GET http://{PA Bluemix load balancer
URL}/pm/v1/model/{contextId}?accesskey={access_key for this bound
application}

使用這個 API 呼叫來下載特定的已部署模型檔的副本。

要求範例：

```
    Content-Type: */*
    Parameters:
        Path parameters:
            contextId: the identifier of deployed predictive model you want to download
        Query Parameters:
            accesskey: access_key from env.VCAP_SERVICES
```
{: codeblock}

當下載要求成功時回應：

```
    Content-Type: application/octet-stream
    Status code: 200
    Header:
        "Content-Disposition":"attachment; filename=model_file_name.str"
```
{: codeblock}

當下載要求失敗時回應：

```
    Content-Type: application/json
    Status code: 200
    body:
        {
           "flag":false, 
           "message":String // failure reason 
        }
```
{: codeblock}

## 刪除已部署的預測模型

DELETE http://{service
instance}/pm/v1/model/{contextId}?accesskey={access_key for this
bound application}

使用這個 API 呼叫來刪除 Machine Learning 服務實例中的預測模型。在這個呼叫之後，預測模型就不能再用於應用程式中下載或評分資料。

要求範例：

```
    Content-Type: */*
    Parameters:
        Path parameters:
            contextId: the identifier of deployed model you want to un-deploy and delete
        Query Parameters:
            accesskey: access_key from env.VCAP_SERVICES
```
{: codeblock}

當取消部署成功時回應：

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

當取消部署失敗時回應：

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
