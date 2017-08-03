---

copyright:
  years: 2016, 2017
lastupdated: "2017-08-03"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Deploying streaming models <span class='tag--beta'>Beta</span>


**Note**: This functionality is currently in beta and only available
for use with Spark MLlib. If you're interested in participating, add yourself to the wait list! For more information, see: [https://www.ibm.biz/mlwaitlist](https://www.ibm.biz/mlwaitlist).

**Scenario name**: Sentiment Analysis.

**Scenario description**: A marketing agency wants to know the
sentiment about a particular topic. The agency would like us to
develop a model that classifies a given statement as POSITIVE or
NEGATIVE. A Data Scientist prepares a predictive model and shares
it with you (the developer). Your task is to deploy the model and
generate predictive analytics by making score requests against
the deployed model.

See this document for more information.

## Using the sample model

1. Go to the Samples tab of the IBM® Watson™ Machine Learning
   Dashboard.

2. In the Sample Models section, find the Sentiment Prediction
   tile and click the Add model button (+).

Now you'll see the sample Sentiment Prediction model in the list
of available models on the Models tab.

## Generating an access token

Generate an access token by using the user and password available
from the Service Credentials tab of the IBM Watson Machine
Learning service instance.

Request example:

```
curl --basic --user username:password https://ibm-watson-ml.mybluemix.net/v3/identity/token
```
{: codeblock}

Output example:

```
{"token":"**********"}
```
{: codeblock}

Use the following terminal command to assign your token value to
the environment variable token:

```
token="<token_value>"
```
{: codeblock}

## Working with published models
Use the following API call to get your instance details, such as:
* published models `url`
* deployments `url`
* usage information

Request example:

```
curl -X GET --header "Content-Type: application/json" --header "Accept: application/json" --header "Authorization: $token" https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}
```
{: codeblock}

Output example:

```
{
   "metadata":{
      "guid":"87452a37-6a8f-4d59-bf88-59c66b5463e4",
      "url":"https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}",
      "created_at":"2017-06-23T08:31:52.026Z",
      "modified_at":"2017-06-23T08:31:52.026Z"
   },
   "entity":{
      "source":"Bluemix",
      "published_models":{
         "url":"https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models"
      },
      "usage":{ },
      "plan_id":"5325f63a-683a-47f0-a04e-97e371385588",
      "account_id":"b56398ea52f470c3173f4cf3bef5cc7e",
      "status":"Active",
      "organization_guid":"3e658178-a60c-48b8-8be9-bf58cc821656",
      "region":"us-south",
      "deployments":{
         "url":"https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}}/deployments"
      },
      "space_guid":"c3ea6205-b895-48ad-bb55-6786bc712c24",
      "plan":"free"
   }
}
```
{: codeblock}


Having **published_models** `url` use the following API call to get model's details:

Request example:

```
curl -X GET --header "Content-Type: application/json" --header "Accept: application/json" --header "Authorization: $token" https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/
```
{: codeblock}

Output example:

```
{
   "count":1,
   "resources":[
      {
         "metadata":{
            "guid":"f96d7e00-cd2d-40d1-9b9e-730efaa5dbe5",
            "url":"https://ibm-watson-ml.stage1.mybluemix.net/v3/wml_instances/7a0f9c88-3cf6-4433-89ee-92a641f26e89/published_models/f96d7e00-cd2d-40d1-9b9e-730efaa5dbe5",
            "created_at":"2017-07-11T10:51:10.900Z",
            "modified_at":"2017-07-11T10:51:11.012Z"
         },
         "entity":{
            "runtime_environment":"spark-2.0",
            "author":{
               "name":"IBM",
               "email":""
            },
            "name":"Sentiment Prediction",
            "description":"Predicts comment sentiment about particular topic for marketing company.",
            "label_col":"sentiment",
            "training_data_schema":{
               "type":"struct",
               "fields":[
                  {
                     "metadata":{

                     },
                     "type":"integer",
                     "name":"id",
                     "nullable":true
                  },
                  {
                     "metadata":{

                     },
                     "type":"string",
                     "name":"text",
                     "nullable":true
                  },
                  {
                     "metadata":{

                     },
                     "type":"string",
                     "name":"label",
                     "nullable":true
                  }
               ]
            },
            "latest_version":{
               "url":"https://ibm-watson-ml.stage1.mybluemix.net/v2/artifacts/models/f96d7e00-cd2d-40d1-9b9e-730efaa5dbe5/versions/0bb2bfd2-418d-4458-892b-1d9c02e56686",
               "guid":"0bb2bfd2-418d-4458-892b-1d9c02e56686",
               "created_at":"2017-07-11T10:51:11.012Z"
            },
            "model_type":"sparkml-model-2.0",
            "deployments":{
               "count":0,
               "url":"https://ibm-watson-ml.stage1.mybluemix.net/v3/wml_instances/7a0f9c88-3cf6-4433-89ee-92a641f26e89/published_models/f96d7e00-cd2d-40d1-9b9e-730efaa5dbe5/deployments"
            },
            "input_data_schema":{
               "type":"struct",
               "fields":[
                  {
                     "metadata":{

                     },
                     "type":"integer",
                     "name":"id",
                     "nullable":true
                  },
                  {
                     "metadata":{

                     },
                     "type":"string",
                     "name":"text",
                     "nullable":true
                  }
               ]
            }
         }
      }
   ]
}
```
{: codeblock}

Please note **deployments** `url` that is needed to create batch deployment in next step.


## Creating a streaming deployment with IBM Message Hub

To use a REST API call to create a streaming deployment of your
predictive model, provide the following details:

*  The access token created in the previous step

*  Spark service credentials, which can be found on the Service
   Credentials tab of the Bluemix Spark service dashboard. Before
   making the deployment request, Spark credentials must be
   decoded as base64 and passed in the header of a curl request
   as X-Spark-Service-Instance.

   Depending on the operating system that you are using, you must issue one of the following terminal commands to perform base64 decoding and assign it to the environment variable.

   On the macOS operating system, use the following command:

   ```
   spark_credentials=$(echo '{"credentials": {"tenant_id": "s068-ade10277b64956-05b1d10fv12b","tenant_id_full": "00fd89e6-8cf2-4712-a068-ade10277b649_41f37bf2-1b95-4c65-a156-05b1d10fb12b","cluster_master_url": "https://spark.bluemix.net","instance_id": "00fd89e6-8cf2-4712-a068-ade10277b649","tenant_secret": "c74c37cf-482a-4da4-836e-f32ca26ccbb9","plan": "ibm.SparkService.PayGoPersonal"},"version": "2.0"}' | base64)
   ```
   {: codeblock}

   On the Microsoft Windows or Linux operating systems, you must use the `--wrap=0` parameter with the `base64` command to perform base64 decoding:

   ```
   spark_credentials=$(echo '{"credentials": {"tenant_id": "s068-ade10277b64956-05b1d10fv12b","tenant_id_full": "00fd89e6-8cf2-4712-a068-ade10277b649_41f37bf2-1b95-4c65-a156-05b1d10fb12b","cluster_master_url": "https://spark.bluemix.net","instance_id": "00fd89e6-8cf2-4712-a068-ade10277b649","tenant_secret": "c74c37cf-482a-4da4-836e-f32ca26ccbb9","plan": "ibm.SparkService.PayGoPersonal"},"version": "2.0"}' | base64 --wrap=0)
   ```
   {: codeblock}

*  IBM Message Hub topic details which will be used as input
   (tweets) for the model and storage for the model output
   (prediction results).

*  To create a deployment, use the **deployments** `url` from previous section.

Request example:

```
curl -i \
-X POST \
-H 'Content-Type: application/json' \
-H "Authorization: $token" \
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

Output example:

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

**Note**: You can also use the Dashboard to create a streaming
deployment.


## Obtaining deployment details

You can check the details of your streaming deployment with a GET
request by using the using **metadata** `url` returned as a result of your
deployment. See the following information.

Request example:

```
curl -i \
-X GET \
-H "Authorization: $token" \
https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/{published_model_id}/deployments/{deployment_id}
```
{: codeblock}

Output example:

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

## Stopping a streaming deployment

Again, by using the Location field from a deployment result, you
can easily stop a streaming deployment as follows.

Request example:

```
curl -i \
-X PATCH \
-H 'Content-Type: application/json' \
-H "Authorization: $token" \
-d '[{"op": "replace","path": "/status","value": "STOPPED"}]' \
https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/{published_model_id}/deployments/{deployment_id}
```
{: codeblock}

Output example:

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

## Starting the streaming deployment

You can easily start a streaming deployment as follows.

Request example:

```
curl -i \
-X PATCH \
-H 'Content-Type: application/json' \
-H "Authorization: $token" \
-d '[{"op": "replace","path": "/status","value": "RUNNING"}]' \
https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/{published_model_id}/deployments/{deployment_id}
```
{: codeblock}

Output example:

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

## Deleting the streaming deployment

You can delete a streaming deployment if it's no longer needed.

Request example:

```
curl -i \
-X DELETE \
-H "Authorization: $token" \
https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/{published_model_id}/deployments/{deployment_id}
```
{: codeblock}

Output example:

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
