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

# Gestione dei modelli SPSS distribuiti


*  [Distribuzione o aggiornamento di un modello predittivo](#deploying-or-refreshing-a-predictive-model)

*  [Richiamo di un elenco di tutti i modelli attualmente distribuiti](#retrieving-a-list-of-all-currently-deployed-models)

*  [Download di una copia di uno specifico file modello distribuito](#downloading-a-copy-of-a-specific-deployed-model-file)

*  [Eliminazione di un modello predittivo distribuito](#deleting-a-deployed-predictive-model)

## Distribuzione o aggiornamento di un modello predittivo

PUT http://{PA Bluemix load balancer
URL}/pm/v1/model/{contextId}?accesskey={access_key for this bound
application}

Utilizza questa API per caricare un file che contiene il ramo di calcolo del punteggio sviluppato
        da IBM SPSS Modeler che desideri distribuire.
Viene reso disponibile per il calcolo del punteggio dei
        dati nelle tue applicazioni. A ogni
file di modello viene dato un ID contesto come un pratico alias da utilizzare per
fare riferimento al modello distribuito nelle successive chiamate di servizio. Se esiste
un modello per un ID contesto, esso viene sostituito da questa chiamata PUT come
un modo per aggiornare l'analisi predittiva utilizzata dalle tue
applicazioni.

Esempio di
richiesta:

```
    Content-Type: multipart/form-data
    Parametri:
        Parametri modulo:
            model_file: il file del modello da caricare
        Parametri percorso:
            contextId: l'identificativo univoco da assegnare al tuo modello o un riferimento al modello distribuito da aggiornare
        Parametri query:
            accesskey: access_key da env.VCAP_SERVICES
```
{: codeblock}

Risposta quando la distribuzione riesce:

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

Risposta quando la distribuzione non riesce:

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

## Richiamo di un elenco di tutti i modelli attualmente distribuiti

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

## Download di una copia di uno specifico file modello distribuito

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

## Eliminazione di un modello predittivo distribuito

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
