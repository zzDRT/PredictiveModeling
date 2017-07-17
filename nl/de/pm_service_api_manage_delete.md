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

# Implementiertes Vorhersagemodell löschen


DELETE http://{service
instance}/pm/v1/model/{contextId}?accesskey={access_key for this
bound application}

Verwenden Sie diesen API-Aufruf, um das Vorhersagemodell aus der
Machine Learning-Serviceinstanz zu löschen. Nach diesem Aufruf steht das Vorhersagemodell nicht mehr zum Download oder zum Scoring von Daten in Ihren Anwendungen zur Verfügung.

Anforderungsbeispiel:

```
    Content-Type: */*
    Parameters:
        Path parameters:
            contextId: the identifier of deployed model you want to un-deploy and delete
        Query Parameters:
            accesskey: access_key from env.VCAP_SERVICES
```
{: codeblock}

Antwort bei erfolgreicher Deimplementierung:

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

Antwort bei fehlgeschlagener Deimplementierung:

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
