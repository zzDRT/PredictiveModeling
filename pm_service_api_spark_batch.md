---

copyright:
  years: 2016, 2017
lastupdated: "2017-07-24"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Deploying batch models <span class='tag--beta'>Beta</span>

**Note**: This functionality is currently in beta and only available
for use with Spark MLlib. If you're interested in participating, add yourself to the wait list! For more information, see: [https://www.ibm.biz/mlwaitlist](https://www.ibm.biz/mlwaitlist).

**Scenario name**: Customer satisfaction prediction.

**Scenario description**: A telecommunications company wants to know
which customers are at risk of leaving. The presented model
predicts customer churn. A data scientist develops a predictive
model and shares it with you (the developer). Your task is to
deploy the model and generate predictive analytics by making
score requests against the deployed model.

## Using the sample model

1. Go to the Samples tab of the IBM® Watson™ Machine Learning
   Dashboard.

2. In the Sample Models section, find the Customer Satisfaction
   Prediction tile and click the Add model button (+).

Now you'll see the sample Customer Satisfaction Prediction model
in the list of available models on the Models tab.

## Generating the access token

Generate an access token using the user and password available
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
the environment variable access_token:

```
access_token="<token_value>"
```
{: codeblock}

## Working with published models
Use the following API call to get your instance details, such as:
* published models ```url```
* deployments ```url```
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


Having **published_models** ```url``` use the following API call to get model's details:

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
            "guid":"dc46315a-c30e-46a3-8e30-33518e6f7976",
            "url":"https://ibm-watson-ml.stage1.mybluemix.net/v3/wml_instances/7a0f9c88-3cf6-4433-89ee-92a641f26e89/published_models/dc46315a-c30e-46a3-8e30-33518e6f7976",
            "created_at":"2017-03-21T13:49:38.711Z",
            "modified_at":"2017-03-21T13:49:38.802Z"
         },
         "entity":{
            "runtime_environment":"spark-2.0",
            "author":{
               "name":"IBM",
               "email":""
            },
            "name":"Customer Satisfaction Prediction",
            "description":"Predicts Telco customer churn.",
            "label_col":"Churn",
            "training_data_schema":{
               "type":"struct",
               "fields":[
                  {
                     "metadata":{

                     },
                     "type":"string",
                     "name":"customerID",
                     "nullable":true
                  },
                  {
                     "metadata":{

                     },
                     "type":"string",
                     "name":"gender",
                     "nullable":true
                  },
                  {
                     "metadata":{

                     },
                     "type":"integer",
                     "name":"SeniorCitizen",
                     "nullable":true
                  },
                  {
                     "metadata":{

                     },
                     "type":"string",
                     "name":"Partner",
                     "nullable":true
                  },
                  {
                     "metadata":{

                     },
                     "type":"string",
                     "name":"Dependents",
                     "nullable":true
                  },
                  {
                     "metadata":{

                     },
                     "type":"integer",
                     "name":"tenure",
                     "nullable":true
                  },
                  {
                     "metadata":{

                     },
                     "type":"string",
                     "name":"PhoneService",
                     "nullable":true
                  },
                  {
                     "metadata":{

                     },
                     "type":"string",
                     "name":"MultipleLines",
                     "nullable":true
                  },
                  {
                     "metadata":{

                     },
                     "type":"string",
                     "name":"InternetService",
                     "nullable":true
                  },
                  {
                     "metadata":{

                     },
                     "type":"string",
                     "name":"OnlineSecurity",
                     "nullable":true
                  },
                  {
                     "metadata":{

                     },
                     "type":"string",
                     "name":"OnlineBackup",
                     "nullable":true
                  },
                  {
                     "metadata":{

                     },
                     "type":"string",
                     "name":"DeviceProtection",
                     "nullable":true
                  },
                  {
                     "metadata":{

                     },
                     "type":"string",
                     "name":"TechSupport",
                     "nullable":true
                  },
                  {
                     "metadata":{

                     },
                     "type":"string",
                     "name":"StreamingTV",
                     "nullable":true
                  },
                  {
                     "metadata":{

                     },
                     "type":"string",
                     "name":"StreamingMovies",
                     "nullable":true
                  },
                  {
                     "metadata":{

                     },
                     "type":"string",
                     "name":"Contract",
                     "nullable":true
                  },
                  {
                     "metadata":{

                     },
                     "type":"string",
                     "name":"PaperlessBilling",
                     "nullable":true
                  },
                  {
                     "metadata":{

                     },
                     "type":"string",
                     "name":"PaymentMethod",
                     "nullable":true
                  },
                  {
                     "metadata":{

                     },
                     "type":"double",
                     "name":"MonthlyCharges",
                     "nullable":true
                  },
                  {
                     "metadata":{

                     },
                     "type":"string",
                     "name":"TotalCharges",
                     "nullable":true
                  },
                  {
                     "metadata":{

                     },
                     "type":"string",
                     "name":"Churn",
                     "nullable":true
                  }
               ]
            },
            "latest_version":{
               "url":"https://ibm-watson-ml.stage1.mybluemix.net/v2/artifacts/models/dc46315a-c30e-46a3-8e30-33518e6f7976/versions/658b5b8b-6958-471d-a8b1-c6ac079e2522",
               "guid":"658b5b8b-6958-471d-a8b1-c6ac079e2522",
               "created_at":"2017-03-21T13:49:38.802Z"
            },
            "model_type":"sparkml-model-2.0",
            "deployments":{
               "count":0,
               "url":"https://ibm-watson-ml.stage1.mybluemix.net/v3/wml_instances/7a0f9c88-3cf6-4433-89ee-92a641f26e89/published_models/dc46315a-c30e-46a3-8e30-33518e6f7976/deployments"
            },
            "input_data_schema":{
               "type":"struct",
               "fields":[
                  {
                     "metadata":{

                     },
                     "type":"string",
                     "name":"customerID",
                     "nullable":true
                  },
                  {
                     "metadata":{

                     },
                     "type":"string",
                     "name":"gender",
                     "nullable":true
                  },
                  {
                     "metadata":{

                     },
                     "type":"integer",
                     "name":"SeniorCitizen",
                     "nullable":true
                  },
                  {
                     "metadata":{

                     },
                     "type":"string",
                     "name":"Partner",
                     "nullable":true
                  },
                  {
                     "metadata":{

                     },
                     "type":"string",
                     "name":"Dependents",
                     "nullable":true
                  },
                  {
                     "metadata":{

                     },
                     "type":"integer",
                     "name":"tenure",
                     "nullable":true
                  },
                  {
                     "metadata":{

                     },
                     "type":"string",
                     "name":"PhoneService",
                     "nullable":true
                  },
                  {
                     "metadata":{

                     },
                     "type":"string",
                     "name":"MultipleLines",
                     "nullable":true
                  },
                  {
                     "metadata":{

                     },
                     "type":"string",
                     "name":"InternetService",
                     "nullable":true
                  },
                  {
                     "metadata":{

                     },
                     "type":"string",
                     "name":"OnlineSecurity",
                     "nullable":true
                  },
                  {
                     "metadata":{

                     },
                     "type":"string",
                     "name":"OnlineBackup",
                     "nullable":true
                  },
                  {
                     "metadata":{

                     },
                     "type":"string",
                     "name":"DeviceProtection",
                     "nullable":true
                  },
                  {
                     "metadata":{

                     },
                     "type":"string",
                     "name":"TechSupport",
                     "nullable":true
                  },
                  {
                     "metadata":{

                     },
                     "type":"string",
                     "name":"StreamingTV",
                     "nullable":true
                  },
                  {
                     "metadata":{

                     },
                     "type":"string",
                     "name":"StreamingMovies",
                     "nullable":true
                  },
                  {
                     "metadata":{

                     },
                     "type":"string",
                     "name":"Contract",
                     "nullable":true
                  },
                  {
                     "metadata":{

                     },
                     "type":"string",
                     "name":"PaperlessBilling",
                     "nullable":true
                  },
                  {
                     "metadata":{

                     },
                     "type":"string",
                     "name":"PaymentMethod",
                     "nullable":true
                  },
                  {
                     "metadata":{

                     },
                     "type":"double",
                     "name":"MonthlyCharges",
                     "nullable":true
                  },
                  {
                     "metadata":{

                     },
                     "type":"string",
                     "name":"TotalCharges",
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


Please note **deployments** ```url``` that is needed to create batch deployment in next step.


## Creating a batch deployment with Object Storage

To use a REST API call to create a batch deployment of your
predictive model, provide the following details:

*  The access token created in the previous step

*  Spark service credentials, which can be found on the Service
   Credentials tab of the Bluemix Spark service dashboard. Before
   making the deployment request, Spark credentials must be
   decoded as base64 and passed in the header of a curL request
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

*  Object Storage details, which will be used as input (customer
   data to score) for the model and storage for the model output
   (results.csv in this case, which is automatically created).

*  To create a deployment, use the **deployments** ```url``` from previous section.


Request example:

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

Output example:

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

**Note**: You can also use the Dashboard to create a batch
deployment.


## Obtaining deployment details

You can check the status, and parameters related to the deployment model using **metadata** ```url``` (see above output example).

Request example:

```
curl -v -XGET -H "Content-Type:application/json" -H "Authorization:Bearer $access_token" https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/{published_model_id}/deployments/{deployment_id}
```
{: codeblock}

Output example:

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

The prediction result is saved to a .csv file in IBM Object
Storage. Following is a sample row.

Input file preview:

```
customerID,gender,SeniorCitizen,Partner,Dependents,tenure,PhoneService,MultipleLines,InternetService,OnlineSecurity,OnlineBackup,DeviceProtection,TechSupport,StreamingTV,StreamingMovies,Contract,PaperlessBilling,PaymentMethod,MonthlyCharges,TotalCharges,Churn
7590-VHVEG,Female,0,Yes,No,1,No,No phone service,DSL,No,Yes,No,No,No,No,Month-to-month,Yes,Electronic check,29.85,29.85,No
5575-GNVDE,Male,0,No,No,34,Yes,No,DSL,Yes,No,Yes,No,No,No,One year,No,Mailed check,56.95,1889.5,No
3668-QPYBK,Male,0,No,No,2,Yes,No,DSL,Yes,Yes,No,No,No,No,Month-to-month,Yes,Mailed check,53.85,108.15,Yes
```
{: codeblock}

Output file preview:

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

## Deleting a batch deployment

You can delete the deployment if it's no longer needed using a
query such as the following sample.

Request example:

```
curl -v -XDELETE -H "Content-Type:application/json" -H
"Authorization:Bearer $access_token" https://ibm-watson-
ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/{published_model_id}/deployments/{deployment_id}
```
{: codeblock}

Output example:

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
