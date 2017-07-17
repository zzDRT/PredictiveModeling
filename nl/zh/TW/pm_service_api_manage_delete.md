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

# 刪除已部署的預測模型


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
