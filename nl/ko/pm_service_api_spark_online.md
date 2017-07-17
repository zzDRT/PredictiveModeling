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

# 온라인 모델 배치


**시나리오 이름**: 제품 라인 예측

**시나리오 설명**: 아웃도어 장비를 판매하는 회사가 자사 제품 라인에서 고객 관심사항을 예측하는 모델을 구축하고자 합니다. 
데이터 과학자는 예측 모델을 준비하고 이를 사용자(개발자)와 공유합니다. 여러분의 태스크는
모델을 배치하고 배치된 모델에 대한 스코어 요청을 작성하여 고객 관심사항의 예측을 생성하는 것입니다. 

## 샘플 모델 사용

1. IBM® Watson™ Machine Learning 대시보드의 샘플 탭으로 이동하십시오. 

2. 샘플 모델 섹션에서 제품 라인 예측 타일을 찾고
모델 추가 단추(+)를 클릭하십시오. 

이제 모델 탭의
사용 가능한 모델 목록에 샘플 제품 라인 예측 모델이 표시됩니다. 

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

## 온라인 배치 작성

예측 모델의 온라인 배치를 작성하려면 다음 API 호출을 사용하십시오. 애플리케이션의 스코어링 데이터에 대해 사용할 수 있습니다. 온라인 배치를 작성하는 데 필요한 엔드포인트는 WML 대시보드 -> 모델 세부사항 -> URL에서 사용 가능합니다. Id 필드에 있는 IBM Watson Machine Learning 대시보드의 모델 세부사항 페이지에서 "published_model_id" 값을 찾을 수 있고
Watson Machine Learning 인스턴스의 VCAP 신임 정보에서 "instance_id"를 가져올 수 있습니다. 

요청 예제: 

```
curl -X POST --header "Content-Type: application/json" --header "Accept: application/json" --header "Authorization: token" -d '{"name": "Product Line Prediction", "description": "Product Line Prediction Deployment", "type": "online"}' 'https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/{published_model_id}/deployments'
```
{: codeblock}

출력 예제: 

```
{  
   "metadata":{
      "guid":"b97072ec-3ef8-4705-a1e7-c264e270e49a",
      "url":"https://ibm-watson-ml.mybluemix.net/v3/wml_instances/21208fa4-f5b8-4fb7-b162-878e455e4be2/published_models/1ab15cdb-4f9e-4d35-8077-c0f6fff10196/deployments/b97072ec-3ef8-4705-a1e7-c264e270e49a",
      "created_at":"2017-06-27T13:47:49.534Z",
      "modified_at":"2017-06-27T13:47:58.347Z"
   },
   "entity":{  
      "runtime_environment":"spark-2.0",
      "name":"Product Line Prediction TMNL",
      "instance_href":"/v2/scoring/online/jobs/b97072ec-3ef8-4705-a1e7-c264e270e49a",
      "scoring_url":"https://ibm-watson-ml.mybluemix.net/v3/wml_instances/21208fa4-f5b8-4fb7-b162-878e455e4be2/published_models/1ab15cdb-4f9e-4d35-8077-c0f6fff10196/deployments/b97072ec-3ef8-4705-a1e7-c264e270e49a/online",
      "description":"Product Line Prediction Deployment",
      "published_model":{  
         "author":{  
            "name":"IBM",
            "email":""
         },
         "name":"Product Line Prediction",
         "url":"https://ibm-watson-ml.mybluemix.net/v3/wml_instances/21208fa4-f5b8-4fb7-b162-878e455e4be2/published_models/1ab15cdb-4f9e-4d35-8077-c0f6fff10196",
         "guid":"1ab15cdb-4f9e-4d35-8077-c0f6fff10196",
         "description":"Predicts clients' interests in terms of sport product lines for chain stores in Europe.",
         "created_at":"2017-06-27T11:54:24.170Z"
      },
      "model_type":"sparkml-model-2.0",
      "status":"INITIALIZING",
      "type":"online",
      "deployed_version":{  
         "url":"https://ibm-watson-ml.mybluemix.net/v2/artifacts/models/1ab15cdb-4f9e-4d35-8077-c0f6fff10196/versions/3ab34346-f195-4ee4-b378-1204f674a725",
         "guid":"3ab34346-f195-4ee4-b378-1204f674a725",
         "created_at":"2017-06-27T11:54:07.507Z"
      }
   }
}
```
{: codeblock}

## 배치 세부사항 확보

배치 모델과 관련된 상태, 스코어링 엔드포인트 주소(scoringHref) 및 매개변수를 확인할 수 있습니다. 

요청 예제: 

```
curl -X GET --header "Content-Type: application/json" --header "Accept: application/json" --header "Authorization: $token" 'https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/{published_model_id}/deployments/{deployment_id}'
```
{: codeblock}

출력 예제: 

```
{  
   "metadata":{
      "guid":"b97072ec-3ef8-4705-a1e7-c264e270e49a",
      "url":"https://ibm-watson-ml.mybluemix.net/v3/wml_instances/21208fa4-f5b8-4fb7-b162-878e455e4be2/published_models/1ab15cdb-4f9e-4d35-8077-c0f6fff10196/deployments/b97072ec-3ef8-4705-a1e7-c264e270e49a",
      "created_at":"2017-06-27T13:47:49.534Z",
      "modified_at":"2017-06-27T13:47:58.347Z"
   },
   "entity":{
      "runtime_environment":"spark-2.0",
      "name":"Product Line Prediction TMNL",
      "instance_href":"/v2/scoring/online/jobs/b97072ec-3ef8-4705-a1e7-c264e270e49a",
      "scoring_url":"https://ibm-watson-ml.mybluemix.net/v3/wml_instances/21208fa4-f5b8-4fb7-b162-878e455e4be2/published_models/1ab15cdb-4f9e-4d35-8077-c0f6fff10196/deployments/b97072ec-3ef8-4705-a1e7-c264e270e49a/online",
      "description":"Product Line Prediction Deployment",
      "published_model":{
         "author":{
            "name":"IBM",
            "email":""
         },
         "name":"Product Line Prediction",
         "url":"https://ibm-watson-ml.mybluemix.net/v3/wml_instances/21208fa4-f5b8-4fb7-b162-878e455e4be2/published_models/1ab15cdb-4f9e-4d35-8077-c0f6fff10196",
         "guid":"1ab15cdb-4f9e-4d35-8077-c0f6fff10196",
         "description":"Predicts clients' interests in terms of sport product lines for chain stores in Europe.",
         "created_at":"2017-06-27T11:54:24.170Z"
      },
      "model_type":"sparkml-model-2.0",
      "status":"ACTIVE",
      "type":"online",
      "deployed_version":{
         "url":"https://ibm-watson-ml.mybluemix.net/v2/artifacts/models/1ab15cdb-4f9e-4d35-8077-c0f6fff10196/versions/3ab34346-f195-4ee4-b378-1204f674a725",
         "guid":"3ab34346-f195-4ee4-b378-1204f674a725",
         "created_at":"2017-06-27T11:54:07.507Z"
      }
   }
}
```
{: codeblock}

## 스코어 요청 작성

스코어링 엔드포인트(scoringHref)가 작성되었으므로 이제 스코어 요청을 작성하여 예측을
생성할 수 있습니다. 이 시나리오에서는 고객 레코드가 예측 모델에 전달되며 스포츠 제품 예측이 리턴됩니다. 

샘플 레코드 헤더:

```
GENDER,AGE,MARITAL_STATUS,PROFESSION
```
{: codeblock}

샘플 레코드:

```
M,27,Single,Professional
F,56,Unspecified,Hospitality
M,45,Married,Retired
```
{: codeblock}

요청 예제: 

```
curl -X POST --header 'Content-Type: application/json' --header 'Accept: application/json' --header 'Authorization: token' -d '{"fields": ["GENDER","AGE","MARITAL_STATUS","PROFESSION"],"values": [["M",23,"Single","Student"],["M",55,"Single","Executive"]]}' 'https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}//published_models/{published_model_id}/deployments/{deployment_id}/online'
```
{: codeblock}

출력 예제: 

```
{
   "fields":[
      "GENDER",
      "AGE",
      "MARITAL_STATUS",
      "PROFESSION",
      "PROFESSION_IX",
      "GENDER_IX",
      "MARITAL_STATUS_IX",
      "features",
      "rawPrediction",
      "probability",
      "prediction",
      "predictedLabel"
   ],
   "records":[
      [
         "M",
         23,
         "Single",
         "Student",
         6.0,
         0.0,
         1.0,
         [
            0.0,
            23.0,
            1.0,
            6.0
         ],
         [
            5.600716147152702,
            6.482458372136143,
            6.048004170730676,
            0.20929155492307386,
            1.6595297550574055
         ],
         [
            0.2800358073576351,
            0.32412291860680714,
            0.3024002085365338,
            0.010464577746153694,
            0.08297648775287028
         ],
         1.0,
         "Personal Accessories"
      ],
      [
         "M",
         55,
         "Single",
         "Executive",
         3.0,
         0.0,
         1.0,
         [
            0.0,
            55.0,
            1.0,
            3.0
         ],
         [
            6.227653744886563,
            4.325198862164969,
            8.031953760146816,
            1.2319759269281225,
            0.1832177058735289
         ],
         [
            0.3113826872443282,
            0.2162599431082485,
            0.40159768800734086,
            0.06159879634640614,
            0.009160885293676447
         ],
         2.0,
         "Mountaineering Equipment"
      ]
   ]
}
```
{: codeblock}

예를 들어, 55세인 경영자는 등산 장비에 관심을 갖고 있으며
23세인 학생은 개인 액세서리에 관심을 갖고 있음을 알 수 있습니다. 
