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

# 스트리밍 모델 배치


**참고**: 이 기능은 현재 베타이며 Spark MLlib와 함께 사용하기 위한 경우에만 사용 가능합니다. 

**시나리오 이름**: 감성 분석

**시나리오 설명**: 마케팅 에이전시에서 특정 주제에 대한 감성에 대해 알고자 합니다.
에이전시는 POSITIVE 또는 NEGATIVE와 같은 주어진 명령문을 분류하는 모델을
개발하도록 요청합니다. 데이터 과학자는 예측 모델을 준비하고
사용자(개발자)와 이를 공유합니다. 여러분의 태스크는
모델을 배치하고 배치된 모델에 대한 스코어 요청을 작성하여 예측 분석을 생성하는 것입니다. 

자세한 정보는 이 문서의 내용을 참조하십시오. 

## 샘플 모델 사용

1. IBM® Watson™ Machine Learning 대시보드의 샘플 탭으로 이동하십시오. 

2. 샘플 모델 섹션에서 감성 예측 타일을 찾아서 모델 추가 단추(+)를 클릭하십시오. 

이제 모델 탭의 사용 가능한 모델 목록에 샘플 감성 예측 모델이 표시됩니다. 

## 액세스 토큰 생성

IBM Watson Machine Learning 서비스 인스턴스의 서비스 신임 정보 탭에서 사용 가능한 사용자 및
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

## IBM Message Hub를 사용하여 스트리밍 배치 작성

예측 모델의 스트리밍 배치를 작성하기 위해 REST API 호출을 사용하려면, 다음 세부사항을 제공하십시오. 

*  이전 단계에서 작성된 액세스 토큰

*  Spark 서비스 신임 정보는 Bluemix Spark 서비스 대시보드의 서비스 신임 정보 탭에서
찾을 수 있습니다. 배치 요청을 작성하기 전에 Spark 신임 정보를 Base64로 디코딩해야 하며
X-Spark-Service-Instance로 curl 요청 헤더에 전달해야 합니다. 

   사용 중인 운영 체제에 따라서 base64 디코딩을 수행하고 이를 환경 변수에 지정하려면 다음 터미널 명령 중 하나를 실행해야 합니다. 

   macOS 운영 체제에서 다음 명령을 사용하십시오. 

   ```
   spark_credentials=$(echo '{"credentials": {"tenant_id": "s068-ade10277b64956-05b1d10fv12b","tenant_id_full": "00fd89e6-8cf2-4712-a068-ade10277b649_41f37bf2-1b95-4c65-a156-05b1d10fb12b","cluster_master_url": "https://spark.bluemix.net","instance_id": "00fd89e6-8cf2-4712-a068-ade10277b649","tenant_secret": "c74c37cf-482a-4da4-836e-f32ca26ccbb9","plan": "ibm.SparkService.PayGoPersonal"},"version": "2.0"}' | base64)
   ```
{: codeblock}

   Microsoft Windows 또는 Linux 운영 체제에서는 base64 디코딩을 수행하려면 `base64` 명령과 함께 `--wrap=0` 매개변수를 사용해야 합니다.

   ```
   spark_credentials=$(echo '{"credentials": {"tenant_id": "s068-ade10277b64956-05b1d10fv12b","tenant_id_full": "00fd89e6-8cf2-4712-a068-ade10277b649_41f37bf2-1b95-4c65-a156-05b1d10fb12b","cluster_master_url": "https://spark.bluemix.net","instance_id": "00fd89e6-8cf2-4712-a068-ade10277b649","tenant_secret": "c74c37cf-482a-4da4-836e-f32ca26ccbb9","plan": "ibm.SparkService.PayGoPersonal"},"version": "2.0"}' | base64 --wrap=0)
   ```
{: codeblock}

*  모델에 대한 입력(트윗) 및 모델 출력(예측 결과)에 대한 스토리지로 사용되는 IBM Message Hub 주제 세부사항. 

*  배치를 작성하려면 다음 엔드포인트를 사용하십시오. 
   
   ```
   /v2/published_models/{published_model_id}/deployments
   ```
   
   온라인 배치를 작성하는 데 필요한 엔드포인트는 WML 대시보드 -> 모델 세부사항 -> URL에서 사용 가능합니다. IBM Watson Machine Learning 대시보드에서 published_model_id 값을 찾으려면
모델 -> 세부사항 보기를 클릭하고 ID 필드에서 값을 복사하고 Watson Machine Learning 인스턴스의 VCAP 신임 정보에서 "instance_id"를 가져올 수 있습니다. 

요청 예제: 

```
curl -i \
-X POST \
-H 'Content-Type: application/json' \
-H "Authorization: Bearer $access_token" \
-H "X-Spark-Service-Instance: $spark_credentials" \
-d '{
   "name":"Sentiment Prediction ",
   "type":"stream",
   "description": "Streaming Deployment",
   "input":{
      "connection":{
         "kafka_brokers_sasl":[
            "kafka01-prod01.messagehub.services.us-south.bluemix.net:9093",
            "kafka02-prod01.messagehub.services.us-south.bluemix.net:9093",
            "kafka03-prod01.messagehub.services.us-south.bluemix.net:9093",
            "kafka04-prod01.messagehub.services.us-south.bluemix.net:9093",
            "kafka05-prod01.messagehub.services.us-south.bluemix.net:9093"
         ],
         "kafka_admin_url":"https://kafka-admin-prod01.messagehub.services.us-south.bluemix.net:443",
         "api_key":"Dv5kEVNNsbuJ9RFEKdUhIn2hruipIrsBolge6v1uQmTzEQti",
         "mqlight_lookup_url":"https://mqlight-lookup-prod01.messagehub.services.us-south.bluemix.net/Lookup?serviceId=5448397d-cb22-4698-8a2b-ffb04f43a4cb",
         "kafka_rest_url":"https://kafka-rest-prod01.messagehub.services.us-south.bluemix.net:443",
         "user":"Dv5kEVNNsbuJ9RFE",
         "password":"KdahIn2hruipIrsBolge6v1uQmTzEQti"
      },
      "source":{
         "topic":"sinput",
         "type":"kafka"
      }
   },
   "output":{
      "connection":{
         "kafka_brokers_sasl":[
            "kafka01-prod01.messagehub.services.us-south.bluemix.net:9093",
            "kafka02-prod01.messagehub.services.us-south.bluemix.net:9093",
            "kafka03-prod01.messagehub.services.us-south.bluemix.net:9093",
            "kafka04-prod01.messagehub.services.us-south.bluemix.net:9093",
            "kafka05-prod01.messagehub.services.us-south.bluemix.net:9093"
         ],
         "kafka_admin_url":"https://kafka-admin-prod01.messagehub.services.us-south.bluemix.net:443",
         "api_key":"Dv5kEVNNsbuJ9RFEKdUhIn2hruipIrsBolge6v1uQmTzEQti",
         "mqlight_lookup_url":"https://mqlight-lookup-prod01.messagehub.services.us-south.bluemix.net/Lookup?serviceId=5448397d-cb22-4698-8a2b-ffb04f43a4cb",
         "kafka_rest_url":"https://kafka-rest-prod01.messagehub.services.us-south.bluemix.net:443",
         "user":"Dv5kEVNNsbuJ9RFE",
         "password":"KdahIn2hruipIrsBolge6v1uQmTzEQti"
      },
      "target":{
         "topic":"soutput",
         "type":"kafka"
      }
   }
}' \
https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/{published_model_id}/deployments
```
{: codeblock}

출력 예제: 

```
{
   "metadata":{
      "guid":"0f5e58ff-db0d-4d33-9a53-9c4e452cdfc2",
      "url":"https://ibm-watson-ml.mybluemix.net/v3/wml_instances/b5bd2368-cc4b-4cf1-aec0-49ed8daeb018/published_models/ceed18b3-ada2-42db-bf00-429f4e2c7601/deployments/0f5e58ff-db0d-4d33-9a53-9c4e452cdfc2",
      "created_at":"2017-06-28T12:39:01.474Z",
      "modified_at":"2017-06-28T12:39:04.274Z"
   },
   "entity":{
      "runtime_environment":"spark-2.0",
      "name":"Sentiment Prediction ",
      "instance_href":"https://ibm-watson-ml.mybluemix.net/v2/streaming/deployments/553f0ed7-b3fc-4659-ae45-c42c66cb5e04",
      "description":"Streaming Deployment",
      "published_model":{
         "author":{
            "name":"IBM",
            "email":""
         },
         "name":"Sentiment Prediction",
         "url":"https://ibm-watson-ml.mybluemix.net/v3/wml_instances/b5bd2368-cc4b-4cf1-aec0-49ed8daeb018/published_models/ceed18b3-ada2-42db-bf00-429f4e2c7601",
         "guid":"ceed18b3-ada2-42db-bf00-429f4e2c7601",
         "description":"Predicts comment sentiment about particular topic for marketing company.",
         "created_at":"2017-06-28T11:57:47.636Z"
      },
      "model_type":"sparkml-model-2.0",
      "status":"INITIALIZING",
      "output":{
         "connection":{
            "kafka_brokers_sasl":[
               "kafka01-prod01.messagehub.services.us-south.bluemix.net:9093",
               "kafka02-prod01.messagehub.services.us-south.bluemix.net:9093",
               "kafka03-prod01.messagehub.services.us-south.bluemix.net:9093",
               "kafka04-prod01.messagehub.services.us-south.bluemix.net:9093",
               "kafka05-prod01.messagehub.services.us-south.bluemix.net:9093"
            ],
            "kafka_admin_url":"https://kafka-admin-prod01.messagehub.services.us-south.bluemix.net:443",
            "api_key":"gDic7zCjvBJQjMjBMGcVHeBLI5VkqELVQsiuMx8Ka0XtddRk",
            "mqlight_lookup_url":"https://mqlight-lookup-prod01.messagehub.services.us-south.bluemix.net/Lookup?serviceId=2e9e624d-967b-4bf3-8eda-504e10fbc14d",
            "kafka_rest_url":"https://kafka-rest-prod01.messagehub.services.us-south.bluemix.net:443",
            "user":"gDic7zCjvBJQjMjB",
            "password":"MGcVHeBLI5VkqELVQsiuMx8Ka0XtddRk"
         },
         "target":{
            "topic":"streamingo",
            "type":"kafka"
         }
      },
      "type":"stream",
      "deployed_version":{
         "url":"https://ibm-watson-ml.mybluemix.net/v2/artifacts/models/ceed18b3-ada2-42db-bf00-429f4e2c7601/versions/848e632f-1ffc-4241-992b-6301686eadbd",
         "guid":"848e632f-1ffc-4241-992b-6301686eadbd",
         "created_at":"2017-06-28T11:57:10.997Z"
      },
      "spark_service":{
         "credentials":{
            "tenant_id":"s000-5f656e4e614595-e0ddb27c0670",
            "cluster_master_url":"https://spark.bluemix.net",
            "tenant_id_full":"fba311a7-532e-4aad-9000-5f656e4e6145_bd856e56-b4f4-4e82-9b95-e0ddb27c0670",
            "tenant_secret":"d9627f06-7a25-46ed-b833-93fe799654c4",
            "instance_id":"fba311a7-532e-4aad-9000-5f656e4e6145",
            "plan":"ibm.SparkService.PayGoPersonal"
         },
         "version":"2.0"
      },
      "input":{
         "connection":{
            "kafka_brokers_sasl":[
               "kafka01-prod01.messagehub.services.us-south.bluemix.net:9093",
               "kafka02-prod01.messagehub.services.us-south.bluemix.net:9093",
               "kafka03-prod01.messagehub.services.us-south.bluemix.net:9093",
               "kafka04-prod01.messagehub.services.us-south.bluemix.net:9093",
               "kafka05-prod01.messagehub.services.us-south.bluemix.net:9093"
            ],
            "kafka_admin_url":"https://kafka-admin-prod01.messagehub.services.us-south.bluemix.net:443",
            "api_key":"gDic7zCjvBJQjMjBMGcVHeBLI5VkqELVQsiuMx8Ka0XtddRk",
            "mqlight_lookup_url":"https://mqlight-lookup-prod01.messagehub.services.us-south.bluemix.net/Lookup?serviceId=2e9e624d-967b-4bf3-8eda-504e10fbc14d",
            "kafka_rest_url":"https://kafka-rest-prod01.messagehub.services.us-south.bluemix.net:443",
            "user":"gDic7zCjvBJQjMjB",
            "password":"MGcVHeBLI5VkqELVQsiuMx8Ka0XtddRk"
         },
         "source":{
            "topic":"streamingi",
            "type":"kafka"
         }
      }
   }
}
```
{: codeblock}

**참고**: 대시보드를 사용하여 스트리밍 배치를 작성할 수도 있습니다. 다음 세 가지 항목을 입력해야 합니다. 

*  사용할 모델에 대한 입력 소스 세부사항. 입력 소스가 이미 업로드되어 있고 수집(IBM Message Hub)에 바로 사용 가능해야 합니다. 

*  출력 소스 세부사항. 결과가 쓰여질 위치 정보입니다. 출력 소스가 이미 작성되어 있고
모델이 입력 소스에서 오는 데이터를 처리할 때 해당 결과를 바로 수신할 수 있어야 합니다. 

*  Spark 서비스 신임 정보.

양식 예제:


입력:

```
{
   "connection":{
      "kafka_brokers_sasl":[
         "kafka01-prod01.messagehub.services.us-south.bluemix.net:9093",
         "kafka02-prod01.messagehub.services.us-south.bluemix.net:9093",
         "kafka03-prod01.messagehub.services.us-south.bluemix.net:9093",
         "kafka04-prod01.messagehub.services.us-south.bluemix.net:9093",
         "kafka05-prod01.messagehub.services.us-south.bluemix.net:9093"
      ],
      "kafka_admin_url":"https://kafka-admin-prod01.messagehub.services.us-south.bluemix.net:443",
      "api_key":"Dv5kEVNNsbuJ9RFEKdUhIn2hruipIrsBolge6v1uQmTzEQti",
      "mqlight_lookup_url":"https://mqlight-lookup-prod01.messagehub.services.us-south.bluemix.net/Lookup?serviceId=5448397d-cb22-4698-8a2b-ffb04f43a4cb",
      "kafka_rest_url":"https://kafka-rest-prod01.messagehub.services.us-south.bluemix.net:443",
      "user":"Dv5kEVNNsbuJ9RFE",
      "password":"KdahIn2hruipIrsBolge6v1uQmTzEQti"
   },
   "source":{
      "topic":"sinput",
      "type":"kafka"
   }
}
```
{: codeblock}

출력:

```
{
   "connection":{
      "kafka_brokers_sasl":[
         "kafka01-prod01.messagehub.services.us-south.bluemix.net:9093",
         "kafka02-prod01.messagehub.services.us-south.bluemix.net:9093",
         "kafka03-prod01.messagehub.services.us-south.bluemix.net:9093",
         "kafka04-prod01.messagehub.services.us-south.bluemix.net:9093",
         "kafka05-prod01.messagehub.services.us-south.bluemix.net:9093"
      ],
      "kafka_admin_url":"https://kafka-admin-prod01.messagehub.services.us-south.bluemix.net:443",
      "api_key":"Dv5kEVNNsbuJ9RFEKdUhIn2hruipIrsBolge6v1uQmTzEQti",
      "mqlight_lookup_url":"https://mqlight-lookup-prod01.messagehub.services.us-south.bluemix.net/Lookup?serviceId=5448397d-cb22-4698-8a2b-ffb04f43a4cb",
      "kafka_rest_url":"https://kafka-rest-prod01.messagehub.services.us-south.bluemix.net:443",
      "user":"Dv5kEVNNsbuJ9RFE",
      "password":"KdahIn2hruipIrsBolge6v1uQmTzEQti"
   },
   "target":{
      "topic":"soutput",
      "type":"kafka"
   }
}
```
{: codeblock}

Spark
서비스
신임 정보:

```
{
      "tenant_id": "s745-299dcf850a6390-35c9a7ecf27a",  
      "tenant_id_full": "ba3dde5a-ee64-4057-9749-299dcf850a63_4c55eb1c-d6fe-4f0a-9390-35c9a7ecf27a",  
      "cluster_master_url": "https://spark.bluemix.net",  
      "instance_id": "ba3dde5a-ee64-4057-9749-299dcf850a63",  
      "tenant_secret": "c0cba7a4-7b19-46e6-9326-44c4f48aaf08",  
      "plan": "ibm.SparkService.PayGoPersonal"
}
```
{: codeblock}

## 배치 세부사항 확보

배치의 결과로 리턴된 위치 필드를 사용하여 GET 요청으로
스트리밍 배치의 세부사항을 확인할 수 있습니다. 다음 정보를 참조하십시오. 

요청 예제: 

```
curl -i \
-X GET \
-H "Authorization: Bearer $token" \
https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/{published_model_id}/deployments/{deployment_id}
```
{: codeblock}

출력 예제: 

```
{
   "metadata":{
      "guid":"0f5e58ff-db0d-4d33-9a53-9c4e452cdfc2",
      "url":"https://ibm-watson-ml.mybluemix.net/v3/wml_instances/b5bd2368-cc4b-4cf1-aec0-49ed8daeb018/published_models/ceed18b3-ada2-42db-bf00-429f4e2c7601/deployments/0f5e58ff-db0d-4d33-9a53-9c4e452cdfc2",
      "created_at":"2017-06-28T12:39:01.474Z",
      "modified_at":"2017-06-28T12:39:04.274Z"
   },
   "entity":{
      "runtime_environment":"spark-2.0",
      "name":"Sentiment Prediction ",
      "instance_href":"https://ibm-watson-ml.mybluemix.net/v2/streaming/deployments/553f0ed7-b3fc-4659-ae45-c42c66cb5e04",
      "description":"Streaming Deployment",
      "published_model":{
         "author":{
            "name":"IBM",
            "email":""
         },
         "name":"Sentiment Prediction",
         "url":"https://ibm-watson-ml.mybluemix.net/v3/wml_instances/b5bd2368-cc4b-4cf1-aec0-49ed8daeb018/published_models/ceed18b3-ada2-42db-bf00-429f4e2c7601",
         "guid":"ceed18b3-ada2-42db-bf00-429f4e2c7601",
         "description":"Predicts comment sentiment about particular topic for marketing company.",
         "created_at":"2017-06-28T11:57:47.636Z"
      },
      "model_type":"sparkml-model-2.0",
      "status":"RUNNING",
      "output":{
         "connection":{
            "kafka_brokers_sasl":[
               "kafka01-prod01.messagehub.services.us-south.bluemix.net:9093",
               "kafka02-prod01.messagehub.services.us-south.bluemix.net:9093",
               "kafka03-prod01.messagehub.services.us-south.bluemix.net:9093",
               "kafka04-prod01.messagehub.services.us-south.bluemix.net:9093",
               "kafka05-prod01.messagehub.services.us-south.bluemix.net:9093"
            ],
            "kafka_admin_url":"https://kafka-admin-prod01.messagehub.services.us-south.bluemix.net:443",
            "api_key":"gDic7zCjvBJQjMjBMGcVHeBLI5VkqELVQsiuMx8Ka0XtddRk",
            "mqlight_lookup_url":"https://mqlight-lookup-prod01.messagehub.services.us-south.bluemix.net/Lookup?serviceId=2e9e624d-967b-4bf3-8eda-504e10fbc14d",
            "kafka_rest_url":"https://kafka-rest-prod01.messagehub.services.us-south.bluemix.net:443",
            "user":"gDic7zCjvBJQjMjB",
            "password":"MGcVHeBLI5VkqELVQsiuMx8Ka0XtddRk"
         },
         "target":{
            "topic":"streamingo",
            "type":"kafka"
         }
      },
      "type":"stream",
      "deployed_version":{
         "url":"https://ibm-watson-ml.mybluemix.net/v2/artifacts/models/ceed18b3-ada2-42db-bf00-429f4e2c7601/versions/848e632f-1ffc-4241-992b-6301686eadbd",
         "guid":"848e632f-1ffc-4241-992b-6301686eadbd",
         "created_at":"2017-06-28T11:57:10.997Z"
      },
      "spark_service":{
         "credentials":{
            "tenant_id":"s000-5f656e4e614595-e0ddb27c0670",
            "cluster_master_url":"https://spark.bluemix.net",
            "tenant_id_full":"fba311a7-532e-4aad-9000-5f656e4e6145_bd856e56-b4f4-4e82-9b95-e0ddb27c0670",
            "tenant_secret":"d9627f06-7a25-46ed-b833-93fe799654c4",
            "instance_id":"fba311a7-532e-4aad-9000-5f656e4e6145",
            "plan":"ibm.SparkService.PayGoPersonal"
         },
         "version":"2.0"
      },
      "input":{
         "connection":{
            "kafka_brokers_sasl":[
               "kafka01-prod01.messagehub.services.us-south.bluemix.net:9093",
               "kafka02-prod01.messagehub.services.us-south.bluemix.net:9093",
               "kafka03-prod01.messagehub.services.us-south.bluemix.net:9093",
               "kafka04-prod01.messagehub.services.us-south.bluemix.net:9093",
               "kafka05-prod01.messagehub.services.us-south.bluemix.net:9093"
            ],
            "kafka_admin_url":"https://kafka-admin-prod01.messagehub.services.us-south.bluemix.net:443",
            "api_key":"gDic7zCjvBJQjMjBMGcVHeBLI5VkqELVQsiuMx8Ka0XtddRk",
            "mqlight_lookup_url":"https://mqlight-lookup-prod01.messagehub.services.us-south.bluemix.net/Lookup?serviceId=2e9e624d-967b-4bf3-8eda-504e10fbc14d",
            "kafka_rest_url":"https://kafka-rest-prod01.messagehub.services.us-south.bluemix.net:443",
            "user":"gDic7zCjvBJQjMjB",
            "password":"MGcVHeBLI5VkqELVQsiuMx8Ka0XtddRk"
         },
         "source":{
            "topic":"streamingi",
            "type":"kafka"
         }
      }
   }
}
```
{: codeblock}

## 스트리밍 배치 중지

배치 결과의 위치 필드를 사용하여 다음과 같이 스트리밍 배치를 쉽게 중지할 수 있습니다. 

요청 예제: 

```
curl -i \
-X PATCH \
-H 'Content-Type: application/json' \
-H "Authorization: Bearer $access_token" \
-d '[{"op": "replace","path": "/status","value": "STOPPED"}]' \
https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/{published_model_id}/deployments/{deployment_id}
```
{: codeblock}

출력 예제: 

```
HTTP/1.1 204 No Content
X-Backside-Transport: OK OK
Connection: close
Cache-Control: no-cache, no-store, must-revalidate
Date: Wed, 28 Jun 2017 13:34:37 GMT
Pragma: no-cache
Server: nginx/1.11.5
X-Content-Type-Options: nosniff
X-Xss-Protection: 1; mode=block
X-Global-Transaction-ID: 2590068775
```
{: codeblock}

## 스트리밍 배치 시작

스트리밍 배치를 다음과 같이 쉽게 시작할 수 있습니다. 

요청 예제: 

```
curl -i \
-X PATCH \
-H 'Content-Type: application/json' \
-H "Authorization: Bearer $access_token" \
-d '[{"op": "replace","path": "/status","value": "RUNNING"}]' \
https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/{published_model_id}/deployments/{deployment_id}
```
{: codeblock}

출력 예제: 

```
HTTP/1.1 204 No Content
X-Backside-Transport: OK OK
Connection: close
Cache-Control: no-cache, no-store, must-revalidate
Date: Wed, 28 Jun 2017 13:35:39 GMT
Pragma: no-cache
Server: nginx/1.11.5
X-Content-Type-Options: nosniff
X-Xss-Protection: 1; mode=block
X-Global-Transaction-ID: 4242073343
```
{: codeblock}

## 스트리밍 배치 삭제

더 이상 스트리밍 배치가 필요하지 않은 경우 삭제할 수 있습니다. 

요청 예제: 

```
curl -i \
-X DELETE \
-H "Authorization: Bearer $access_token" \
https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/{published_model_id}/deployments/{deployment_id}
```
{: codeblock}

출력 예제: 

```
HTTP/1.1 204 No Content
X-Backside-Transport: OK OK
Connection: Keep-Alive
Cache-Control: no-cache, no-store, must-revalidate
Date: Wed, 28 Jun 2017 13:36:38 GMT
Pragma: no-cache
Server: nginx/1.11.5
X-Content-Type-Options: nosniff
X-Xss-Protection: 1; mode=block
X-Global-Transaction-ID: 2025130991
```
{: codeblock}
