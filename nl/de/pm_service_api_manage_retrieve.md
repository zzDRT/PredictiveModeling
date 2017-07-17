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

# Liste aller momentan implementierten Modelle abrufen


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
