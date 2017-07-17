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

# Déploiement de modèles de traitement par lots


**Remarque **: cette fonctionnalité est actuellement en version bêta et n'est disponible que pour son utilisation avec Spark MLlib. 

**Nom du scénario **: Pronostic de satisfaction des clients.

**Description du scénario **: Une entreprise de télécommunications désire savoir quels clients risquent de les abandonner. Le modèle présenté pronostique une attrition de la clientèle. Un spécialiste des données développe un modèle prédictif et le partage avec vous (le développeur). Votre tâche consiste à déployer le modèle et à générer des analyses prédictives en effectuant des requêtes de score à partir du modèle déployé.

## Utilisation de l'exemple de modèle

1. Accédez à l'onglet Exemples du tableau de bord IBM® Watson™ Machine Learning.

2. Dans la section Exemples de modèles, recherchez la vignette Pronostic de satisfaction des clients et cliquez sur le bouton Ajouter le modèle (+).

Vous pouvez observer à présent l'exemple de modèle Pronostic de satisfaction des clients dans la liste des modèles disponibles sous l'onglet Modèles.

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

Utilisez la commande de terminal suivante pour affecter la valeur de votre jeton à la variable d'environnement
access_token:

```
access_token="<valeur_jeton>"
```
{: codeblock}

## Création d'un déploiement par lots à l'aide d'Object Storage

Pour utiliser un appel d'API REST pour créer un déploiement par lots de votre modèle prédictif, fournissez les informations suivantes :

*  Le jeton d'accès créé à l'étape précédente

*  Les données d'identification pour le service Spark, que vous pouvez trouver dans l'onglet Données d'identification pour le service du tableau de bord du service Bluemix Spark. Avant de soumettre la demande de déploiement, les données d'identification Spark doivent être décodées en base64 et transmises dans l'en-tête d'une requête curL sous la forme
Instance-Service-X-Spark.

   Selon le système d'exploitation que vous utilisez, vous devez exécuter l'une des commandes de terminal suivantes pour effectuer un décodage en base64 et l'affecter à la variable d'environnement.

   Sur système d'exploitation macOS, utilisez la commande suivante :

   ```
   spark_credentials=$(echo '{"credentials": {"tenant_id": "s068-ade10277b64956-05b1d10fv12b","tenant_id_full": "00fd89e6-8cf2-4712-a068-ade10277b649_41f37bf2-1b95-4c65-a156-05b1d10fb12b","cluster_master_url": "https://spark.bluemix.net","instance_id": "00fd89e6-8cf2-4712-a068-ade10277b649","tenant_secret": "c74c37cf-482a-4da4-836e-f32ca26ccbb9","plan": "ibm.SparkService.PayGoPersonal"},"version": "2.0"}' | base64)
   ```
{: codeblock}

   Sur système d'exploitation Microsoft Windows ou Linux, vous devez utiliser le paramètre `--wrap=0` avec la commande `base64` pour effectuer un décodage en base64 :

   ```
   spark_credentials=$(echo '{"credentials": {"tenant_id": "s068-ade10277b64956-05b1d10fv12b","tenant_id_full": "00fd89e6-8cf2-4712-a068-ade10277b649_41f37bf2-1b95-4c65-a156-05b1d10fb12b","cluster_master_url": "https://spark.bluemix.net","instance_id": "00fd89e6-8cf2-4712-a068-ade10277b649","tenant_secret": "c74c37cf-482a-4da4-836e-f32ca26ccbb9","plan": "ibm.SparkService.PayGoPersonal"},"version": "2.0"}' | base64 --wrap=0)
   ```
{: codeblock}

*  Informations d'Object Storage, qui seront utilisées comme entrées (données client auxquelles attribuer un score) pour le modèle et
stockage de la sortie du modèle (results.csv, en l'occurrence, qui est créé automatiquement).

*  Pour créer un déploiement, utilisez le noeud final suivant : 

   ```
   /v3/wml_instances/{ID_instance}/modèles_publiés/{ID_modèle_publié}/deployments
   ```
   
   Le noeud final requis pour créer un déploiement en ligne est disponible sur votre tableau de bord WML -> Détails du modèle > URL. Notez également que vous pouvez identifier la valeur de ID_modèle_publié depuis le tableau de bord d'IBM Watson Machine Learning en cliquant sur Modèle -> Afficher les détails et copier la valeur figurant dans la zone ID et identifier votre "ID_instance" dans les données d'identification VCAP de l'instance Watson Machine Learning.

Exemple de requête :

```
curl -v -XPOST \
    -H "Content-Type:application/json" \
    -H "Authorization:Bearer $access_token" \
    -H "X-Spark-Service-Instance: $spark_credentials" \
    -d '{
      "name":"Pronostic de satisfaction des clients",
      "type": "batch",
      "description": "Déploiement par lots",
       "input":{
          "source":{
             "container":"batchjob",
             "filename":"TelcoCustomerData.csv",
             "fileformat":"csv",
             "inferschema":"1",
             "firstlineheader":"true",
             "type":"bluemixobjectstorage"
          },
          "connection":{
             "userid":"b2d83cf6056e040ddb91ca00a2686c7d3",
             "projectid":"252341ed707d4558b5b2da245e785cd7",
             "password":"eJ_y9R^OE{j?8Ub!!",
             "region":"dallas",
             "authurl":"https://identity.open.softlayer.com"
          }
       },
       "output":{
          "target":{
             "container":"batchjob",
             "filename":"result.csv",
             "fileformat":"csv",
             "writemode":"write",
             "firstlineheader":"true",
             "type":"bluemixobjectstorage"
          },
          "connection":{
             "userid":"b283cf6056e040ddb91ca00a2686c7d3",
             "projectid":"252341ed707d4558b5b2da245e785cd7",
             "password":"eJ_y9R^OE{j?8Ub!!",
             "region":"dallas",
             "authurl":"https://identity.open.softlayer.com"
          }
       }
    }' \
https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/{published_model_id}/deployments
```
{: codeblock}

Exemple de sortie :

```
{
   "metadata":{
      "guid":"60ad595a-2d53-4094-aeb0-be0273b3a8b1",
      "url":"https://ibm-watson-ml.mybluemix.net/v3/wml_instances/b5bd2368-cc4b-4cf1-aec0-49ed8daeb018/published_models/315231e8-e021-40dc-a250-6e4f9d1c11d7/deployments/60ad595a-2d53-4094-aeb0-be0273b3a8b1",
      "created_at":"2017-06-28T11:39:26.663Z",
      "modified_at":"2017-06-28T11:39:48.637Z"
   },
   "entity":{
      "runtime_environment":"spark-2.0",
      "name":"Pronostic de satisfaction des clients",
      "instance_href":"https://ibm-watson-ml.mybluemix.net/v2/batch/deployments/8e7ffee3-700c-4bbf-acfa-ccc0ad299dd7",
      "description":"Déploiement par lots",
      "published_model":{
         "author": {
            "name":"IBM",
            "email":""
         },
         "name":"Pronostic de satisfaction des clients",
         "url":"https://ibm-watson-ml.mybluemix.net/v3/wml_instances/b5bd2368-cc4b-4cf1-aec0-49ed8daeb018/published_models/315231e8-e021-40dc-a250-6e4f9d1c11d7",
         "guid":"315231e8-e021-40dc-a250-6e4f9d1c11d7",
         "description":"Pronostique une attrition de la clientèle de Telco.",
         "created_at":"2017-06-28T11:39:26.642Z"
      },
      "model_type":"sparkml-model-2.0",
      "status":"En cours d'initialisation",
      "output":{
         "target":{
            "fileformat":"csv",
            "firstlineheader":"true",
            "writemode":"write",
            "container":"batchjob",
            "filename":"customerOutput.csv",
            "type":"bluemixobjectstorage"
         },
          "connection":{
             "projectid":"cabb5f0c377d4ed080f917dc29605aad",
            "userid":"1d5815d49ea4479ab6f29cc1733db774",
            "region":"dallas",
            "authurl":"https://identity.open.softlayer.com",
            "password":"UF.D_/p9SKx5YPxw"
         }
      },
      "type":"batch",
      "deployed_version": {
         "url":"https://ibm-watson-ml.mybluemix.net/v2/artifacts/models/315231e8-e021-40dc-a250-6e4f9d1c11d7/versions/e52d8fac-c62c-4ace-9b86-afbbc85ea25f",
         "guid":"e52d8fac-c62c-4ace-9b86-afbbc85ea25f",
         "created_at":"2017-06-28T11:38:21.121Z"
      },
      "spark_service":{
         "credentials": {
            "tenant_id":"s000-5f656e4e614595-e0ddb27c0670",
            "cluster_master_url":"https://spark.bluemix.net",
            "tenant_id_full":"fba311a7-532e-4aad-9000-5f656e4e6145_bd856e56-b4f4-4e82-9b95-e0ddb27c0670",
            "tenant_secret":"d9627f06-7a25-46ed-b833-93fe799654c4",
            "instance_id":"fba311a7-532e-4aad-9000-5f656e4e6145",
            "plan":"ibm.SparkService.PayGoPersonal"
         },
         "version":"2.0"
      },
      "input":{
         "source":{
            "fileformat":"csv",
            "firstlineheader":"true",
            "container":"batchjob",
            "inferschema":"1",
            "filename":"customerInput.csv",
            "type":"bluemixobjectstorage"
         },
          "connection":{
             "projectid":"cabb5f0c377d4ed080f917dc29605aad",
            "userid":"1d5815d49ea4479ab6f29cc1733db774",
            "region":"dallas",
            "authurl":"https://identity.open.softlayer.com",
            "password":"UF.D_/p9SKx5YPxw"
         }
      }
   }
}
```
{: codeblock}

**Remarque **: vous pouvez également utiliser le tableau de bord pour créer un déploiement par lots. Vous devez soumettre les trois entrées suivantes :

Exemple de formulaire :

Input:

```
{
   "source":{
      "fileformat":"csv",
      "firstlineheader":"true",
      "container":"batchjob",
      "inferschema":"1",
      "filename":"TelcoCustomerData.csv",
      "type":"bluemixobjectstorage"
   },
          "connection":{
             "projectid":"252341ed707d4558b5b2da245e785cd7",
      "userid":"b2d83cf6056e040ddb91ca00a2686c7d3",
      "region":"dallas",
      "authurl":"https://identity.open.softlayer.com",
      "password":"eJ_y9R^OE{j?8Ub!!"
   }
}
```
{: codeblock}

Sortie :

```
{
   "target":{
      "fileformat":"csv",
      "firstlineheader":"true",
      "container":"batchjob",
      "inferschema":"1",
      "filename":"result.csv",
      "type":"bluemixobjectstorage"
   },
          "connection":{
             "projectid":"252341ed707d4558b5b2da245e785cd7",
      "userid":"b2d83cf6056e040ddb91ca00a2686c7d3",
      "region":"dallas",
      "authurl":"https://identity.open.softlayer.com",
      "password":"eJ_y9R^OE{j?8Ub!!"
   }
}
```
{: codeblock}

Données d'identification pour le service Spark :

```
{
      "tenant_id": "s745-299dcf850a6390-35c9a7ecf27a",
      "tenant_id_full": "ba3dde5a-ee64-4057-9749-299dcf850a63_4c55eb1c-d6fe-4f0a-9390-35c9a7ecf27a",
      "cluster_master_url": "https://spark.bluemix.net",
      "instance_id": "ba3dde5a-ee64-4057-9749-299dcf850a63",
      "tenant_secret": "c0cba7a4-7b19-46e6-9326-44c4f48aaf08",
      "plan": "ibm.SparkService.PayGoPersonal"
}
```
{: codeblock}

## Obtention des détails du déploiement

Vous pouvez examiner le statut, le lien href du noeud final et les paramètres associés au modèle de déploiement.

Exemple de requête :

```
curl -v -XGET -H "Content-Type:application/json" -H "Authorization:Bearer $access_token" https://ibm-watson-ml.mybluemix.net/v3/instances_wml/{ID_instance}/modèles_publiés/{ID_modèle_publié}/deployments/{ID_déploiement}
```
{: codeblock}

Exemple de sortie :

```
{
   "metadata":{
      "guid":"60ad595a-2d53-4094-aeb0-be0273b3a8b1",
      "url":"https://ibm-watson-ml.mybluemix.net/v3/wml_instances/b5bd2368-cc4b-4cf1-aec0-49ed8daeb018/published_models/315231e8-e021-40dc-a250-6e4f9d1c11d7/deployments/60ad595a-2d53-4094-aeb0-be0273b3a8b1",
      "created_at":"2017-06-28T11:39:26.663Z",
      "modified_at":"2017-06-28T11:39:48.637Z"
   },
   "entity":{
      "runtime_environment":"spark-2.0",
      "name":"Pronostic de satisfaction des clients",
      "instance_href":"https://ibm-watson-ml.mybluemix.net/v2/batch/deployments/8e7ffee3-700c-4bbf-acfa-ccc0ad299dd7",
      "description":"Déploiement par lots",
      "published_model":{
         "author": {
            "name":"IBM",
            "email":""
         },
         "name":"Pronostic de satisfaction des clients",
         "url":"https://ibm-watson-ml.mybluemix.net/v3/wml_instances/b5bd2368-cc4b-4cf1-aec0-49ed8daeb018/published_models/315231e8-e021-40dc-a250-6e4f9d1c11d7",
         "guid":"315231e8-e021-40dc-a250-6e4f9d1c11d7",
         "description":"Pronostique une attrition de la clientèle de Telco.",
         "created_at":"2017-06-28T11:39:26.642Z"
      },
      "model_type":"sparkml-model-2.0",
      "status":"COMPLETE",
      "output":{
         "target":{
            "fileformat":"csv",
            "firstlineheader":"true",
            "writemode":"write",
            "container":"batchjob",
            "filename":"customerOutput.csv",
            "type":"bluemixobjectstorage"
         },
          "connection":{
             "projectid":"cabb5f0c377d4ed080f917dc29605aad",
            "userid":"1d5815d49ea4479ab6f29cc1733db774",
            "region":"dallas",
            "authurl":"https://identity.open.softlayer.com",
            "password":"UF.D_/p9SKx5YPxw"
         }
      },
      "type":"batch",
      "deployed_version": {
         "url":"https://ibm-watson-ml.mybluemix.net/v2/artifacts/models/315231e8-e021-40dc-a250-6e4f9d1c11d7/versions/e52d8fac-c62c-4ace-9b86-afbbc85ea25f",
         "guid":"e52d8fac-c62c-4ace-9b86-afbbc85ea25f",
         "created_at":"2017-06-28T11:38:21.121Z"
      },
      "spark_service":{
         "credentials": {
            "tenant_id":"s000-5f656e4e614595-e0ddb27c0670",
            "cluster_master_url":"https://spark.bluemix.net",
            "tenant_id_full":"fba311a7-532e-4aad-9000-5f656e4e6145_bd856e56-b4f4-4e82-9b95-e0ddb27c0670",
            "tenant_secret":"d9627f06-7a25-46ed-b833-93fe799654c4",
            "instance_id":"fba311a7-532e-4aad-9000-5f656e4e6145",
            "plan":"ibm.SparkService.PayGoPersonal"
         },
         "version":"2.0"
      },
      "input":{
         "source":{
            "fileformat":"csv",
            "firstlineheader":"true",
            "container":"batchjob",
            "inferschema":"1",
            "filename":"customerInput.csv",
            "type":"bluemixobjectstorage"
         },
          "connection":{
             "projectid":"cabb5f0c377d4ed080f917dc29605aad",
            "userid":"1d5815d49ea4479ab6f29cc1733db774",
            "region":"dallas",
            "authurl":"https://identity.open.softlayer.com",
            "password":"UF.D_/p9SKx5YPxw"
         }
      }
   }
}
```
{: codeblock}

Le résultat du pronostic est enregistré dans un fichier .csv dans IBM Object Storage.
Un exemple de lignes figure ci-dessous.

Aperçu du fichier d'entrée :

```
customerID,gender,SeniorCitizen,Partner,Dependents,tenure,PhoneService,MultipleLines,InternetService,OnlineSecurity,OnlineBackup,DeviceProtection,TechSupport,StreamingTV,StreamingMovies,Contract,PaperlessBilling,PaymentMethod,MonthlyCharges,TotalCharges,Churn
7590-VHVEG,Female,0,Yes,No,1,No,No phone service,DSL,No,Yes,No,No,No,No,Month-to-month,Yes,Electronic check,29.85,29.85,No
5575-GNVDE,Male,0,No,No,34,Yes,No,DSL,Yes,No,Yes,No,No,No,One year,No,Mailed check,56.95,1889.5,No
3668-QPYBK,Male,0,No,No,2,Yes,No,DSL,Yes,Yes,No,No,No,No,Month-to-month,Yes,Mailed check,53.85,108.15,Yes
```
{: codeblock}

Aperçu du fichier de sortie :

```
InternetService, Contract, tenure, MonthlyCharges, Churn
Fiber optic, Month-to-month, 2, 70.7, 1
DSL, Two year, 58, 59.9, 0
DSL, Month-to-month, 1, 30.2, 1
DSL, Month-to-month, 17, 64.7, 1
DSL, Month-to-month, 13, 76.2, 0
DSL, Month-to-month, 25, 69.5, 1
Fiber optic, Month-to-month, 8, 80.65, 1
No, One year, 52, 20.4, 0
Fiber optic, Two year, 64, 111.6, 0
Fiber optic, Month-to-month, 1, 79.35, 1
```
{: codeblock}

## Suppression d'un déploiement par lots

Si vous n'en avez plus besoin, vous pouvez supprimer le déploiement à l'aide d'une requête similaire à l'exemple suivant.

Exemple de requête :

```
curl -v -XDELETE -H "Content-Type:application/json" -H
"Authorization:Bearer $access_token" https://ibm-watson-
ml.mybluemix.net/v3/instances_wml/{ID_instance}/modèles_publiés/{ID_modèle_publié}/deployments/{ID_déploiement}
```
{: codeblock}

Exemple de sortie :

```
HTTP/1.1 204 No Content
X-Backside-Transport: OK OK
Connection: Keep-Alive
Cache-Control: no-cache, no-store, must-revalidate
Date: Wed, 28 Jun 2017 11:54:19 GMT
Pragma: no-cache
Server: nginx/1.11.5
X-Content-Type-Options: nosniff
X-Xss-Protection: 1; mode=block
X-Global-Transaction-ID: 1600446575
```
{: codeblock}
