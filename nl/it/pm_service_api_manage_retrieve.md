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

# Richiamo di un elenco di tutti i modelli attualmente distribuiti


GET http://{PA Bluemix load balancer
URL}/pm/v1/model?accesskey={access_key for this bound
application}

Richiama un riepilogo di tutti i modelli attualmente distribuiti su questa istanza del servizio.

Esempio di
richiesta:

```
    Content-Type: */*
    Parametri:
        Parametri query:
            accesskey: access_key da env.VCAP_SERVICES
```
{: codeblock}

Risposta quando la richiesta per un riepilogo di modello distribuito riesce:

```
    Content-Type: application/json
    Codice di stato: 200
    corpo: un array vuoto se non è stato distribuito alcun modello o un riepilogo dei modelli distribuiti...
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

Risposta quando la richiesta per un riepilogo di modello distribuito non riesce:

```
    Content-Type: application/json
    Codice di stato: 200
    corpo:
        {
           "flag":false,
           "message":"motivo"
         }
```
{: codeblock}
