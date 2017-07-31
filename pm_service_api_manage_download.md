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

# Downloading a copy of a specific deployed model file


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