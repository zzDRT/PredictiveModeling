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

# Gestion des modèles SPSS déployés


*  [Déploiement ou actualisation d'un modèle prédictif](#deploying-or-refreshing-a-predictive-model)

*  [Extraction d'une liste de tous les modèles actuellement déployés](#retrieving-a-list-of-all-currently-deployed-models)

*  [Téléchargement d'une copie d'un fichier de modèle déployé spécifique](#downloading-a-copy-of-a-specific-deployed-model-file)

*  [Suppression d'un modèle prédictif déployé](#deleting-a-deployed-predictive-model)

## Déploiement ou actualisation d'un modèle prédictif

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

## Extraction d'une liste de tous les modèles actuellement déployés

GET http://{PA Bluemix load balancer
URL}/pm/v1/model?accesskey={access_key pour cette application liée}

Cet appel d'API permet d'extraire un récapitulatif de tous les modèles actuellement déployés sur cette instance de service.

Exemple de requête :

```
    Content-Type: */*
    Parameters:
        Query Parameters:
            accesskey: access_key de env.VCAP_SERVICES
```
{: codeblock}

Réponse en cas de succès de la demande de récapitulatif du modèle déployé :

```
    Content-Type: application/json
    Status code: 200
    body : tableau vide si aucun modèle n'a été déployé ou récapitulatif des modèles déployés...
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

Réponse en cas d'échec de la demande de récapitulatif du modèle déployé :

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

## Téléchargement d'une copie d'un fichier de modèle déployé spécifique

GET http://{PA Bluemix load balancer
URL}/pm/v1/model/{contextId}?accesskey={access_key pour cette application
liée}

Cet appel API permet de télécharger une copie d'un fichier de modèle déployé spécifique.

Exemple de requête :

```
    Content-Type: */*
    Parameters:
        Path parameters:
            contextId : identificateur du modèle prédictif déployé que vous désirez télécharger
        Query Parameters:
            accesskey: access_key de env.VCAP_SERVICES
```
{: codeblock}

Réponse en cas de succès de la requête de téléchargement :

```
    Content-Type: application/octet-stream
    Status code: 200
    Header:
        "Content-Disposition":"attachment; filename=nom_fichier_modèle.str"
```
{: codeblock}

Réponse en cas d'échec de la requête de téléchargement :

```
    Content-Type: application/json
    Status code: 200
    body:
        {
           "flag":false,
           "message":String // motif de l'échec
        }
```
{: codeblock}

## Suppression d'un modèle prédictif déployé

DELETE http://{service
instance}/pm/v1/model/{contextId}?accesskey={access_key pour cette application
liée}

Cet appel d'API permet de supprimer de l'instance de service Machine
Learning le modèle prédictif. Après cet appel, le modèle prédictif n'est plus disponible en téléchargement ou pour l'évaluation par score des données de vos applications.

Exemple de requête :

```
    Content-Type: */*
    Parameters:
        Path parameters:
            contextId : identificateur du modèle déployé dont vous désirez annuler le déploiement et supprimer
        Query Parameters:
            accesskey: access_key de env.VCAP_SERVICES
```
{: codeblock}

Réponse en cas de succès de l'annulation du déploiement :

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

Réponse en cas d'échec de l'annulation du déploiement :

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
