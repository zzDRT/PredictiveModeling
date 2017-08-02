---

copyright:
  years: 2016, 2017
lastupdated: "2017-08-02"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Deploying or refreshing a predictive model


PUT http://{PA Bluemix load balancer
URL}/pm/v1/model/{contextId}?accesskey={access_key for this bound
application}

Use this API call to upload a file that contains the IBM SPSS
Modeler developed scoring branch that you would like to deploy.
It is made available for scoring data in your applications. Each
model file is given a context ID as a convenient alias to use for
referencing the deployed model in subsequent service calls. If a
model exists for a context ID, it is replaced by this PUT call as
a means of refreshing the predictive analytics in use by your
applications.

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