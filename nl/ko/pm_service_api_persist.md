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

# 지속 모델


*  [모델 지속성 및 버전 제어](#model-persistence-and-version-control)

   *  [액세스 토큰 생성](#generating-the-access-token)

   *  [파이프라인 메타데이터 작성](#creating-pipeline-metadata)

   *  [파이프라인 버전 작성](#creating-pipeline-version)

   *  [파이프라인 컨텐츠 업로드](#uploading-pipeline-content)

   *  [파이프라인 모델 메타데이터 작성](#creating-pipeline-model-metadata)

   *  [파이프라인 모델 버전 작성](#creating-pipeline-model-version)

   *  [파이프라인 모델 컨텐츠 업로드](#uploading-pipeline-model-content)

데이터 과학자는 연구 및 개발의 일부로 모델을 개선하기 위해 끊임없이 노력하고
있습니다. 새로운 기능을 기존 모델에 추가하고 모델 매개변수를 최적화할 수 있습니다. 데이터 과학자가
반복적인 방식으로 모델을 개발하는 경우 변경사항을 꾸준히 파악하고 있으면 신속하게 문제를 확인할 수
있습니다. 다음은 모델 버전화를 제공하고 전체 프로세스를 올바르게 구성함으로써 데이터 과학자를 지원하는 방법을 보여줍니다. 

시작하기 전에 모델 개발에 처음으로 관심을 갖는 경우라면 다음 노트북을 참조하십시오. 

*  [Python을 사용하여 SparkML 모델 개발](https://apsportal.ibm.com/exchange/public/entry/view/d80de77f784fed7915c14353512ef14d)

*  [Scala를 사용하여 SparkML 모델 개발](https://apsportal.ibm.com/exchange/public/entry/view/d80de77f784fed7915c1435351309e93)

## 모델 지속성 및 버전 제어

### 액세스 토큰 생성

IBM® Watson™ Machine Learning 서비스 인스턴스의 서비스 신임 정보 탭에서 사용 가능한 사용자 및
비밀번호를 사용하여 액세스 토큰을 생성하십시오. 

요청 예제: 

```
curl --basic --user username:password https://ibm-watson-ml.mybluemix.net/v2/identity/token
```
{: codeblock}

출력 예제: 

```
{"token":"**********"}
```
{: codeblock}

토큰 값을 access_token 환경 변수에 지정하려면 다음 터미널 명령을 사용하십시오. 

```
access_token="<token_value>"
```
{: codeblock}

다음으로, 파이프라인 및 파이프라인 모델을 저장하려면, 파이프라인 및 파이프라인 모델 메타데이터를 작성하고,
파이프라인 및 파이프라인 모델 버전을 작성한 후, 파이프라인 및 파이프라인 모델 버전을 업로드합니다. 세부사항은 다음 섹션을 참조하십시오. 

### 파이프라인 메타데이터 작성

파이프라인에 대한 메타데이터를 작성하려면 다음 예제에 표시된 대로 curl 요청에 파이프라인의 기본 특성을 설명하십시오. 

```
curl -i \
-X POST \
-H 'Content-Type: application/json' \
-H "Authorization: Bearer $access_token" \
-d '{ 
  "name": "sample pipeline",
  "description": "sample description",
  "author": {
    "name": "John Smith",
    "email": "john.smith@example.com"
  },
  "type": "sparkml-pipeline-2.0",
  "runtimeEnvironment": "spark-2.0"
}' \
https://ibm-watson-ml.mybluemix.net/v2/artifacts/pipelines
```
{: codeblock}

예제
응답:

```
 HTTP/1.1 201 Created
 X-Backside-Transport: OK OK
 Connection: Keep-Alive
 Transfer-Encoding: chunked
 Content-Type: application/json
 Date: Fri, 17 Feb 2017 10:46:16 GMT
 Location: https://ibm-watson-ml.stage1.mybluemix.net/v2/artifacts/pipelines/223b38a6-41c6-417c-91ad-51064261b6f7
 Server: nginx/1.11.5
 X-Global-Transaction-ID: 3172115887
* Connection #0 to host ibm-watson-ml.stage1.mybluemix.net left intact
{"ok":"true"}
```
{: codeblock}

### 파이프라인 버전 작성

파이프라인에 대한 버전을 작성하려면 다음 예제에 표시된 대로 curl 요청에 파이프라인에 대한 parentVersionHref를 지정하십시오. 

```
curl -i \
-X POST \
-H 'Content-Type: application/json' \
-H "Authorization: Bearer $access_token" \
-d '{}' \
https://ibm-watson-ml.mybluemix.net/v2/artifacts/pipelines/1314f74d-a2a7-46d3-8f11-89baa1d7ffea/versions
```
{: codeblock}

예제
응답:

```
 HTTP/1.1 201 Created
 X-Backside-Transport: OK OK
 Connection: Keep-Alive
 Transfer-Encoding: chunked
 Content-Type: application/json
 Date: Fri, 17 Feb 2017 10:47:56 GMT
 Location: https://ibm-watson-ml.stage1.mybluemix.net/v2/artifacts/pipelines/223b38a6-41c6-417c-91ad-51064261b6f7/versions/0bb830fc-bc1e-439a-b929-685d9fc7712d
 Server: nginx/1.11.5
 X-Global-Transaction-ID: 3510865585
* Connection #0 to host ibm-watson-ml.stage1.mybluemix.net left intact
{"ok":"true"}
```
{: codeblock}

### 파이프라인 컨텐츠 업로드

파이프라인 컨텐츠를 업로드하려면, 파이프라인이 2진 형식이어야 합니다. 이를 수행하려면 SparkML 파이프라인에서 save 메소드를 호출하십시오. 다음 예제에 표시된 대로
파이프라인의 ID 및 버전을 엔드포인트에 제공하여 2진 컨텐츠를 업로드할 수 있습니다. 

```
curl -i \
-X PUT \
-H 'Content-Type: application/json' \
-H "Authorization: Bearer $access_token" \
-d <path_to_your_binary_content> \
https://ibm-watson-ml.mybluemix.net/v2/artifacts/pipelines/1314f74d-a2a7-46d3-8f11-89baa1d7ffea/versions/a535417c-ba8f-43b5-aae5-7dd8af02f518/content
```
{: codeblock}

예제
응답:

```
 HTTP/1.1 200 OK
 X-Backside-Transport: OK OK
 Connection: Keep-Alive
 Transfer-Encoding: chunked
 Content-Type: application/json
 Date: Fri, 17 Feb 2017 10:50:53 GMT
 Server: nginx/1.11.5
 X-Global-Transaction-ID: 980490895
* Connection #0 to host ibm-watson-ml.stage1.mybluemix.net left intact
{"ok":"true"}
```
{: codeblock}

파이프라인을 저장한 후에 파이프라인 모델 저장을 계속할 수 있습니다. 

### 파이프라인 모델 메타데이터 작성

다음 예제에 표시된 대로 파이프라인 모델의 데이터 스키마에 대해 설명해야 합니다.


```
curl -i \
-X POST \
-H 'Content-Type: application/json' \
-H "Authorization: Bearer $access_token" \
-d ' {
  "name": "sample-model",
  "description": "sample description",
  "author": {
    "name": "John Smith",
    "email": "john.smith@example.com"
  },
  "pipelineVersionHref": "string",
  "type": "sparkml-model-2.0",
  "runtimeEnvironment": "spark-2.0",
  "trainingDataSchema": {
    "type": "struct",
    "fields": [
      {
        "name": "id",
        "type": "double",
        "nullable": true,
        "metadata": {}
      },
      {
        "name": "review",
        "type": "string",
        "nullable": true,
        "metadata": {}
      },
      {
        "name": "label",
        "type": "int",
        "nullable": true,
        "metadata": {}
      }
    ]
  },
  "labelCol": "string",
  "inputDataSchema": {
    "type": "struct",
    "fields": [
      {
        "name": "id",
        "type": "double",
        "nullable": true,
        "metadata": {}
      },
      {
        "name": "review",
        "type": "string",
        "nullable": true,
        "metadata": {}
      }
    ]
  }
}' \
https://ibm-watson-ml.mybluemix.net/v2/artifacts/models
```
{: codeblock}

예제
응답:

```
 HTTP/1.1 201 Created
 X-Backside-Transport: OK OK
 Connection: Keep-Alive
 Transfer-Encoding: chunked
 Content-Type: application/json
 Date: Fri, 17 Feb 2017 10:58:29 GMT
 Location: https://ibm-watson-ml.stage1.mybluemix.net/v2/artifacts/models/ce274d29-49d0-43d8-adf7-6d9afa73c9cb
 Server: nginx/1.11.5
 X-Global-Transaction-ID: 437931687
* Connection #0 to host ibm-watson-ml.stage1.mybluemix.net left intact
{"ok":"true"}
```
{: codeblock}

### 파이프라인 모델 버전 작성

파이프라인 모델에 대한 버전을 작성하려면 curl 요청을 사용하여
훈련 데이터를 저장하는 위치 및 모델 평가 메소드와 같은 세부사항을 지정하십시오. 다음 예제를
참조하십시오. 

```
curl -i \
-X POST \
-H 'Content-Type: application/json' \
-H "Authorization: Bearer $access_token" \
-d ' {
  "trainingDataRef": {
    "connection": {
      "auth_url": "https://identity.open.softlayer.com",
      "project": "object_storage_008c8b63_b3d6_47c4_9da2_6c3ea6cfa713",
      "projectId": "f771e5d082e24857adb7554d15fc357c",
      "region": "dallas",
      "userId": "9b2e44721b91471ab86ecc41aeb7d37f",
      "username": "admin_926e1098fc72ff3c59d9222a2ab31a8a22143497",
      "password": "xxxxxxxxxxxxxxxx",
      "domainId": "a7b64174a5d74134b73b1533f6b99ae6",
      "domainName": "1143537",
      "role": "admin"
    },
    "source": {
      "container": "notebooks",
      "filename": "WA_Fn-UseC_-Telco-Customer-Churn.csv",
      "fileformat": "csv",
      "inferschema": 1,
      "firstlineheader": true
    }
  },
  "evaluation": {
    "method": "Binary",
    "metrics": [
      {
        "name": "areaUnderROC",
        "threshold": 0.8
      }
    ]
  }
}' \
https://ibm-watson-ml.mybluemix.net/v2/artifacts/models/30ed894b-4018-4c88-9e53-86005548317a/versions
```
{: codeblock}

예제
응답:

```
 HTTP/1.1 201 Created
 X-Backside-Transport: OK OK
 Connection: Keep-Alive
 Transfer-Encoding: chunked
 Content-Type: application/json
 Date: Fri, 17 Feb 2017 11:10:12 GMT
 Location: https://ibm-watson-ml.stage1.mybluemix.net/v2/artifacts/models/ce274d29-49d0-43d8-adf7-6d9afa73c9cb/versions/8be22c6e-e214-45a4-8cfb-be4cade109ef
 Server: nginx/1.11.5
 X-Global-Transaction-ID: 285978151
* Connection #0 to host ibm-watson-ml.stage1.mybluemix.net left intact
{"ok":"true"}
```
{: codeblock}

### 파이프라인 모델 컨텐츠 업로드

파이프라인 모델 컨텐츠를 업로드하려면, 파이프라인 모델이 2진 형식이어야 합니다. 이를 수행하려면 SparkML 파이프라인 모델에서 save 메소드를 호출하십시오. 다음 예제에 표시된 대로
파이프라인의 ID 및 버전을 엔드포인트에 제공하여 2진 컨텐츠를 업로드할 수 있습니다. 

```
curl -i \
-X PUT \
-H 'Content-Type: application/json' \
-H "Authorization: Bearer $access_token" \
-d <path_to_your_binary_content> \
https://ibm-watson-ml.mybluemix.net/ v2/artifacts/pipelines/1314f74d-a2a7-46d3-8f11-89baa1d7ffea/versions/bb328cc3-b78d-4527-9dcc-f2172f43ae9e/content
```
{: codeblock}

예제
응답:

```
 HTTP/1.1 200 OK
 X-Backside-Transport: OK OK
 Connection: Keep-Alive
 Transfer-Encoding: chunked
 Content-Type: application/json
 Date: Fri, 17 Feb 2017 11:11:23 GMT
 Server: nginx/1.11.5
 X-Global-Transaction-ID: 605948113
* Connection #0 to host ibm-watson-ml.stage1.mybluemix.net left intact
{"ok":"true"}
```
{: codeblock}
