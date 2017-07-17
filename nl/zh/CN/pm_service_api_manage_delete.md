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

# 删除已部署的预测模型


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
