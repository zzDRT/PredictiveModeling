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

# Managing the deployed SPSS models


*  [Deploying or refreshing a predictive model](#deploying-or-refreshing-a-predictive-model)

*  [Retrieving a list of all currently deployed models](#retrieving-a-list-of-all-currently-deployed-models)

*  [Downloading a copy of a specific deployed model file](#downloading-a-copy-of-a-specific-deployed-model-file)

*  [Deleting a deployed predictive model](#deleting-a-deployed-predictive-model)

## Deploying or refreshing a predictive model

Use this API call to upload a file that contains the IBM SPSS
Modeler developed scoring branch that you would like to deploy.
It is made available for scoring data in your applications. Each
model file is given a context ID as a convenient alias to use for
referencing the deployed model in subsequent service calls. If a
model exists for a context ID, it is replaced by this PUT call as
a means of refreshing the predictive analytics in use by your
applications.

```
PUT http://{PA Bluemix load balancer URL}/pm/v1/model/{contextId}?accesskey={access_key for this bound application}
```
{: codeblock}

Request example:

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

Response when the deployment succeeds:

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

Response when the deployment fails:

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

## Retrieving a list of all currently deployed models

Retrieve a summary of all models that are currently deployed on
this service instance.

```
GET http://{PA Bluemix load balancer URL}/pm/v1/model?accesskey={access_key for this bound application}
```
{: codeblock}

Request example:

```
    Content-Type: */*
    Parameters:
        Query Parameters:
            accesskey: access_key from env.VCAP_SERVICES
```
{: codeblock}

Response when the request for a deployed model summary succeeds:

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

Response when the request for a deployed model summary fails:

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

## Downloading a copy of a specific deployed model file

GET http://{PA Bluemix load balancer
URL}/pm/v1/model/{contextId}?accesskey={access_key for this bound
application}

Use this API call to download a copy of a specific deployed model
file.

Request example:

```
    Content-Type: */*
    Parameters:
        Path parameters:
            contextId: the identifier of deployed predictive model you want to download
        Query Parameters:
            accesskey: access_key from env.VCAP_SERVICES
```
{: codeblock}

Response when the download request succeeds:

```
    Content-Type: application/octet-stream
    Status code: 200
    Header:
        "Content-Disposition":"attachment; filename=model_file_name.str"
```
{: codeblock}

Response when the downlaod request fails:

```
    Content-Type: application/json
    Status code: 200
    body:
        {
           "flag":false, 
           "message":String // failure reason 
        }
```
{: codeblock}

## Deleting a deployed predictive model

DELETE http://{service
instance}/pm/v1/model/{contextId}?accesskey={access_key for this
bound application}

Use this API call to delete the predictive model from the Machine
Learning service instance. After this call, the predictive model
will no longer be available for download or scoring data in your
applications.

Request example:

```
    Content-Type: */*
    Parameters:
        Path parameters:
            contextId: the identifier of deployed model you want to un-deploy and delete
        Query Parameters:
            accesskey: access_key from env.VCAP_SERVICES
```
{: codeblock}

Response when the undeploy succeeds:

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

Response when the undeploy fails:

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