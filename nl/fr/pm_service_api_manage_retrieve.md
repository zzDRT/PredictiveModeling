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

# Extraction d'une liste de tous les modèles actuellement déployés


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
