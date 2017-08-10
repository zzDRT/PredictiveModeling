---

copyright:
  years: 2016, 2017
lastupdated: "2017-08-09"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Deploying online models


**Scenario name**: Product line prediction.

**Scenario description**: A company that sells outdoor equipment
wants to build a model that predicts client interest in their
product line. A data scientist prepared a predictive model and
shares it with you (the developer). Your task is to deploy the
model and generate predictions of customer interest by making
score requests against the deployed model.

## Using the sample model

1. Go to the Samples tab of the IBM® Watson™ Machine Learning
   Dashboard.

2. In the Sample Models section, find the Product Line Prediction
   tile and click the Add model button (+).

Now you'll see the sample Product Line Prediction model in the
list of available models on the Models tab.

## Generating the access token

Generate an access token using the `user` and `password` available on
the **Service Credentials** tab of the IBM Watson Machine Learning
service instance.

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
            "guid":"7715dfcc-3005-4bc2-8bee-58ebdc9a43f3",
            "url":"https://ibm-watson-ml-dev.stage1.mybluemix.net/v3/wml_instances/{instance_id}/published_models/{publishepublished_model_id}",
            "created_at":"2017-07-10T12:45:56.623Z",
            "modified_at":"2017-07-10T12:45:56.710Z"
         },
         "entity":{
            "runtime_environment":"spark-2.0",
            "author":{
               "name":"IBM",
               "email":""
            },
            "name":"Product Line Prediction",
            "description":"Predicts clients' interests in terms of sport product lines for chain stores in Europe.",
            "label_col":"PRODUCT_LINE",
            "training_data_schema":{ },
            "latest_version":{ },
            "model_type":"sparkml-model-2.0",
            "deployments":{
               "count":0,
               "url":"https://ibm-watson-ml-dev.stage1.mybluemix.net/v3/wml_instances/{instance_id}/published_models/{publipublished_model_id}/deployments"
            },
            "input_data_schema":{
               "type":"struct",
               "fields":[
                  {
                     "metadata":{

                     },
                     "type":"string",
                     "name":"GENDER",
                     "nullable":true
                  },
                  {
                     "metadata":{

                     },
                     "type":"integer",
                     "name":"AGE",
                     "nullable":true
                  },
                  {
                     "metadata":{

                     },
                     "type":"string",
                     "name":"MARITAL_STATUS",
                     "nullable":true
                  },
                  {
                     "metadata":{

                     },
                     "type":"string",
                     "name":"PROFESSION",
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


Please note **deployments** `url` that is needed to create online deployment in next step.


## Creating the online deployment

Use the following API call to create an online deployment of your
predictive model.

Request example:

```
curl -X POST --header "Content-Type: application/json" --header "Accept: application/json" --header "Authorization: $token" -d '{"name": "Product Line Prediction", "description": "Product Line Prediction Deployment", "type": "online"}' https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/{published_model_id}/deployments
```
{: codeblock}


Output example:

```
{  
   "metadata":{  
      "guid":"b97072ec-3ef8-4705-a1e7-c264e270e49a",
      "url":"https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/{published_model_id}/deployments/{deployment_id}",
      "created_at":"2017-06-27T13:47:49.534Z",
      "modified_at":"2017-06-27T13:47:58.347Z"
   },
   "entity":{  
      "runtime_environment":"spark-2.0",
      "name":"Product Line Prediction TMNL",
      "instance_href":"/v2/scoring/online/jobs/b97072ec-3ef8-4705-a1e7-c264e270e49a",
      "scoring_url":"https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/{published_model_id}/deployments/{deployment_id}/online",
      "description":"Product Line Prediction Deployment",
      "published_model":{  
         "author":{  
            "name":"IBM",
            "email":""
         },
         "name":"Product Line Prediction",
         "url":"https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/{published_model_id}",
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

## Obtaining deployment details

You can check the status, scoring endpoint address (`scoring_url`),
and parameters related to the deployed model.

Request example:

```
curl -X GET --header "Content-Type: application/json" --header "Accept: application/json" --header "Authorization: $token" https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/{published_model_id}/deployments/{deployment_id}
```
{: codeblock}

Output example:

```
{  
   "metadata":{  
      "guid":"b97072ec-3ef8-4705-a1e7-c264e270e49a",
      "url":"https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/1{published_model_id}/deployments/{deployment_id}",
      "created_at":"2017-06-27T13:47:49.534Z",
      "modified_at":"2017-06-27T13:47:58.347Z"
   },
   "entity":{  
      "runtime_environment":"spark-2.0",
      "name":"Product Line Prediction TMNL",
      "instance_href":"/v2/scoring/online/jobs/b97072ec-3ef8-4705-a1e7-c264e270e49a",
      "scoring_url":"https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/{published_model_id}/deployments/{deployment_id}/online",
      "description":"Product Line Prediction Deployment",
      "published_model":{  
         "author":{  
            "name":"IBM",
            "email":""
         },
         "name":"Product Line Prediction",
         "url":"https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/{published_model_id}",
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

## Making score requests

Since your scoring endpoint has been created (`scoring_url`), you
can now generate predictions by making score requests. In this
scenario, customer records are passed to the predictive model and
sport product prediction is returned.

Sample record header:

```
GENDER,AGE,MARITAL_STATUS,PROFESSION
```
{: codeblock}

Sample record:

```
M,27,Single,Professional
F,56,Unspecified,Hospitality
M,45,Married,Retired
```
{: codeblock}

Request example:

```
curl -X POST --header 'Content-Type: application/json' --header 'Accept: application/json' --header "Authorization: $token" -d '{"fields": ["GENDER","AGE","MARITAL_STATUS","PROFESSION"],"values": [["M",23,"Single","Student"],["M",55,"Single","Executive"]]}' https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/{published_model_id}/deployments/{deployment_id}/online
```
{: codeblock}

Output example:

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
   "values":[
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

We can see, for example, that a 55-year-old executive is
interested in Mountaineering Equipment, while a 23-year-old
student is interested in Personal Accessories.

**Note**: For scikit-learn models only `values` field is required in scoring payload.

Request example:

```
curl -X POST --header 'Content-Type: application/json' --header 'Accept: application/json' --header "Authorization: $token" -d '{"values": [[0.0,1.0],[4.0,15.0]]}' https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/{published_model_id}/deployments/{deployment_id}/online
```
{: codeblock}
