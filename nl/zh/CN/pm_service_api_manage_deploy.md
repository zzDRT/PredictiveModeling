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

# 部署或刷新预测模型


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
