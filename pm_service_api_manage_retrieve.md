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

# Retrieving a list of all currently deployed models


GET http://{PA Bluemix load balancer
URL}/pm/v1/model?accesskey={access_key for this bound
application}

Retrieve a summary of all models that are currently deployed on
this service instance.

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