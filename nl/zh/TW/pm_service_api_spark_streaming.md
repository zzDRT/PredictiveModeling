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

# 部署串流模型


**附註**：此功能目前為測試版，只提供搭配 Spark MLlib 使用。

**情境名稱**：觀感分析。

**情境說明**：行銷機構想要瞭解有關特定主題的觀感。該機構想要我們開發出一種模型，以將給定的表達方式分類為 POSITIVE 或 NEGATIVE。資料科學家準備預測模型，並與身為開發人員的您分享。您的工作是部署模型，然後對已配置的模型提出評分要求，以產生預測分析模型。

如需相關資訊，請參閱此文件。

## 使用範例模型

1. 移至「IBM® Watson™ Machine Learning 儀表板」的「範例」標籤。

2. 在「範例模型」區段中，尋找「觀感預測」磚，然後按一下「新增模型」按鈕 (+)。

現在您會在模型標籤上看到範例「觀感預測」模型列在可用的模型清單中。

## 產生存取記號

使用 IBM Watson Machine Learning 服務實例的「服務認證」標籤上所提供之使用者和密碼，產生存取記號。

要求範例：

```
curl --basic --user username:password https://ibm-watson-ml.mybluemix.net/v2/identity/token
```
{: codeblock}

輸出範例：

```
{"token":"**********"}
```
{: codeblock}

請使用下列終端機指令來指派記號值給 access_token 環境變數：

```
access_token="<token_value>"
```
{: codeblock}

## 使用 IBM Message Hub 來建立串流部署

若要使用 REST API 呼叫來建立預測模型的串流部署，請提供下列詳細資料：

*  在前一個步驟中建立的存取記號

*  Spark 服務認證（可以在 Bluemix Spark 服務儀表板的「服務認證」標籤上找到）。在提出部署要求之前，必須先將 Spark 認證解碼為 base64，並傳入 curl 要求的標頭中，成為 X-Spark-Service-Instance。

   視您正在使用的作業系統而定，您必須發出下列其中一個終端機指令來執行 base64 解碼，並將其指派至環境變數。

   在 macOS 作業系統上，請使用下列指令：

   ```
   spark_credentials=$(echo '{"credentials": {"tenant_id": "s068-ade10277b64956-05b1d10fv12b","tenant_id_full": "00fd89e6-8cf2-4712-a068-ade10277b649_41f37bf2-1b95-4c65-a156-05b1d10fb12b","cluster_master_url": "https://spark.bluemix.net","instance_id": "00fd89e6-8cf2-4712-a068-ade10277b649","tenant_secret": "c74c37cf-482a-4da4-836e-f32ca26ccbb9","plan": "ibm.SparkService.PayGoPersonal"},"version": "2.0"}' | base64)
   ```
{: codeblock}

   在 Microsoft Windows 或 Linux 作業系統上，您必須使用 `--wrap=0` 參數與 `base64` 指令來執行 base64 解碼：

   ```
   spark_credentials=$(echo '{"credentials": {"tenant_id": "s068-ade10277b64956-05b1d10fv12b","tenant_id_full": "00fd89e6-8cf2-4712-a068-ade10277b649_41f37bf2-1b95-4c65-a156-05b1d10fb12b","cluster_master_url": "https://spark.bluemix.net","instance_id": "00fd89e6-8cf2-4712-a068-ade10277b649","tenant_secret": "c74c37cf-482a-4da4-836e-f32ca26ccbb9","plan": "ibm.SparkService.PayGoPersonal"},"version": "2.0"}' | base64 --wrap=0)
   ```
{: codeblock}

*  IBM Message Hub 主題詳細資料，將會用來做為模型的的輸入（推文），以及模型輸出（預測結果）儲存空間。

*  若要建立部署，請使用下列端點：
   
   ```
   /v2/published_models/{published_model_id}/deployments
   ```
   
   建立線上部署所需的端點提供於您的「WML 儀表板」->「模型詳細資料」-> URL。另外也請注意，您可以從 IBM Watson Machine Learning 儀表板找到 published_model_id 值，按一下「模型」->「檢視詳細資料」，然後複製 ID 欄位中的值，您就可以從 Watson Machine Learning 實例的 VCAP 認證取得 "instance_id"。

要求範例：

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

輸出範例：

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

**附註**：您也可以使用該儀表板來建立串流部署。您必須提供下列三項輸入：

*  輸入來源詳細資料，以供模型使用。您的輸入來源必須已上傳，並且準備好進行汲取 (IBM Message Hub)。

*  輸出來源詳細資料，結果將會在這裡寫入。您的輸出來源必須已建立，並準備好在模型處理輸入來源的資料時，接收結果。

*  Spark 服務認證。

表單範例：

輸入：

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

輸出：

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

Spark 服務認證：

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

## 取得部署詳細資料

您可以使用部署結果傳回的位置欄位，透過 GET 要求來檢查串流部署。請參閱下列資訊。

要求範例：

```
curl -i \
-X GET \
-H "Authorization: Bearer $token" \
https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/{published_model_id}/deployments/{deployment_id}
```
{: codeblock}

輸出範例：

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

## 停止串流部署

同樣地，您可以使用部署結果傳回的「位置」欄位，輕鬆地停止串流部署，如下所示。

要求範例：

```
curl -i \
-X PATCH \
-H 'Content-Type: application/json' \
-H "Authorization: Bearer $access_token" \
-d '[{"op": "replace","path": "/status","value": "STOPPED"}]' \
https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/{published_model_id}/deployments/{deployment_id}
```
{: codeblock}

輸出範例：

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

## 開始串流部署

您可以輕鬆地開始串流部署，如下所示。

要求範例：

```
curl -i \
-X PATCH \
-H 'Content-Type: application/json' \
-H "Authorization: Bearer $access_token" \
-d '[{"op": "replace","path": "/status","value": "RUNNING"}]' \
https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/{published_model_id}/deployments/{deployment_id}
```
{: codeblock}

輸出範例：

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

## 刪除串流部署

您可以刪除不再需要的串流部署。

要求範例：

```
curl -i \
-X DELETE \
-H "Authorization: Bearer $access_token" \
https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/{published_model_id}/deployments/{deployment_id}
```
{: codeblock}

輸出範例：

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
