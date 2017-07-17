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

# 使用已部署的預測模型進行評分


POST http://{PA Bluemix load balancer
URL}/pm/v1/score/{contextId}?accesskey={access_key for this bound
application}

使用這個 API 呼叫來公佈輸入資料，供已部署的模型用以產生預測分析並傳回至評分結果中。

要求範例：

```
    Content-Type: application/json;charset=UTF-8
    Parameters:
        Path parameters:
            contextId: the identifier of the deployed model to use to process this score request
        Query Parameters:
            accesskey: access_key from env.VCAP_SERVICES
        Body: the input data, json string, eg.
            {
                "tablename":"DRUG1n.sav", 
                "header":["Age", "Sex", "BP", "Cholesterol", "Na", "K", "Drug"], 
                "data":[[43.0, "M", "LOW", "NORMAL", 0.526102, 0.027164, "drugY"]]
            }   
```
{: codeblock}

前一個要求的成功回應範例：

```
    Content-Type: application/json;charset=UTF-8
    Status code: 200
    body: the score result, a json array, eg.
        [
            {
                "header":["Age","Sex","BP","Cholesterol","Na""K","Drug","$N-Drug","$NC-Drug"], 
                "data":[[23.0,"M","NORMAL","NORMAL",0.78452,0.055959,"drugX","drugX",0.9892564426956728]]
            }
        ]
```
{: codeblock}

當評分要求失敗時回應：

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
