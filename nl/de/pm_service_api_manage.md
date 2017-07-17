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

# Implementierte SPSS-Modelle verwalten


*  [Vorhersagemodell implementieren oder aktualisieren](#deploying-or-refreshing-a-predictive-model)

*  [Liste aller momentan implementierten Modelle abrufen](#retrieving-a-list-of-all-currently-deployed-models)

*  [Kopie einer bestimmten implementierten Modelldatei herunterladen](#downloading-a-copy-of-a-specific-deployed-model-file)

*  [Implementiertes Vorhersagemodell löschen](#deleting-a-deployed-predictive-model)

## Vorhersagemodell implementieren oder aktualisieren

PUT http://{PA Bluemix load balancer
URL}/pm/v1/model/{contextId}?accesskey={access_key for this bound
application}

Verwenden Sie diesen API-Aufruf, um eine Datei hochzuladen, die die von IBM SPSS Modeler entwickelte Scoring-Verzweigung enthält, die implementiert werden soll.
Sie wird für das Scoring von Daten in Ihren Anwendungen zur Verfügung gestellt. Jeder Modelldatei
wird eine Kontext-ID als Alias zugeordnet, der zum Referenzieren des implementierten Modells
in nachfolgenden Serviceaufrufen verwendet wird. Wenn für eine Kontext-ID ein Modell vorhanden ist,
wird sie durch diesen PUT-Aufruf ersetzt, um die momentan von Ihren Anwendungen benutzte Vorhersageanalyse
zu aktualisieren. 

Anforderungsbeispiel:

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

Antwort bei erfolgreicher Implementierung:

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

Antwort bei fehlgeschlagener Implementierung:

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

## Liste aller momentan implementierten Modelle abrufen

GET http://{PA Bluemix load balancer
URL}/pm/v1/model?accesskey={access_key for this bound
application}

Rufen Sie eine Zusammenfassung aller Modelle ab, die momentan in dieser Serviceinstanz implementiert sind.

Anforderungsbeispiel:

```
    Content-Type: */*
    Parameters:
        Query Parameters:
            accesskey: access_key from env.VCAP_SERVICES
```
{: codeblock}

Antwort bei erfolgreicher Anforderung einer Zusammenfassung für ein implementiertes Modell:

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

Antwort bei fehlgeschlagener Anforderung einer Zusammenfassung für ein implementiertes Modell:

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

## Kopie einer bestimmten implementierten Modelldatei herunterladen

GET http://{PA Bluemix load balancer
URL}/pm/v1/model/{contextId}?accesskey={access_key for this bound
application}

Verwenden Sie diesen API-Aufruf, um eine Kopie einer bestimmten implementierten Modelldatei herunterzuladen.

Anforderungsbeispiel:

```
    Content-Type: */*
    Parameters:
        Path parameters:
            contextId: the identifier of deployed predictive model you want to download
        Query Parameters:
            accesskey: access_key from env.VCAP_SERVICES
```
{: codeblock}

Antwort bei erfolgreicher Downloadanforderung:

```
    Content-Type: application/octet-stream
    Status code: 200
    Header:
        "Content-Disposition":"attachment; filename=model_file_name.str"
```
{: codeblock}

Antwort bei fehlgeschlagener Downloadanforderung:

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

## Implementiertes Vorhersagemodell löschen

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
