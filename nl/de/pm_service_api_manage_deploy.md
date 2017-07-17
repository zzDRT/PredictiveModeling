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

# Vorhersagemodell implementieren oder aktualisieren


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
