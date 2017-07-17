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

# Extraction du récapitulatif WADL (Web Application Description Language) de ce service


OPTIONS http://{PA Bluemix load balancer URL}/pm/v1/wadl

Cet appel API permet d'extraire le récapitulatif WADL de ce service.

Exemple de requête :

```
    Content-Type: */*
```
{: codeblock}

Réponse en cas de succès de la requête WADL :

```
    Content-Type: application/vnd.sun.wadl+xml
    Status code: 200
    body: the WADL XML ,eg.
        <?xml version="1.0" encoding="UTF-8" standalone="yes" ?>
            <application>
                <resources base="http://{PA Bluemix load balancer URL}/pm/v1/">
                    <resource path="score">
                        <doc title="Score utilisant le modèle avec l'ID de contexte indiqué" />
                        <resource path="{id}">
                            <param style="template" name="id" />
                            <method name="POST">
                                <requête>
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

Réponse en cas d'échec de la requête WADL :

```
    Content-Type: application/json
    Status code: 200
    body:
        {
           "flag":false,
           "message":"motif"
        } 
        ```
{: codeblock}
