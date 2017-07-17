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

# Eliminazione di un modello predittivo distribuito


DELETE http://{service
instance}/pm/v1/model/{contextId}?accesskey={access_key for this
bound application}

Utilizza questa chiamata API per eliminare il modello predittivo dall'istanza del servizio Machine
Learning. Dopo questa chiamata, il modello predittivo non sarà più disponibile per il download o il calcolo del punteggio dei dati nelle tue applicazioni.

Esempio di
richiesta:

```
    Content-Type: */*
    Parametri:
        Parametri percorso:
            contextId: l'identificativo del modello distribuito di cui si desidera annullare la distribuzione e che si desidera eliminare
        Parametri query:
            accesskey: access_key da env.VCAP_SERVICES
```
{: codeblock}

Risposta quando l'annullamento della distribuzione riesce:

```
    Content-Type: application/json
    Codice di stato: 200
    corpo:
        {
           "flag":true,
           "message":"informazioni dettagliate"
         }
```
{: codeblock}

Risposta quando l'annullamento della distribuzione non riesce:

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
