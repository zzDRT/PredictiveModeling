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

# Evaluation par score avec un modèle prédictif déployé


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
