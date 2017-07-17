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

# Téléchargement d'une copie d'un fichier de modèle déployé spécifique


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
