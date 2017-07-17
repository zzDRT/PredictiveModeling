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

# Kopie einer bestimmten implementierten Modelldatei herunterladen


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
