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


*  [部署或刷新预测模型](#deploying-or-refreshing-a-predictive-model)

*  [检索所有当前已部署模型的列表](#retrieving-a-list-of-all-currently-deployed-models)

*  [下载特定已部署模型文件的副本](#downloading-a-copy-of-a-specific-deployed-model-file)

*  [删除已部署的预测模型](#deleting-a-deployed-predictive-model)

## 部署或刷新预测模型

PUT http://{PA Bluemix load balancer
URL}/pm/v1/model/{contextId}?accesskey={access_key for this bound
application}

使用此 API 调用可上传模型文件，其中包含要部署的 IBM SPSS Modeler 开发的评分分支。此文件可用于对应用程序中的数据进行评分。为了方便在后续服务调用中引用已部署的模型，系统会为每个模型文件提供一个 contextID 来作为别名。如果存在与 contextID 对应的模型，那么此 PUT 调用会替换该模型，从而来刷新应用程序正在使用的预测性分析。

请求示例：

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

部署成功时的响应：

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

部署失败时的响应：

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

## 检索所有当前已部署模型的列表

GET http://{PA Bluemix load balancer
URL}/pm/v1/model?accesskey={access_key for this bound
application}

检索当前在此服务实例上部署的所有模型的一览表。

请求示例：

```
    Content-Type: */*
    Parameters:
        Query Parameters:
            accesskey: access_key from env.VCAP_SERVICES
```
{: codeblock}

对已部署模型摘要的请求成功时的响应：

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

对已部署模型摘要的请求失败时的响应：

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

## 下载特定已部署模型文件的副本

GET http://{PA Bluemix load balancer
URL}/pm/v1/model/{contextId}?accesskey={access_key for this bound
application}

使用此 API 调用可下载特定已部署模型文件的副本。

请求示例：

```
    Content-Type: */*
    Parameters:
        Path parameters:
            contextId: the identifier of deployed predictive model you want to download
        Query Parameters:
            accesskey: access_key from env.VCAP_SERVICES
```
{: codeblock}

下载请求成功时的响应：

```
    Content-Type: application/octet-stream
    Status code: 200
    Header:
        "Content-Disposition":"attachment; filename=model_file_name.str"
```
{: codeblock}

下载请求失败时的响应：

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

## 删除已部署的预测模型

DELETE http://{service
instance}/pm/v1/model/{contextId}?accesskey={access_key for this
bound application}

使用此 API 调用可从 Machine
Learning 服务实例中删除预测模型。执行此调用后，预测模型将不再可供下载，也不再可用于对应用程序中的数据进行评分。

请求示例：

```
    Content-Type: */*
    Parameters:
        Path parameters:
            contextId: the identifier of deployed model you want to un-deploy and delete
        Query Parameters:
            accesskey: access_key from env.VCAP_SERVICES
```
{: codeblock}

取消部署成功时的响应：

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

取消部署失败时的响应：

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
