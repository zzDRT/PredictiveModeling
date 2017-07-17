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

# Modèles persistants


*  [Persistance de modèle et contrôle des versions](#model-persistence-and-version-control)

   *  [Génération du jeton d'accès](#generating-the-access-token)

   *  [Création des métadonnées du pipeline](#creating-pipeline-metadata)

   *  [Création d'une version du pipeline](#creating-pipeline-version)

   *  [Téléchargement de contenu du pipeline](#uploading-pipeline-content)

   *  [Création des métadonnées du modèle de pipeline](#creating-pipeline-model-metadata)

   *  [Création d'une version du modèle de pipeline](#creating-pipeline-model-version)

   *  [Téléchargement du contenu du pipeline](#uploading-pipeline-model-content)

Les spécialistes des données s'évertuent à améliorer continuellement leurs modèles dans le cadre de leurs activités de recherche et développement. Ils peuvent ajouter de nouvelles fonctions et optimiser les paramètres du modèle.
Lorsque les spécialistes des données développent des modèles
en mode itératif, le suivi des modifications peut rapidement devenir un problème. Nous allons décrire ici des techniques pouvant aider un spécialiste des données en fournissant une gestion des versions de modèle et en maintenant bien organisée toute la procédure.

Avant de commencer, si vous vous intéressez tout d'abord au développement de modèles, reportez-vous aux dossiers suivants :

*  [Developing SparkML models with Python](https://apsportal.ibm.com/exchange/public/entry/view/d80de77f784fed7915c14353512ef14d) (Développement de modèles SparkML avec Python)

*  [Developing SparkML models with Scala](https://apsportal.ibm.com/exchange/public/entry/view/d80de77f784fed7915c1435351309e93) (Développement de modèles SparkML avec Scala)

## Persistance de modèle et contrôle des versions

### Génération du jeton d'accès

Générez un jeton d'accès à partir de l'ID et du mot de passe utilisateur affichés dans l'onglet Données d'identification pour le service de l'instance de service IBM Watson Machine Learning. 

Exemple de requête :

```
curl --basic --user nom_utilisateur:mot_de_passe https://ibm-watson-ml.mybluemix.net/v2/identity/token
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

Ensuite, pour sauvegarder le pipeline et le modèle de pipeline, vous allez créer leurs métadonnées, créer le pipeline et la version de modèle du pipeline et télécharger le pipeline et la version de modèle du pipeline. Reportez-vous aux sections ci-dessous pour plus d'informations.

### Création des métadonnées du pipeline

Pour créer des métadonnées pour votre pipeline, décrivez les propriétés de base de votre pipeline dans une requête
curl comme illustré dans l'exemple suivant :

```
curl -i \
-X POST \
-H 'Content-Type: application/json' \
-H "Authorization: Bearer $access_token" \
-d '{
  "name": "exemple de pipeline",
  "description": "exemple de description",
  "author": {
    "name": "John Smith",
    "email": "john.smith@example.com"
  },
  "type": "sparkml-pipeline-2.0",
  "runtimeEnvironment": "spark-2.0"
}' \
https://ibm-watson-ml.mybluemix.net/v2/artifacts/pipelines
```
{: codeblock}

Exemple de réponse :

```
 HTTP/1.1 201 Created
 X-Backside-Transport: OK OK
 Connection: Keep-Alive
 Transfer-Encoding: chunked
 Content-Type: application/json
 Date: Fri, 17 Feb 2017 10:46:16 GMT
 Location: https://ibm-watson-ml.stage1.mybluemix.net/v2/artifacts/pipelines/223b38a6-41c6-417c-91ad-51064261b6f7
 Server: nginx/1.11.5
 X-Global-Transaction-ID: 3172115887
* Connexion #0 à l'hôte host ibm-watson-ml.stage1.mybluemix.net conservée intacte
{"ok":"true"}
```
{: codeblock}

### Création d'une version du pipeline

Pour créer une version de votre pipeline, spécifiez l'élément parentVersionHref pour votre
pipeline dans une requête curl comme illustré dans l'exemple suivant :

```
curl -i \
-X POST \
-H 'Content-Type: application/json' \
-H "Authorization: Bearer $access_token" \
-d '{}' \
https://ibm-watson-ml.mybluemix.net/v2/artifacts/pipelines/1314f74d-a2a7-46d3-8f11-89baa1d7ffea/versions
```
{: codeblock}

Exemple de réponse :

```
 HTTP/1.1 201 Created
 X-Backside-Transport: OK OK
 Connection: Keep-Alive
 Transfer-Encoding: chunked
 Content-Type: application/json
 Date: Fri, 17 Feb 2017 10:47:56 GMT
 Location: https://ibm-watson-ml.stage1.mybluemix.net/v2/artifacts/pipelines/223b38a6-41c6-417c-91ad-51064261b6f7/versions/0bb830fc-bc1e-439a-b929-685d9fc7712d
 Server: nginx/1.11.5
 X-Global-Transaction-ID: 3510865585
* Connexion #0 à l'hôte ibm-watson-ml.stage1.mybluemix.net conservée intacte
{"ok":"true"}
```
{: codeblock}

### Téléchargement du contenu du pipeline

Pour télécharger le contenu du pipeline, celui-ci doit être au format binaire. Pour ce faire, appelez la méthode
save depuis le pipeline SparkML. Vous pouvez télécharger votre contenu binaire en indiquant l'ID de votre pipeline et sa version dans un noeud final, comme illustré dans l'exemple ci-après :

```
curl -i \
-X PUT \
-H 'Content-Type: application/json' \
-H "Authorization: Bearer $access_token" \
-d <path_to_your_binary_content> \
https://ibm-watson-ml.mybluemix.net/v2/artifacts/pipelines/1314f74d-a2a7-46d3-8f11-89baa1d7ffea/versions/a535417c-ba8f-43b5-aae5-7dd8af02f518/content
```
{: codeblock}

Exemple de réponse :

```
 HTTP/1.1 200 OK
 X-Backside-Transport: OK OK
 Connection: Keep-Alive
 Transfer-Encoding: chunked
 Content-Type: application/json
 Date: Fri, 17 Feb 2017 10:50:53 GMT
 Server: nginx/1.11.5
 X-Global-Transaction-ID: 980490895
* Connexion #0 à l'hôte ibm-watson-ml.stage1.mybluemix.net conservée intacte
{"ok":"true"}
```
{: codeblock}

Une fois que vous avez sauvegardé le pipeline, vous pouvez continuer et sauvegarder sa version.

### Création des métadonnées du modèle de pipeline

Vous devez décrire le schéma de données de votre modèle de pipeline, comme illustré dans l'exemple suivant :

```
curl -i \
-X POST \
-H 'Content-Type: application/json' \
-H "Authorization: Bearer $access_token" \
-d '{
  "name": "exemple de modèle",
  "description": "exemple de description",
  "author": {
    "name": "John Smith",
    "email": "john.smith@example.com"
  },
  "pipelineVersionHref": "string",
  "type": "sparkml-model-2.0",
  "runtimeEnvironment": "spark-2.0",
  "trainingDataSchema": {
    "type": "struct",
    "fields": [
      {
        "name": "id",
        "type": "double",
        "nullable": true,
        "metadata": {}
      },
      {
        "name": "review",
        "type": "string",
        "nullable": true,
        "metadata": {}
      },
      {
        "name": "label",
        "type": "int",
        "nullable": true,
        "metadata": {}
      }
    ]
  },
  "labelCol": "string",
  "inputDataSchema": {
    "type": "struct",
    "fields": [
      {
        "name": "id",
        "type": "double",
        "nullable": true,
        "metadata": {}
      },
      {
        "name": "review",
        "type": "string",
        "nullable": true,
        "metadata": {}
      }
    ]
  }
}' \
https://ibm-watson-ml.mybluemix.net/v2/artifacts/models
```
{: codeblock}

Exemple de réponse :

```
 HTTP/1.1 201 Created
 X-Backside-Transport: OK OK
 Connection: Keep-Alive
 Transfer-Encoding: chunked
 Content-Type: application/json
 Date: Fri, 17 Feb 2017 10:58:29 GMT
 Location: https://ibm-watson-ml.stage1.mybluemix.net/v2/artifacts/models/ce274d29-49d0-43d8-adf7-6d9afa73c9cb
 Server: nginx/1.11.5
 X-Global-Transaction-ID: 437931687
* Connexion #0 à l'hôte ibm-watson-ml.stage1.mybluemix.net conservée intacte
{"ok":"true"}
```
{: codeblock}

### Création d'une version du modèle de pipeline

Pour créer une version de votre modèle de pipeline, utilisez une requête curl en spécifiant des informations comme l'emplacement de stockage de vos données d'apprentissage, ainsi que la méthode d'évaluation du modèle. Exemple :

```
curl -i \
-X POST \
-H 'Content-Type: application/json' \
-H "Authorization: Bearer $access_token" \
-d '{
  "trainingDataRef": {
    "connection":{
      "auth_url": "https://identity.open.softlayer.com",
      "project": "object_storage_008c8b63_b3d6_47c4_9da2_6c3ea6cfa713",
      "projectId": "f771e5d082e24857adb7554d15fc357c",
      "region": "dallas",
      "userId": "9b2e44721b91471ab86ecc41aeb7d37f",
      "username": "admin_926e1098fc72ff3c59d9222a2ab31a8a22143497",
      "password": "xxxxxxxxxxxxxxxx",
      "domainId": "a7b64174a5d74134b73b1533f6b99ae6",
      "domainName": "1143537",
      "role": "admin"
    },
   "source":{
      "container": "notebooks",
      "filename": "WA_Fn-UseC_-Telco-Customer-Churn.csv",
      "fileformat": "csv",
      "inferschema": 1,
      "firstlineheader": true
    }
  },
  "evaluation": {
    "method": "Binary",
    "metrics": [
      {
        "name": "areaUnderROC",
        "threshold": 0.8
      }
    ]
  }
}' \
https://ibm-watson-ml.mybluemix.net/v2/artifacts/models/30ed894b-4018-4c88-9e53-86005548317a/versions
```
{: codeblock}

Exemple de réponse :

```
 HTTP/1.1 201 Created
 X-Backside-Transport: OK OK
 Connection: Keep-Alive
 Transfer-Encoding: chunked
 Content-Type: application/json
 Date: Fri, 17 Feb 2017 11:10:12 GMT
 Location: https://ibm-watson-ml.stage1.mybluemix.net/v2/artifacts/models/ce274d29-49d0-43d8-adf7-6d9afa73c9cb/versions/8be22c6e-e214-45a4-8cfb-be4cade109ef
 Server: nginx/1.11.5
 X-Global-Transaction-ID: 285978151
* Connexion #0 à l'hôte ibm-watson-ml.stage1.mybluemix.net conservée intacte
{"ok":"true"}
```
{: codeblock}

### Téléchargement du contenu du modèle de pipeline

Pour télécharger le contenu du modèle de pipeline, ce modèle doit être au format binaire. Pour ce faire, appelez la méthode
save depuis le modèle de pipeline SparkML. Vous pouvez télécharger votre contenu binaire en indiquant l'ID de votre pipeline et sa version dans un noeud final, comme illustré dans l'exemple ci-après :

```
curl -i \
-X PUT \
-H 'Content-Type: application/json' \
-H "Authorization: Bearer $access_token" \
-d <path_to_your_binary_content> \
https://ibm-watson-ml.mybluemix.net/ v2/artifacts/pipelines/1314f74d-a2a7-46d3-8f11-89baa1d7ffea/versions/bb328cc3-b78d-4527-9dcc-f2172f43ae9e/content
```
{: codeblock}

Exemple de réponse :

```
 HTTP/1.1 200 OK
 X-Backside-Transport: OK OK
 Connection: Keep-Alive
 Transfer-Encoding: chunked
 Content-Type: application/json
 Date: Fri, 17 Feb 2017 11:11:23 GMT
 Server: nginx/1.11.5
 X-Global-Transaction-ID: 605948113
* Connexion #0 à l'hôte ibm-watson-ml.stage1.mybluemix.net conservée intacte
{"ok":"true"}
```
{: codeblock}
