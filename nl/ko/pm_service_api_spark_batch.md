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

# 일괄처리 모델 배치


**참고**: 이 기능은 현재 베타이며 Spark MLlib와 함께 사용하기 위한 경우에만 사용 가능합니다. 

**시나리오 이름**: 고객 만족도 예측

**시나리오 설명**: 통신사에서는 어떤 고객이 떠날 위험성이 있는지 알려고 합니다. 제시된 모델은 고객 이탈을 예측합니다. 데이터 과학자는 예측 모델을 개발하고 이를 사용자(개발자)와 공유합니다. 여러분의 태스크는
모델을 배치하고 배치된 모델에 대한 스코어 요청을 작성하여 예측 분석을 생성하는 것입니다. 

## 샘플 모델 사용

1. IBM® Watson™ Machine Learning 대시보드의 샘플 탭으로 이동하십시오. 

2. 샘플 모델 섹션에서 고객 만족도 예측 타일을 찾아서 모델 추가 단추(+)를 클릭하십시오. 

이제 모델 탭의 사용 가능한 모델 목록에서 샘플 고객 만족도 예측 모델을 보게 됩니다. 

## 액세스 토큰 생성

IBM Watson Machine Learning 서비스 인스턴스의 서비스 신임 정보 탭에서 사용 가능한 사용자 및
비밀번호를 사용하여 액세스 토큰을 생성하십시오. 

요청 예제: 

```
curl --basic --user username:password https://ibm-watson-ml.mybluemix.net/v3/identity/token
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

## Object Storage를 사용하여 일괄처리 배치 작성

예측 모델의 일괄처리 배치를 작성하기 위해 REST API 호출을 사용하려면, 다음 세부사항을 제공하십시오. 

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

*  Object Storage 세부사항은 모델에 대한 입력(스코어에 대한 고객 데이터) 및 모델 출력(이 경우 자동으로 작성되는
results.csv)에 대한 스토리지로 사용됩니다. 

*  배치를 작성하려면 다음 엔드포인트를 사용하십시오.  

   ```
   /v3/wml_instances/{instance_id}/published_models/{published_model_id}/deployments
   ```
   
   온라인 배치를 작성하는 데 필요한 엔드포인트는 WML 대시보드 -> 모델 세부사항 -> URL에서 사용 가능합니다. IBM Watson Machine Learning 대시보드에서 published_model_id 값을 찾으려면
모델 -> 세부사항 보기를 클릭하고 ID 필드에서 값을 복사하고 Watson Machine Learning 인스턴스의 VCAP 신임 정보에서 "instance_id"를 가져올 수 있습니다. 

요청 예제: 

```
curl -v -XPOST \
    -H "Content-Type:application/json" \
    -H "Authorization:Bearer $access_token" \
    -H "X-Spark-Service-Instance: $spark_credentials" \
    -d '{
      "name":"Customer Satisfaction Prediction",
      "type": "batch",
      "description": "Batch Deployment",
       "input":{
          "source":{
             "container":"batchjob",
             "filename":"TelcoCustomerData.csv",
             "fileformat":"csv",
             "inferschema":"1",
             "firstlineheader":"true",
             "type":"bluemixobjectstorage"
          },
          "connection":{
             "userid":"b2d83cf6056e040ddb91ca00a2686c7d3",
             "projectid":"252341ed707d4558b5b2da245e785cd7",
             "password":"eJ_y9R^OE{j?8Ub!!",
             "region":"dallas",
             "authurl":"https://identity.open.softlayer.com"
          }
       },
       "output":{
          "target":{
             "container":"batchjob",
             "filename":"result.csv",
             "fileformat":"csv",
             "writemode":"write",
             "firstlineheader":"true",
             "type":"bluemixobjectstorage"
          },
          "connection":{
             "userid":"b283cf6056e040ddb91ca00a2686c7d3",
             "projectid":"252341ed707d4558b5b2da245e785cd7",
             "password":"eJ_y9R^OE{j?8Ub!!",
             "region":"dallas",
             "authurl":"https://identity.open.softlayer.com"
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
      "guid":"60ad595a-2d53-4094-aeb0-be0273b3a8b1",
      "url":"https://ibm-watson-ml.mybluemix.net/v3/wml_instances/b5bd2368-cc4b-4cf1-aec0-49ed8daeb018/published_models/315231e8-e021-40dc-a250-6e4f9d1c11d7/deployments/60ad595a-2d53-4094-aeb0-be0273b3a8b1",
      "created_at":"2017-06-28T11:39:26.663Z",
      "modified_at":"2017-06-28T11:39:48.637Z"
   },
   "entity":{
      "runtime_environment":"spark-2.0",
      "name":"Customer Satisfaction Prediction",
      "instance_href":"https://ibm-watson-ml.mybluemix.net/v2/batch/deployments/8e7ffee3-700c-4bbf-acfa-ccc0ad299dd7",
      "description":"Batch Deployment",
      "published_model":{
         "author":{
            "name":"IBM",
            "email":""
         },
         "name":"Customer Satisfaction Prediction",
         "url":"https://ibm-watson-ml.mybluemix.net/v3/wml_instances/b5bd2368-cc4b-4cf1-aec0-49ed8daeb018/published_models/315231e8-e021-40dc-a250-6e4f9d1c11d7",
         "guid":"315231e8-e021-40dc-a250-6e4f9d1c11d7",
         "description":"Predicts Telco customer churn.",
         "created_at":"2017-06-28T11:39:26.642Z"
      },
      "model_type":"sparkml-model-2.0",
      "status":"INITIALIZING",
      "output":{
         "target":{
            "fileformat":"csv",
            "firstlineheader":"true",
            "writemode":"write",
            "container":"batchjob",
            "filename":"customerOutput.csv",
            "type":"bluemixobjectstorage"
         },
         "connection":{
            "projectid":"cabb5f0c377d4ed080f917dc29605aad",
            "userid":"1d5815d49ea4479ab6f29cc1733db774",
            "region":"dallas",
            "authurl":"https://identity.open.softlayer.com",
            "password":"UF.D_/p9SKx5YPxw"
         }
      },
      "type":"batch",
      "deployed_version": {
         "url":"https://ibm-watson-ml.mybluemix.net/v2/artifacts/models/315231e8-e021-40dc-a250-6e4f9d1c11d7/versions/e52d8fac-c62c-4ace-9b86-afbbc85ea25f",
         "guid":"e52d8fac-c62c-4ace-9b86-afbbc85ea25f",
         "created_at":"2017-06-28T11:38:21.121Z"
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
         "source":{
            "fileformat":"csv",
            "firstlineheader":"true",
            "container":"batchjob",
            "inferschema":"1",
            "filename":"customerInput.csv",
            "type":"bluemixobjectstorage"
         },
         "connection":{
            "projectid":"cabb5f0c377d4ed080f917dc29605aad",
            "userid":"1d5815d49ea4479ab6f29cc1733db774",
            "region":"dallas",
            "authurl":"https://identity.open.softlayer.com",
            "password":"UF.D_/p9SKx5YPxw"
         }
      }
   }
}
```
{: codeblock}

**참고**: 대시보드를 사용하여 일괄처리 배치를 작성할 수도 있습니다. 다음 세 가지 항목을 입력해야 합니다. 

참고: 양식 예제: 

입력:

```
{
   "source":{
      "fileformat":"csv",
      "firstlineheader":"true",
      "container":"batchjob",
      "inferschema":"1",
      "filename":"TelcoCustomerData.csv",
      "type":"bluemixobjectstorage"
   },
   "connection":{
      "projectid":"252341ed707d4558b5b2da245e785cd7",
      "userid":"b2d83cf6056e040ddb91ca00a2686c7d3",
      "region":"dallas",
      "authurl":"https://identity.open.softlayer.com",
      "password":"eJ_y9R^OE{j?8Ub!!"
   }
}
```
{: codeblock}

출력:

```
{
   "target":{
      "fileformat":"csv",
      "firstlineheader":"true",
      "container":"batchjob",
      "inferschema":"1",
      "filename":"result.csv",
      "type":"bluemixobjectstorage"
   },
   "connection":{
      "projectid":"252341ed707d4558b5b2da245e785cd7",
      "userid":"b2d83cf6056e040ddb91ca00a2686c7d3",
      "region":"dallas",
      "authurl":"https://identity.open.softlayer.com",
      "password":"eJ_y9R^OE{j?8Ub!!"
   }
}
```
{: codeblock}

Spark 서비스 신임 정보:

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

배치 모델과 관련된 상태, 엔드포인트 href 및 매개변수를 확인할 수 있습니다. 

요청 예제: 

```
curl -v -XGET -H "Content-Type:application/json" -H "Authorization:Bearer $access_token" https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/{published_model_id}/deployments/{deployment_id}
```
{: codeblock}

출력 예제: 

```
{
   "metadata":{
      "guid":"60ad595a-2d53-4094-aeb0-be0273b3a8b1",
      "url":"https://ibm-watson-ml.mybluemix.net/v3/wml_instances/b5bd2368-cc4b-4cf1-aec0-49ed8daeb018/published_models/315231e8-e021-40dc-a250-6e4f9d1c11d7/deployments/60ad595a-2d53-4094-aeb0-be0273b3a8b1",
      "created_at":"2017-06-28T11:39:26.663Z",
      "modified_at":"2017-06-28T11:39:48.637Z"
   },
   "entity":{
      "runtime_environment":"spark-2.0",
      "name":"Customer Satisfaction Prediction",
      "instance_href":"https://ibm-watson-ml.mybluemix.net/v2/batch/deployments/8e7ffee3-700c-4bbf-acfa-ccc0ad299dd7",
      "description":"Batch Deployment",
      "published_model":{
         "author":{
            "name":"IBM",
            "email":""
         },
         "name":"Customer Satisfaction Prediction",
         "url":"https://ibm-watson-ml.mybluemix.net/v3/wml_instances/b5bd2368-cc4b-4cf1-aec0-49ed8daeb018/published_models/315231e8-e021-40dc-a250-6e4f9d1c11d7",
         "guid":"315231e8-e021-40dc-a250-6e4f9d1c11d7",
         "description":"Predicts Telco customer churn.",
         "created_at":"2017-06-28T11:39:26.642Z"
      },
      "model_type":"sparkml-model-2.0",
      "status":"COMPLETED",
      "output":{
         "target":{
            "fileformat":"csv",
            "firstlineheader":"true",
            "writemode":"write",
            "container":"batchjob",
            "filename":"customerOutput.csv",
            "type":"bluemixobjectstorage"
         },
         "connection":{
            "projectid":"cabb5f0c377d4ed080f917dc29605aad",
            "userid":"1d5815d49ea4479ab6f29cc1733db774",
            "region":"dallas",
            "authurl":"https://identity.open.softlayer.com",
            "password":"UF.D_/p9SKx5YPxw"
         }
      },
      "type":"batch",
      "deployed_version":{
         "url":"https://ibm-watson-ml.mybluemix.net/v2/artifacts/models/315231e8-e021-40dc-a250-6e4f9d1c11d7/versions/e52d8fac-c62c-4ace-9b86-afbbc85ea25f",
         "guid":"e52d8fac-c62c-4ace-9b86-afbbc85ea25f",
         "created_at":"2017-06-28T11:38:21.121Z"
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
         "source":{
            "fileformat":"csv",
            "firstlineheader":"true",
            "container":"batchjob",
            "inferschema":"1",
            "filename":"customerInput.csv",
            "type":"bluemixobjectstorage"
         },
         "connection":{
            "projectid":"cabb5f0c377d4ed080f917dc29605aad",
            "userid":"1d5815d49ea4479ab6f29cc1733db774",
            "region":"dallas",
            "authurl":"https://identity.open.softlayer.com",
            "password":"UF.D_/p9SKx5YPxw"
         }
      }
   }
}
```
{: codeblock}

예측 결과가 IBM Object Storage의 .csv 파일에 저장됩니다. 다음은 샘플 행입니다. 

입력 파일
미리보기:

```
customerID,gender,SeniorCitizen,Partner,Dependents,tenure,PhoneService,MultipleLines,InternetService,OnlineSecurity,OnlineBackup,DeviceProtection,TechSupport,StreamingTV,StreamingMovies,Contract,PaperlessBilling,PaymentMethod,MonthlyCharges,TotalCharges,Churn
7590-VHVEG,Female,0,Yes,No,1,No,No phone service,DSL,No,Yes,No,No,No,No,Month-to-month,Yes,Electronic check,29.85,29.85,No
5575-GNVDE,Male,0,No,No,34,Yes,No,DSL,Yes,No,Yes,No,No,No,One year,No,Mailed check,56.95,1889.5,No
3668-QPYBK,Male,0,No,No,2,Yes,No,DSL,Yes,Yes,No,No,No,No,Month-to-month,Yes,Mailed check,53.85,108.15,Yes
```
{: codeblock}

출력 파일 미리보기:

```
InternetService, Contract, tenure, MonthlyCharges, Churn
Fiber optic, Month-to-month, 2, 70.7, 1
DSL, Two year, 58, 59.9, 0
DSL, Month-to-month, 1, 30.2, 1
DSL, Month-to-month, 17, 64.7, 1
DSL, Month-to-month, 13, 76.2, 0
DSL, Month-to-month, 25, 69.5, 1
Fiber optic, Month-to-month, 8, 80.65, 1
No, One year, 52, 20.4, 0
Fiber optic, Two year, 64, 111.6, 0
Fiber optic, Month-to-month, 1, 79.35, 1
```
{: codeblock}

## 일괄처리 배치 삭제

배치가 더 이상 필요하지 않은 경우 다음 샘플과 같은 쿼리를 사용하여 배치를 삭제할 수 있습니다. 

요청 예제: 

```
curl -v -XDELETE -H "Content-Type:application/json" -H
"Authorization:Bearer $access_token" https://ibm-watson-
ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/{published_model_id}/deployments/{deployment_id}
```
{: codeblock}

출력 예제: 

```
HTTP/1.1 204 No Content
X-Backside-Transport: OK OK
Connection: Keep-Alive
Cache-Control: no-cache, no-store, must-revalidate
Date: Wed, 28 Jun 2017 11:54:19 GMT
Pragma: no-cache
Server: nginx/1.11.5
X-Content-Type-Options: nosniff
X-Xss-Protection: 1; mode=block
X-Global-Transaction-ID: 1600446575
```
{: codeblock}
