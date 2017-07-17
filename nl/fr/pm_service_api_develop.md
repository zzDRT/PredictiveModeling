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

# Développement d'applications d'optimisation des modèles SPSS déployés


*  Evaluation par score avec un modèle prédictif déployé

*  Extraction des métadonnées d'un modèle prédictif déployé

*  Extraction du récapitulatif WADL (Web Application Description Language) de ce service

SCORING A L'AIDE UN MODÈLE PRÉDICTIF DÉPLOYÉ

POST http://{PA Bluemix load balancer
URL}/pm/v1/score/{contextId}?accesskey={access_key pour cette application liée}

Cet appel API permet de poster des données d'entrée devant être utilisées par le modèle déployé pour générer et renvoyer les analyses prédictives dans les résultats du score.

Exemple de requête :

```
    Content-Type: application/json;charset=UTF-8
    Parameters:
        Path parameters:
            contextId: identificateur du modèle déployé à utiliser pour traiter cette requête de score
        Query Parameters:
            accesskey: access_key de env.VCAP_SERVICES
        Body: the input data, json string, eg.
            {
                "tablename":"DRUG1n.sav",
                "header":["Age", "Sex", "BP", "Cholesterol", "Na", "K", "Drug"],
                "data":[[43.0, "M", "LOW", "NORMAL", 0.526102, 0.027164, "drugY"]]
            }   
```
{: codeblock}

Exemple de réponse réussie à la requête précédente :

```
    Content-Type: application/json;charset=UTF-8
    Status code: 200
    body: résultat du score, tableau json, par exemple.
        [
            {
                "header":["Age","Sex","BP","Cholesterol","Na""K","Drug","$N-Drug","$NC-Drug"],
                "data":[[23.0,"M","NORMAL","NORMAL",0.78452,0.055959,"drugX","drugX",0.9892564426956728]]
            }
        ]
```
{: codeblock}

Réponse en cas d'échec de la requête de score :

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

EXTRACTION DES MÉTADONNÉES D'UN MODÈLE PRÉDICTIF DÉPLOYÉ

GET http://{service
instance}/pm/v1/metadata/{contextId}?accesskey={access_key pour
cette application liée} &metadatatype=score

Cet appel API permet d'extraire les métadonnées de la branche d'évaluation d'un flux IBM SPSS
        Modeler déployé. Vous ne devez pas indiquer de corps de demande avec cette méthode.

Exemple de requête :

```
    Content-Type: application/json;charset=UTF-8
    Parameters:
        Path parameters:
            contextId: identificateur du modèle déployé à utiliser pour traiter cette requête d'extraction des métadonnées
        Query Parameters:
            accesskey: access_key de env.VCAP_SERVICES
```
{: codeblock}

Exemple de réponse réussie à la requête précédente :

```
    Content-Type: application/json;charset=UTF-8
    Status code: 200
    body: métadonnées (formatées pour lisibilité dans ce document) sur la branche d'évaluation, par exemple.
    [
      {
        flag: true
        message:
          "Métadonnées d'entrée de scoring du modèle :
          <?xml version="1.0" encoding="UTF-8" standalone="yes"?>
          <metadata xmlns="http://spss.ibm.com/meta/internal">
          <table xsi:type="nodeImpl" tag="id574QKQ8NL6E" name="baskrule_input.csv" 
              xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
            <field measurementLevel="CATEGORICAL" storageType="STRING" name="gender"/>
            <field measurementLevel="CONTINUOUS" storageType="LONG" name="income"/>
          </table>
          </metadata>
          Métadonnées de sortie de scoring du modèle :
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

Réponse en cas d'échec de la requête de scoring :

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

EXTRACTION DU RÉCAPITULATIF WADL (WEB APPLICATION DESCRIPTION LANGUAGE) DE CE SERVICE

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
