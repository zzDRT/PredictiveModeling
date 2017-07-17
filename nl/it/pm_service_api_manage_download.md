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

# Download di una copia di uno specifico file modello distribuito


GET http://{PA Bluemix load balancer
URL}/pm/v1/model/{contextId}?accesskey={access_key for this bound
application}

Utilizza questa chiamata API per eseguire il download di uno specifico file modello distribuito.

Esempio di
richiesta:

```
    Content-Type: */*
    Parametri:
        Parametri percorso:
            contextId: l'identificativo del modello predittivo distribuito di cui desideri eseguire il download
        Parametri query:
            accesskey: access_key da env.VCAP_SERVICES
```
{: codeblock}

Risposta quando la richiesta di download riesce:

```
    Content-Type: application/octet-stream
    Codice di stato: 200
    Intestazione:
        "Content-Disposition":"attachment; filename=model_file_name.str"
```
{: codeblock}

Risposta quando la richiesta di download non riesce:

```
    Content-Type: application/json
    Codice di stato: 200
    corpo:
        {
           "flag":false,
           "message":String // motivo dell'errore
        }
```
{: codeblock}
