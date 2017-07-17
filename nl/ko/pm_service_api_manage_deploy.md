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

# 예측 모델 배치 또는 새로 고치기


PUT http://{PA Bluemix load balancer
URL}/pm/v1/model/{contextId}?accesskey={access_key for this bound application}

이 API 호출을 사용하여 배치하려는 IBM SPSS Modeler 개발 스코어링 분기를 포함하는
파일을 업로드하십시오. 애플리케이션의 스코어링 데이터에 대해 사용할 수 있습니다. 각 모델 파일은
후속 서비스 호출에서 배치된 모델을 참조하는 데 사용하기 위한 편리한 별명으로 컨텍스트 ID가
지정됩니다. 컨텍스트 ID에 대한 모델이 존재하는 경우, 애플리케이션에서 사용 중인
예측 분석을 새로 고치는 방법으로 이 PUT 호출에 의해 모델이 대체됩니다.


요청 예제: 

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

배치에 성공한 경우의 응답:

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

배치에 실패한 경우의 응답:

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
