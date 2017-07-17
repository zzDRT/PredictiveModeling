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

# Calcolo del punteggio con un modello predittivo distribuito


POST http://{PA Bluemix load balancer
URL}/pm/v1/score/{contextId}?accesskey={access_key for this bound
application}

Utilizza questa chiamata API per inserire i dati di input che devono essere utilizzati dal modello distribuito per generare e
                restituire l'analisi predittiva nei risultati di punteggio.

Esempio di
richiesta:

```
    Content-Type: application/json;charset=UTF-8
    Parametri:
        Parametri percorso:
            contextId: l'identificativo del modello distribuito da utilizzare per elaborare questa richiesta di punteggio
        Parametri query:
            accesskey: access_key da env.VCAP_SERVICES
        Corpo: i dati di input, stringa json, ad es.
            {
                "tablename":"DRUG1n.sav", 
                "header":["Age", "Sex", "BP", "Cholesterol", "Na", "K", "Drug"], 
                "data":[[43.0, "M", "LOW", "NORMAL", 0.526102, 0.027164, "drugY"]]
            }   
```
{: codeblock}

Esempio di una risposta positiva alla richiesta precedente:

```
    Content-Type: application/json;charset=UTF-8
    Codice di stato: 200
    corpo: il risultato di punteggio, un array json, ad es.
        [
            {
                "header":["Age","Sex","BP","Cholesterol","Na""K","Drug","$N-Drug","$NC-Drug"], 
                "data":[[23.0,"M","NORMAL","NORMAL",0.78452,0.055959,"drugX","drugX",0.9892564426956728]]
            }
        ]
```
{: codeblock}

Risposta quando la richiesta di calcolo di punteggio non riesce:

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
