---

copyright:
  years: 2016, 2017
lastupdated: "2017-08-01"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Deleting a deployed predictive model


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