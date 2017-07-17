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

# 배치된 예측 모델 삭제


DELETE http://{service
instance}/pm/v1/model/{contextId}?accesskey={access_key for this
bound application}

이 API 호출을 사용하여 Machine Learning 서비스 인스턴스에서 예측 모델을 삭제하십시오. 이 호출 후에 예측 모델은 더 이상 애플리케이션에서 데이터를 다운로드하거나 스코어링할 수 없게 됩니다.


요청 예제: 

```
    Content-Type: */*
    Parameters:
        Path parameters:
            contextId: the identifier of deployed model you want to un-deploy and delete
        Query Parameters:
            accesskey: access_key from env.VCAP_SERVICES
```
{: codeblock}

배치 취소에 성공한 경우의 응답:

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

배치 취소에 실패한 경우의 응답:

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
