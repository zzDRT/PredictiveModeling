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

# Distribuzione o aggiornamento di un modello predittivo


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
