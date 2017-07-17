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

# Richiama il riepilogo WADL (Web Application Description Language) di questo servizio


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
