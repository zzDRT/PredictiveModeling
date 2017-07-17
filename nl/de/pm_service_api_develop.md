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

# Anwendungen zur Nutzung implementierter SPSS-Modelle entwickeln


*  Scoring mit implementiertem Vorhersagemodell durchführen

*  Metadaten für implementiertes Vorhersagemodell abrufen

*  WADL-Zusammenfassung (Web Application Description Language) dieses Service abrufen

SCORING MIT IMPLEMENTIERTEM VORHERSAGEMODELL DURCHFÜHREN

POST http://{PA Bluemix load balancer
URL}/pm/v1/score/{contextId}?accesskey={access_key for this bound
application}

Verwenden Sie diesen API-Aufruf, um die Eingabedaten zu veröffentlichen, die vom implementierten Modell zum Generieren und Zurückgeben der Vorhersageanalyse in den Scoring-Ergebnissen verwendet werden sollen.

Anforderungsbeispiel:

```
    Content-Type: application/json;charset=UTF-8
    Parameters:
        Path parameters:
            contextId: the identifier of the deployed model to use to process this score request
        Query Parameters:
            accesskey: access_key from env.VCAP_SERVICES
        Body: the input data, json string, eg.
            {
                "tablename":"DRUG1n.sav",
                "header":["Age", "Sex", "BP", "Cholesterol", "Na", "K", "Drug"],
                "data":[[43.0, "M", "LOW", "NORMAL", 0.526102, 0.027164, "drugY"]]
            }   
```
{: codeblock}

Beispiel für erfolgreiche Antwort auf vorherige Anforderung: 

```
    Content-Type: application/json;charset=UTF-8
    Status code: 200
    body: the score result, a json array, eg.
        [
            {
                "header":["Age","Sex","BP","Cholesterol","Na""K","Drug","$N-Drug","$NC-Drug"],
                "data":[[23.0,"M","NORMAL","NORMAL",0.78452,0.055959,"drugX","drugX",0.9892564426956728]]
            }
        ]
```
{: codeblock}

Antwort für fehlgeschlagene Scoring-Anforderung:

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

METADATEN FÜR IMPLEMENTIERTES VORHERSAGEMODELL ABRUFEN

GET http://{service
instance}/pm/v1/metadata/{contextId}?accesskey={access_key for
this bound application}&metadatatype=score

Verwenden Sie diesen API-Aufruf zum Abrufen von Metadaten für die Scoring-Verzweigung eines implementierten IBM SPSS Modeler-Datenstroms. Geben Sie bei dieser Methode keinen Anforderungshauptteil an.

Anforderungsbeispiel:

```
    Content-Type: application/json;charset=UTF-8
    Parameters:
        Path parameters:
            contextId: the identifier of the deployed model to use to process this metadata retrieval request
        Query Parameters:
            accesskey: access_key from env.VCAP_SERVICES
```
{: codeblock}

Beispiel für erfolgreiche Antwort auf vorherige Anforderung: 

```
    Content-Type: application/json;charset=UTF-8
    Status code: 200
    body: the metadata (formatted for readability in this document) on the scoring branch eg.
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

Antwort für fehlgeschlagene Scoring-Anforderung:

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

WADL-ZUSAMMENFASSUNG (WEB APPLICATION DESCRIPTION LANGUAGE) DIESES SERVICE ABRUFEN

OPTIONS http://{PA Bluemix load balancer URL}/pm/v1/wadl

Verwenden Sie diesen API-Aufruf zum Abrufen der WADL für diesen Service.

Anforderungsbeispiel:

```
    Content-Type: */*
```
{: codeblock}

Antwort bei erfolgreicher WADL-Anforderung:

```
    Content-Type: application/vnd.sun.wadl+xml
    Status code: 200
    body: the WADL XML ,eg.
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

Antwort bei fehlgeschlagener WADL-Anforderung:

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
