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

# 현재 배치된 모든 모델의 목록 검색


GET http://{PA Bluemix load balancer URL}/pm/v1/model?accesskey={access_key for
this bound application}

현재 이 서비스 인스턴스에 배치되는 모든 모델의 요약을 검색합니다. 

요청 예제: 

```
    Content-Type: */*
    Parameters:
        Query Parameters:
            accesskey: access_key from env.VCAP_SERVICES
```
{: codeblock}

배치된 모델 요약에 대한 요청이 성공한 경우의 응답:

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

배치된 모델 요약에 대한 요청이 실패한 경우의 응답:

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
