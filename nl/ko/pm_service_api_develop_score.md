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

# 배치된 예측 모델로 스코어링


POST http://{PA Bluemix load balancer
URL}/pm/v1/score/{contextId}?accesskey={access_key for this bound
application}

이 API 호출을 사용하여 스코어 결과에서 예측 분석을 생성하고 리턴하기 위해 배치된 모델에서 사용할 입력 데이터를 게시하십시오.


요청 예제: 

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

이전 요청에 대한 성공적인 응답의 예제:

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

스코어링 요청이 실패한 경우의 응답:

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
