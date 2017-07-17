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

# Sviluppo di applicazioni che si avvalgono di modelli SPSS distribuiti


*  Calcolo del punteggio con un modello predittivo distribuito

*  Richiamo dei metadati per un modello predittivo distribuito

*  Richiamo del riepilogo WADL (Web Application Description Language) di questo servizio

CALCOLO DEL PUNTEGGIO CON UN MODELLO PREDITTIVO DISTRIBUITO

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

RICHIAMO DEI METADATI PER UN MODELLO PREDITTIVO DISTRIBUITO

GET http://{service
instance}/pm/v1/metadata/{contextId}?accesskey={access_key for
this bound application}&metadatatype=score

Utilizza questa chiamata API per richiamare i metadati per il ramo di calcolo del punteggio
        di un flusso IBM SPSS Modeler distribuito. Non fornire un corpo della richiesta con questo metodo.

Esempio di
richiesta:

```
    Content-Type: application/json;charset=UTF-8
    Parametri:
        Parametri percorso:
            contextId:  l'identificativo del modello distribuito da utilizzare per elaborare questa richiesta di richiamo dei metadati
        Parametri query:
            accesskey: access_key da env.VCAP_SERVICES
```
{: codeblock}

Esempio di una risposta positiva alla richiesta precedente:

```
    Content-Type: application/json;charset=UTF-8
    Codice di stato: 200
    corpo: i metadati (formattati per la leggibilità in questo documento) sul ramo di calcolo del punteggio (ad es.
    [
      {
        flag: true
        message: 
          "Model Score Input Metadata: 
          <?xml version="1.0" encoding="UTF-8" standalone="yes"?>
          <metadata xmlns="http://spss.ibm.com/meta/internal">
          <table xsi:type="nodeImpl" tag="id574QKQ8NL6E" name="baskrule_input.csv" 
              xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
            <field measurementLevel="CATEGORICAL" storageType="STRING" name="gender"/>
            <field measurementLevel="CONTINUOUS" storageType="LONG" name="income"/>
          </table>
          </metadata> 
          Model Score Output Metadata:
          <?xml version="1.0" encoding="UTF-8" standalone="yes"?>
          <metadata xmlns="http://spss.ibm.com/meta/internal">
          <table xsi:type="nodeImpl" tag="id32CPAJBGJFG" name="Flat File" 
              xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
            <field measurementLevel="CATEGORICAL" storageType="STRING" name="gender"/>
            <field measurementLevel="CONTINUOUS" storageType="LONG" name="income"/>
            <field measurementLevel="FLAG" storageType="STRING" name="$C-beer_beans_pizza"/>
            <field measurementLevel="CONTINUOUS" storageType="DOUBLE" name="$CC-beer_beans_pizza"/>
          </table>
          </metadata>"
      }
    ]
```
{: codeblock}

Risposta quando una richiesta di calcolo del punteggio non riesce:

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

RICHIAMO DEL RIEPILOGO WADL (WEB APPLICATION DESCRIPTION LANGUAGE) DI QUESTO
SERVIZIO

OPTIONS http://{PA Bluemix load balancer URL}/pm/v1/wadl

Utilizza questa API per richiamare il WADL per questo servizio.

Esempio di
richiesta:

```
    Content-Type: */*
```
{: codeblock}

Risposta quando la richiesta WADL riesce:

```
    Content-Type: application/vnd.sun.wadl+xml
    Codice di stato: 200
    corpo: l'XML WADL, ad es.
        <?xml version="1.0" encoding="UTF-8" standalone="yes" ?>
            <application>
                <resources base="http://{PA Bluemix load balancer URL}/pm/v1/">
                    <resource path="score">
                        <doc title="Score using the model with given context ID" />
                        <resource path="{id}">
                            <param style="template" name="id" />
                            <method name="POST">
                                <request>
                                    <param style="query" name="accesskey" />
                                    <representation mediaType="application/json;charset=UTF-8" />
                                </request>
                                <response>
                                    <representation mediaType="application/json;charset=UTF-8" />
                                </response>
                            </method>
                        </resource>
                     </resource>
                     ...

             </resources>
        </application>
```
{: codeblock}

Risposta quando la richiesta WADL non riesce:

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
