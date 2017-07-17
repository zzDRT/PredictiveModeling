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

# Suppression d'un modèle prédictif déployé


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
