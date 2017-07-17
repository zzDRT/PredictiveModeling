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

# Déploiement de modèles en ligne


**Nom du scénario **:  Pronostic de ligne de produits.

**Description du scénario **: une entreprise qui commercialise des équipements de plein air désire construire un modèle pronostiquant l'intérêt des clients pour leur ligne de produits. Un spécialiste des données a développé un modèle prédictif et le partage avec vous (le développeur). Votre tâche consiste à déployer le modèle et à générer des pronostics de l'intérêt des clients en lançant des requêtes de score à partir du modèle déployé.

## Utilisation de l'exemple de modèle

1. Accédez à l'onglet Exemples du tableau de bord IBM® Watson™ Machine Learning.

2. Dans la section Exemples de modèles, recherchez la vignette Pronostics de ligne de produits et cliquez sur le bouton Ajouter le modèle (+).

Vous pouvez observer à présent le modèle Pronostics de ligne de produits dans la liste des modèles disponibles sous l'onglet Modèles.

## Génération du jeton d'accès

Générez un jeton d'accès à partir de l'ID et du mot de passe utilisateur affichés dans l'onglet Données d'identification pour le service de l'instance de service IBM Watson Machine Learning. 

Exemple de requête :

```
curl --basic --user nom_utilisateur:mot_de_passe https://ibm-watson-ml.mybluemix.net/v3/identity/token
```
{: codeblock}

Exemple de sortie :

```
{"token":"**********"}
```
{: codeblock}

## Création du déploiement en ligne

Utilisez l'appel d'API suivant pour générer un déploiement en ligne de votre modèle prédictif. Cette API est mise à votre disposition afin que vous puissiez évaluer par score les données de vos applications. Le noeud final requis pour créer un déploiement en ligne est disponible sur votre tableau de bord WML -> Détails du modèle > URL. Notez également que vous pouvez identifier la valeur de "ID_modèle_publié" dans la zone ID sur la page Détails du modèle dans le tableau de bord IBM Watson Machine Learning et la valeur de
"ID_instance" dans les données d'identification VCAP de l'instance Watson Machine Learning. 

Exemple de requête :

```
curl -X POST --header "Content-Type: application/json" --header "Accept: application/json" --header "Authorization: token" -d '{"name": "Pronostic de ligne de produits", "description": "Déploiement du modèle Pronostic de ligne de produits", "type": "online"}' 'https://ibm-watson-ml.mybluemix.net/v3/instances_wml/{instance_id}/modèles_publiés/ID_modèle_publié}/deployments'
```
{: codeblock}

Exemple de sortie :

```
{  
   "metadata":{
      "guid":"b97072ec-3ef8-4705-a1e7-c264e270e49a",
      "url":"https://ibm-watson-ml.mybluemix.net/v3/wml_instances/21208fa4-f5b8-4fb7-b162-878e455e4be2/published_models/1ab15cdb-4f9e-4d35-8077-c0f6fff10196/deployments/b97072ec-3ef8-4705-a1e7-c264e270e49a",
      "created_at":"2017-06-27T13:47:49.534Z",
      "modified_at":"2017-06-27T13:47:58.347Z"
   },
   "entity":{
      "runtime_environment":"spark-2.0",
      "name":"TMNL Pronostic de ligne de produits ",
      "instance_href":"/v2/scoring/online/jobs/b97072ec-3ef8-4705-a1e7-c264e270e49a",
      "scoring_url":"https://ibm-watson-ml.mybluemix.net/v3/wml_instances/21208fa4-f5b8-4fb7-b162-878e455e4be2/published_models/1ab15cdb-4f9e-4d35-8077-c0f6fff10196/deployments/b97072ec-3ef8-4705-a1e7-c264e270e49a/online",
      "description":"Déploiement du modèle Pronostic de ligne de produits",
      "published_model":{
         "author":{
            "name":"IBM",
            "email":""
         },
         "name":"Pronostic de ligne de produits",
         "url":"https://ibm-watson-ml.mybluemix.net/v3/wml_instances/21208fa4-f5b8-4fb7-b162-878e455e4be2/published_models/1ab15cdb-4f9e-4d35-8077-c0f6fff10196",
         "guid":"1ab15cdb-4f9e-4d35-8077-c0f6fff10196",
         "description":"Pronostic d'intérêt des clients pour les articles de sport dans les chaînes de magasins en Europe.",
         "created_at":"2017-06-27T11:54:24.170Z"
      },
      "model_type":"sparkml-model-2.0",
      "status":"en cours d'initialisation",
      "type":"online",
      "deployed_version":{
         "url":"https://ibm-watson-ml.mybluemix.net/v2/artifacts/models/1ab15cdb-4f9e-4d35-8077-c0f6fff10196/versions/3ab34346-f195-4ee4-b378-1204f674a725",
         "guid":"3ab34346-f195-4ee4-b378-1204f674a725",
         "created_at":"2017-06-27T11:54:07.507Z"
      }
   }
}
```
{: codeblock}

## Obtention des détails du déploiement

Vous pouvez examiner le statut, l'adresse du noeud final d'évaluation (scoringHref) et les paramètres associés au modèle déployé.

Exemple de requête :

```
curl -X GET --header "Content-Type: application/json" --header "Accept: application/json" --header "Authorization: $token" 'https://ibm-watson-ml.mybluemix.net/v3/instances_wml/{ID_instance}/modèles_publiés/{ID_modèle_publié}/deployments/{ID_déploiement}'
```
{: codeblock}

Exemple de sortie :

```
{  
   "metadata":{
      "guid":"b97072ec-3ef8-4705-a1e7-c264e270e49a",
      "url":"https://ibm-watson-ml.mybluemix.net/v3/wml_instances/21208fa4-f5b8-4fb7-b162-878e455e4be2/published_models/1ab15cdb-4f9e-4d35-8077-c0f6fff10196/deployments/b97072ec-3ef8-4705-a1e7-c264e270e49a",
      "created_at":"2017-06-27T13:47:49.534Z",
      "modified_at":"2017-06-27T13:47:58.347Z"
   },
   "entity":{
      "runtime_environment":"spark-2.0",
      "name":"TMNL Pronostic de ligne de produits ",
      "instance_href":"/v2/scoring/online/jobs/b97072ec-3ef8-4705-a1e7-c264e270e49a",
      "scoring_url":"https://ibm-watson-ml.mybluemix.net/v3/wml_instances/21208fa4-f5b8-4fb7-b162-878e455e4be2/published_models/1ab15cdb-4f9e-4d35-8077-c0f6fff10196/deployments/b97072ec-3ef8-4705-a1e7-c264e270e49a/online",
      "description":"Déploiement du modèle Pronostic de ligne de produits",
      "published_model":{
         "author":{
            "name":"IBM",
            "email":""
         },
         "name":"Pronostic de ligne de produits",
         "url":"https://ibm-watson-ml.mybluemix.net/v3/wml_instances/21208fa4-f5b8-4fb7-b162-878e455e4be2/published_models/1ab15cdb-4f9e-4d35-8077-c0f6fff10196",
         "guid":"1ab15cdb-4f9e-4d35-8077-c0f6fff10196",
         "description":"Pronostic d'intérêt des clients pour les articles de sport dans les chaînes de magasins en Europe.",
         "created_at":"2017-06-27T11:54:24.170Z"
      },
      "model_type":"sparkml-model-2.0",
      "status":"Actif",
      "type":"online",
      "deployed_version":{
         "url":"https://ibm-watson-ml.mybluemix.net/v2/artifacts/models/1ab15cdb-4f9e-4d35-8077-c0f6fff10196/versions/3ab34346-f195-4ee4-b378-1204f674a725",
         "guid":"3ab34346-f195-4ee4-b378-1204f674a725",
         "created_at":"2017-06-27T11:54:07.507Z"
      }
   }
}
```
{: codeblock}

## Soumission de requêtes de score

Comme votre noeud final d'évaluation a été créé (scoringHref), vous pouvez maintenant générer des prévisions en créant des requêtes de score. Dans ce scénario, des enregistrements client sont transmis au modèle prédictif et un pronostic sur les articles de sport est renvoyé.

Exemple d'en-tête d'enregistrement :

```
GENDER,AGE,MARITAL_STATUS,PROFESSION
```
{: codeblock}

Exemple d'enregistrement :

```
M,27,Single,Professional
F,56,Unspecified,Hospitality
M,45,Married,Retired
```
{: codeblock}

Exemple de requête :

```
curl -X POST --header 'Content-Type: application/json' --header 'Accept: application/json' --header 'Authorization: token' -d '{"fields": ["GENDER","AGE","MARITAL_STATUS","PROFESSION"],"values": [["M",23,"Single","Student"],["M",55,"Single","Executive"]]}' 'https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}//published_models/{published_model_id}/deployments/{deployment_id}/online'
```
{: codeblock}

Exemple de sortie :

```
{
   "fields": ["GENDER",
      "AGE",
      "MARITAL_STATUS",
      "PROFESSION",
      "PROFESSION_IX",
      "GENDER_IX",
      "MARITAL_STATUS_IX",
      "features",
      "rawPrediction",
      "probability",
      "prediction",
      "predictedLabel"
   ],
   "records":[
      [
         "M",
         23,
         "Single",
         "Student",
         6.0,
         0.0,
         1.0,
         [
            0.0,
            23.0,
            1.0,
            6.0
         ],
         [
            5.600716147152702,
            6.482458372136143,
            6.048004170730676,
            0.20929155492307386,
            1.6595297550574055
         ],
         [
            0.2800358073576351,
            0.32412291860680714,
            0.3024002085365338,
            0.010464577746153694,
            0.08297648775287028
         ],
         1.0,
         "Accessoires personnels"
      ],
      [
         "M",
         55,
         "Single",
         "Executive",
         3.0,
         0.0,
         1.0,
         [
            0.0,
            55.0,
            1.0,
            3.0
         ],
         [
            6.227653744886563,
            4.325198862164969,
            8.031953760146816,
            1.2319759269281225,
            0.1832177058735289
         ],
         [
            0.3113826872443282,
            0.2162599431082485,
            0.40159768800734086,
            0.06159879634640614,
            0.009160885293676447
         ],
         2.0,
         "Equipements d'alpinisme montagne"
      ]
   ]
}
```
{: codeblock}

Nous pouvons constater, par exemple, qu'un cadre âgé de 55 ans est intéressé par les produits de la catégorie Equipements d'alpinisme, tandis qu'un étudiant âgé de 23 ans s'intéresse à la catégorie Accessoires personnels. 
