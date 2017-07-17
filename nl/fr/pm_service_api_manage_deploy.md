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

# Déploiement ou actualisation d'un modèle prédictif


PUT http://{PA Bluemix load balancer
URL}/pm/v1/model/{contextId}?accesskey={access_key pour cette application liée}

Cet appel API permet de télécharger un fichier contenant la branche d'évaluation développée par IBM SPSS Modeler que vous souhaitez déployer.
Cette API est mise à votre disposition afin que vous puissiez évaluer par score les données de vos applications. Chaque fichier de modèle dispose d'un ID de contexte, lequel constitue un alias pratique pour référencer le modèle déployé dans les appels de service ultérieurs. Si un modèle est associé à un ID de contexte, il est remplacé par cet appel PUT en actualisant l'analyse prédictive utilisée par vos
applications.

Exemple de requête :

```
    Content-Type: multipart/form-data
    Parameters:
        Form parameters:
            model_file : fichier modèle à télécharger
        Path parameters:
            contextId : identificateur unique à affecter à votre modèle ou référence au modèle déployé à actualiser
        Query Parameters:
            accesskey: access_key de env.VCAP_SERVICES
```
{: codeblock}

Réponse si le déploiement aboutit :

```
    Content-Type: application/json
    Status code: 200
    body:
        {
           "flag":true,
           "message":"informations détaillées"
         }
```
{: codeblock}

Réponse en cas d'échec du déploiement :

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
