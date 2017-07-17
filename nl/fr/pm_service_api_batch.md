---

copyright:
  years: 2016, 2017
lastupdated: "2017-06-21"

---
<!-- Copyright info and last updated date at top of file: REQUIRED
    The copyright and lastupdated info is YAML content that must occur at the top of the MD file, before attributes are listed.
    It must be --- surrounded by 3 dashes ---
    The value "years" can contain just one year or a two years separated by a comma. (years: 2014, 2016)
    The value "lastupdated" must be followed by a machine date in quotes in the following format: "YYYY-MM-DD"
    The value for "years" must be indented 2 spaces under "copyright", followed by "lastupdated" which should start on its own non-indented line.

-->

<!-- Common attributes used in the template are defined as follows: -->
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# API de travail par lots du service Machine Learning pour les modèles IBM SPSS Modeler


*  [Suppression de travaux](#deleting-jobs)

*  [Vérification du statut d'un travail](#checking-the-status-of-a-job)

*  [Nouvelle soumission d'un travail](#resubmit-a-job)

*  [Soumission d'un travail vis à vis d'un fichier de flux Modeler téléchargé](#submit-a-job-against-an-uploaded-modeler-stream-file)

*  [Téléchargement d'un fichier de flux pour son utilisation dans vos travaux](#upload-a-stream-file-to-use-in-your-jobs)

*  [Types de travail](#job-types)

*  [Etat de travail](#job-status)

*  [Détails d'API de travail](#job-api-details)

*  [JSON de définition de travail](#job-definition-json)

*  [Détails de l'API de travail par lots](#batch-job-api-details)

L'API de travail par lots pour le service Machine Learning prend en charge les tâches à exécution longue relatives à l'apprentissage par le modèle, son évaluation et l'évaluation par lots. Cette API gère deux types d'actif : les fichiers de flux SPSS Modeler utilisés dans les travaux par lots et les définitions de travail soumises. Pour chaque fichier de flux Modeler téléchargé, il se peut que de nombreux travaux de différents types soient soumis. Si un travail a la capacité de mettre à jour le contenu d'un fichier de flux Modeler, le fichier modifié sera inclus dans les pièces jointes de résultat de travail. Dans un type de travail d'évaluation de modèle, tous les fichiers d'évaluation générés figurent dans les pièces jointes de résultat de travail.

Pour un exemple d'adoption de travail par lots, reportez-vous au dossier suivant :
[From SPSS stream to batch scoring with
Python](https://apsportal.ibm.com/analytics/notebooks/9d7ce38e-9417-4c76-a6b9-5bc8cf40938a/view?access_token=5ca87e3007804e5b2bbbce77c20e99ac3c164d66f2d28dfffb54aa365caaef37).

## Suppression de travaux

Vous pouvez supprimer des travaux, ce qui entraîne leur annulation s'ils sont en cours d'exécution. Appelez la méthode DELETE sur l'ID de travail (vous pouvez annuler plusieurs travaux à la fois) :

DELETE https://{PA Bluemix load balancer URL}/pm/v1/jobs/{job ID
1, job ID 2,...}?accesskey=xxxxxxxxxx

Les données obtenues en retour indiquent combien de travaux référencés par un ID dans la demande ont été supprimés. Si ce nombre ne correspond pas à la liste de travaux transmise via la demande, vous devez vérifier le statut de chacun des travaux.

## Vérification du statut d'un travail

Vous pouvez appeler une méthode GET à n'importe quel moment pour identifier le statut de votre ID de travail :

GET https://{PA Bluemix load balancer URL}/pm/v1/jobs/{job
ID}/status?accesskey=xxxxxxxxxx

L'objet JSON renvoyé indique l'ID de travail (jobstatus) et, si le travail a abouti, une URL de données (dataUrl) que vous pouvez utiliser pour obtenir le contenu complet du fichier généré.

## Nouvelle soumission d'un travail

Appelez la méthode PUT pour l'<ID de travail>. Le travail ne doit pas être en cours d'exécution :

PUT https://{PA Bluemix load balancer URL}/pm/v1/jobs/{job
ID}?accesskey=xxxxxxxxxx

avec content_type "application/json" en incluant dans le corps de la demande la nouvelle définition de travail JSON (ou celle mise à jour).

En réalité, cet appel crée un nouveau travail si l'ID référencé n'existe pas, et le code réponse 201 ou 200 est renvoyé pour indiquer lequel de ces travaux a été exécuté. Vous devez transmettre le JSON de définition de travail à utiliser dans cette exécution.

## Soumission d'un travail vis à vis d'un fichier de flux Modeler téléchargé

Appelez la méthode PUT pour votre définition de travail afin de la placer dans la file d'attente d'exécution :

PUT https://{PA Bluemix load balancer URL}/pm/v1/jobs/{job
ID}?accesskey=xxxxxxxxxx

avec content_type "application/json" en incluant dans le corps de la demande la définition de travail JSON.

Cette demande génère un retour immédiat, indiquant que l'opération a abouti si la définition de travail a été placée dans la file d'attente d'exécution. Il n'existe aucun test permettant de vérifier la capacité à exécuter le flux Modeler tel qu'il est configuré dans votre définition de travail. Le travail sera exécuté par l'un des serveurs de travaux Machine Learning dans le cloud et vous pourrez surveiller son statut. Si vous effectuez une formation de modèle et indiquez qu'une opération d'actualisation automatique est nécessaire, le travail remplacera le fichier de flux Modeler d'origine à la fin de son exécution.

Pour plus d'informations sur l'objet JSON de définition de travail, voir [Objet JSON de définition de travail](#job-definition-json).

## Téléchargement d'un fichier de flux à utiliser dans vos travaux

Remarque : le tableau de bord Machine Learning est destiné uniquement à l'évaluation en temps réel. Vous ne pouvez pas l'utiliser pour exécuter des travaux (évaluation par lots).

Appelez la méthode PUT pour rendre un fichier de flux Modeler accessible pour les travaux :

PUT https://{PA Bluemix load balancer URL}/pm/v1/file/{file
ID}?accesskey=xxxxxxxxxx

Avec content_type "multipart/form-data" pour transmettre le fichier dans la demande.

Le nom unique utilisé (ID de fichier dans l'appel PUT) est ce à quoi vous faites référence dans un appel
DELETE à l'API de fichier, ainsi que la référence de modèle dans vos définitions de travaux. Notez que vous contrôlez pleinement l'espace de nom des fichiers utilisé dans vos travaux, de sorte que l'appel PUT d'un fichier
avec le même <ID de fichier> remplace implicitement la copie actuellement détenue.

Vous pouvez effectuer une opération GET afin d'obtenir la liste de tous les fichiers téléchargés pour vos travaux en omettant
l'<ID fichier> ou extraire un fichier spécifique à l'aide d'une opération GET en indiquant l'ID de ce fichier. Vous pouvez également effectuer une opération DELETE sur un fichier téléchargé (bien entendu, ceci générera des erreurs lors de l'exécution de travaux en attente qui font référence au fichier supprimé).

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

## Types de travail

Training : ce type de travail indique que tous les noeuds terminal "générateur de modèle" doivent être exécutés dans le flux Modeler. Une fois le travail exécuté, un flux Modeler mis à jour avec des nuggets de modèle nouvellement formés figurent dans les résultats de travail pouvant être extraits. Si le fichier de flux Modeler comporte des liens entre les noeuds génération de modèle et les nuggets de modèle formés dans l'évaluation et les branches d'évaluation, cela provoque une actualisation de ces noeuds.

Evaluation : Ce type de travail déclenche l'exécution de tous les noeuds terminal "générateur de documents" (principalement depuis les onglets Graphiques et Sortie dans le client Modeler) qui génèrent un contenu de fichier de rapport statique pouvant être transmis à l'appelant.
La branche d'évaluation n'est pas considérée comme faisant partie de ce type de travail.

Auto-Refresh : version du type de travail Apprentissage où, si le travail abouti, le fichier de flux
Modeler d'origine dans la liste du fichier de traitement par lots sera mis à jour. La décision d'évaluation explicite concernant un événement d'actualisation des flux Modeler déployés pour l'évaluation en temps réel est supposée et elle n'est pas abordée dans la section sur l'actualisation automatique à ce stade.

Batch Score : exécution du noeud terminal auquel vous avez appliqué l'option Utiliser en tant que branche d'évaluation, indiquant qu'il s'agit de la branche d'évaluation dans cette conception de flux Modeler. La définition de travail doit spécifier les détails relatifs à l'exportation et à la source.

Run Stream : même effet que si vous cliquez sur le bouton vert "Exécuter"
dans Modeler avec l'option Exécuter ce script sélectionnée dans l'onglet
Exécution des propriétés de flux. L'utilisation couvre les besoins de l'exécution de script de formation de modèle ou d'autres types de travail. Tout le contrôle dynamique du script doit être géré par des paramètres de flux, avec des valeurs de paramètre transmises dans la définition de travail.

## Etat de travail

En attente : la  définition de travail a été soumise mais n'a pas encore été réclamée par un serveur de travaux pour son exécution. 

En cours d'exécution : la définition de travail a été réclamée par un serveur de travaux et est en cours d'exécution. 

En cours d'annulation : le travail est en cours d'annulation.

Annulé : le travail a été annulé.

Echec : Le travail a échoué. La cause détaillée de l'échec est renvoyée par une opération GET sur le statut du travail.

Réussite : L'exécution du travail a abouti. Le résultat éventuel communiqué pour cet événement est renvoyé dans l'objet
JSON de l'opération GET sur le statut du travail.

## Détails d'API de travail

POST /v1/jobs/{id}

Description : soumet une description de travail pour exécution. Une erreur est générée si l'ID de travail (<job ID>) existe déjà.

Content Types:

```
@Consumes({“application/json”})
@Produces({“application/json”})
```
{: codeblock}

Parameters:

Clé d'accès renvoyée en tant que données d'identification pour la mise à disposition ou la liaison :

```
@QueryParam("accesskey")
```
{: codeblock}

ID de travail spécifié par l'utilisateur. Doit être unique pour l'instance de service Machine Learning :

```
@PathParam("id")
```
{: codeblock}

Objet JSON de définition de travail (voir Objet JSON de définition de travail) :

```
@BodyParam
```
{: codeblock}

Réponses :

Réussite. Le travail est soumis pour exécution et un JSON décrivant le travail soumis est renvoyé.

```
@ApiResponse(code = 200)
```
{: codeblock}

Erreur indiquant que le travail existe déjà. Un travail portant cet ID a déjà été soumis. Le message renvoyé décrit cette erreur.

```
@ApiResponse(code = 409)
```
{: codeblock}

Autre erreur. Un JSON d'exception est renvoyé.

```
@ApiResponse(code = 500)
```
{: codeblock}

PUT /v1/jobs/{id}

Description : Création ou mise à jour d'un travail. S'il n'existe aucun travail portant cet ID, créez-le ; sinon, mettez-le à jour (dans ce cas, le travail est soumis à nouveau pour exécution).

Content Types:

```
@Consumes({“application/json”})
@Produces({“application/json”})
```
{: codeblock}

Parameters:

Clé d'accès renvoyée en tant que données d'identification pour la mise à disposition ou la liaison :

```
@QueryParam("accesskey")
```
{: codeblock}

ID de travail défini par l'utilisateur :

```
@PathParam("id")
```
{: codeblock}

Objet JSON de définition de travail (voir Objet JSON de définition de travail) :

```
@BodyParam
```
{: codeblock}

Réponses :

Réussite. Le travail a été mis à jour et soumis pour exécution. Renvoie l'objet
JSON décrivant le travail soumis.

```
@ApiResponse(code = 200)
```
{: codeblock}

Réussite. Le travail a été créé et soumis pour exécution. Renvoie l'objet
JSON décrivant le travail soumis.

```
@ApiResponse(code = 201)
```
{: codeblock}

Autre erreur. Un JSON d'exception est renvoyé.

```
@ApiResponse(code = 500)
```
{: codeblock}

GET /v1/jobs

Description : renvoie la liste de tous les travaux définis sur cette instance de service Machine Learning.

Content Types:

```
@Produces({“application/json”})
```
{: codeblock}

Parameters:

Clé d'accès renvoyée en tant que données d'identification pour la mise à disposition ou la liaison :

```
@QueryParam("accesskey")
```
{: codeblock}

Réponses :

Réussite. Un tableau JSON répertoriant toutes les définitions de travail est renvoyé.

```
@ApiResponse(code = 200)
```
{: codeblock}

Autre erreur. Un JSON d'exception est renvoyé :

```
@ApiResponse(code = 500)
```
{: codeblock}

GET /v1/jobs/{id}

Description : demande le renvoi d'une définition de travail spécifique.

Content Types:

```
@Produces({“application/json”})
```
{: codeblock}

Parameters:

Clé d'accès renvoyée en tant que données d'identification pour la mise à disposition ou la liaison :

```
@QueryParam("accesskey")
```
{: codeblock}

ID de travail soumis :

```
@PathParam("id")
```
{: codeblock}

Réponses :

Réussite. Un JSON décrivant le travail référencé est renvoyé.

```
@ApiResponse(code = 200)
```
{: codeblock}

Erreur de travail introuvable. Détails dans le message renvoyé :

```
@ApiResponse(code = 404)
```
{: codeblock}

Autre erreur. Un JSON d'exception est renvoyé :

```
@ApiResponse(code = 500)
```
{: codeblock}

GET /v1/jobs/{id}/status

Description : extrait le statut d'un travail spécifique.

Content Types:

```
@Produces({“application/json”})
```
{: codeblock}

Parameters:

Clé d'accès renvoyée en tant que données d'identification pour la mise à disposition ou la liaison :

```
@QueryParam("accesskey")
```
{: codeblock}

ID de travail soumis :

```
@PathParam("id")
```
{: codeblock}

Réponses :

Réussite. Un JSON décrivant le statut du travail est renvoyé:

```
@ApiResponse(code = 200)
```
{: codeblock}

Erreur de travail introuvable. Détails dans le message renvoyé :

```
@ApiResponse(code = 404)
```
{: codeblock}

Autre erreur. Un JSON d'exception est renvoyé :

```
@ApiResponse(code = 500)
```
{: codeblock}

DELETE /v1/jobs/{ids}

Description : supprime un ou plusieurs travaux. Lorsqu'il est supprimé, un travail en cours d'exécution est annulé.

Content Types:

```
@Produces({“application/json”})
```
{: codeblock}

Parameters:

Clé d'accès renvoyée en tant que données d'identification pour la mise à disposition ou la liaison :

```
@QueryParam("accesskey")
```
{: codeblock}

ID de travail soumis ou liste d'ID de travail avec des valeurs d'ID séparées par une virgule (,)

```
@PathParam("id" or “id,id,id”)
```
{: codeblock}

Réponses :

Réussite. Renvoie le nombre de travaux supprimés :

```
@ApiResponse(code = 200)
```
{: codeblock}

Autre erreur. Un JSON d'exception est renvoyé :

```
@ApiResponse(code = 500)
```
{: codeblock}

## JSON de définition de travail

Le JSON de définition de travail contient les sections générales suivantes :

Référence de modèle prédictif et de type de travail

Types d'action :

*  TRAINING – Exécute un apprentissage du noeud du générateur de modèle ou l'actualisation des nuggets de modèle. Le flux Modeler mis à jour est extrait dans les résultats de travail.

*  EVALUATION – Exécute le générateur de document et les noeuds d'analyse qui évaluent le modèle ayant suivi un apprentissage.

*  AUTO_REFRESH – Effectue un apprentissage et met à jour le contenu de fichier dans l'espace de téléchargement de fichier de traitement par lots.

*  BATCH_SCORE – Exécute la branche d'évaluation et exporte les données obtenues comme indiqué par la définition de travail. 

*  RUN-STREAM – Exécute le flux Modeler comme indiqué dans les propriétés de flux ; utilisé la plupart du temps lorsque l'exécution de script est requise.

Model : ID tel que spécifié dans l'action de téléchargement de fichier pour la référence de travail par lots. Pour plus d'informations, voir Téléchargement d'un fichier de flux à utiliser dans vos travaux.
Notez que /pm/v1/file est utilisé.

```
"action": "TRAINING",
"model": {
     "id": "str2"
     "name":"stream1.str"
},
```
{: codeblock}

Notez que l'ID doit être identique à l'ID fichier utilisé dans l'API PUT. Le nom n'est pas obligatoire, mais dans le cas d'apprentissage par le modèle et d'actualisation, le résultat du travail sera sauvegardé sous le nom défini ici. S'il n'a pas été défini, le service Machine Learning générera le résultat compte tenu de règles de nommage prédéfinies.

Paramètres de travail

Tous les paramètres requis pour l'exécution de ce travail.

```
"setting": {
     … Export settings
     … Import settings
     … Parameter value settings
     … Document control settings
}
```
{: codeblock}

Définitions de connectivité de base de données

Type de base de données : DB2, DashDB, Informix, Oracle, Sybase, SQLServer, MySQL.

Connectivité spécifiée dans l'instance de service de base de données ; des paramètres spécifiques doivent être indiqués dans les 'Options' pour de nombreux types de base de données

```
"dbDefinitions":{
     "db1":{
          "type":"DashDB",
          "host":"dashdb-enterprise-yp-dal09-11.services.dal.bluemix.net",
          "port":50001,
          "db":"BLUDB",
          "username":"bluadmin",
          "password":"NmI4MDViMzBkNDUz",
          "options":"Security=SSL"
     },
     "db2": {
          "type": "SQLServer",
          "db": "qatest",
          "host": "9.20.27.37",
          "port": 1433,
          "username": "sa",
          "password": "Pass1234",
          "options": "EnableQuotedIdentifiers=1"
     }
},
```
{: codeblock}

Paramètres de noeud source

Connectivité et table de base de données de référence utilisées pour définir la source d'une branche définie dans le flux Modeler identifiée par le nom de noeud source.

```
"inputs": [
     {
          "odbc": {
               "dbRef”; “db2”,
               "table": "DRUG1N",
          },
          "node": "ScoreInput",
          "attributes": []
     }
],
```
{: codeblock}

Le paramètre table désigne le nom de la table de base de données. Cette table permet de remplacer le noeud source de flux. Le paramètre node désigne le nom de source de données pour le flux. Le paramètre dbRef fait référence à la connectivité de base de données définie par l'objet dbDefinitions, et le paramètre table désigne la table de base de données utilisée comme entrée de la branche définie dans le flux Modeler. Le paramètre node identifie le noeud source d'origine dans le flux Modeler qui doit être remplacé par un noeud source de base de données construit avec les paramètres fournis. 

Paramètres de noeud d'exportation

Connectivité et table de base de données de référence utilisées pour conserver les données d'une branche définie dans le flux Modeler identifiée par le nom de noeud terminal.

Méthode de contrôle utilisée lorsque des données persistantes sont communiquées via l'attribut insertMode :

*  Append – la table doit exister et être compatible pour insertion

*  Create – la table est créée (une erreur est renvoyée si elle existe déjà)

*  Drop – si la table référencée existe, elle est supprimée et recréée

*  Refresh – les lignes existantes de la table sont supprimées avant l'insertion de nouvelles lignes

```
"exports": [
     {
          "odbc": {
               "dbRef”; “db1”,
               "table": "DRUGSCORES",
               “insertMode”:”Append”
          },
          "node": "ExportScores",
          "attributes": [],
     }
],
```
{: codeblock}

Le paramètre table désigne le nom de la table de base de données dans laquelle consigner les résultats du travail. Le paramètre node désigne le nom du noeud terminal pour le flux. Comme pour les paramètres de noeud source, le paramètre node identifie le noeud de sortie d'origine dans le flux Modeler qui doit être remplacé par un noeud d'exportation de base de données construit avec les paramètres fournis.

Remarque : les paramètres de noeud d'exportation et de noeud source sont importants pour permettre l'exécution correcte de votre travail. 

Substitutions de valeur de paramètre

Afin de remplacer les valeurs par défaut pour les paramètres de niveau de flux avant l'exécution d'un travail, vous pouvez indiquer les paires nom/valeur à utiliser dans la définition de travail.

```
"parameterOverride": [
     {
          "name": "p1",
          "value": "v1"
     },
     {
          "name": "p2",
          "value": "v2"
     }
],
```
{:codeblock}

Formats de rapport souhaités pour le document d'évaluation renvoyé lors de l'exécution d'un type de travail d'évaluation

Tous les documents générés à partir de l'exécution du travail d'évaluation sont regroupés dans un fichier .zip. Seuls les noeuds de générateur de document capables de prendre en charge le format reportFormat indiqué sont exécutés et leur contenu renvoyé après l'aboutissement du travail.

Les formats pris en charge sont HTML, JPG, PNG, RTF, SAV, TAB et XML.

```
"reportFormat": "HTML"
```
{: codeblock}

Exemple JSON complet

```
{ 
     "action": "TRAINING",
     "model": {
          "id": "str2"
          "name": "stream1.str"
     },
     "dbDefinitions":{
          "db":{
                    "type":"DashDB",
                    "host":"dashdb-enterprise-yp-dal09-11.services.dal.bluemix.net",
                    "port":50001,
                    "db":"BLUDB",
                    "username":"bluadmin",
                    "password":"NmI4MDViMzBkNDUz",
                    "options":"Security=SSL"
               }
     },
     "setting": {
          "inputs": [
                    {
                         "odbc": {
                                   "dbRef”; “db”,
                                   "table": "DRUG1N",
                         },
                         "node": "ScoreInput",
                         "attributes": []
                    }
          ],
          "parameterOverride": [
                    {
                        "name": "p1",
                        "value": "v1"
                    },
                    {
                        "name": "p2",
                        "value": "v2"
                    }
          ],
     }
}
```
{: codeblock}

## Détails de l'API de travail par lots

Les sections ci-après fournissent des informations détaillées sur l'API de gestion de fichier de SPSS Modeler pour travaux par lots.

PUT /v1/file/{id}

Description : télécharge un fichier de flux SPSS Modeler à utiliser dans des travaux par lots.

Remarque : si un fichier est déjà stocké sous l'ID spécifié, il est remplacé.

Content Types:

```
@Consumes({ "multipart/form-data"  })
@Produces({“application/json”})
```
{: codeblock}

Parameters:

Clé d'accès renvoyée en tant que données d'identification pour la mise à disposition ou la liaison :

```
@QueryParam("accesskey") 
```
{: codeblock}

ID de fichier spécifié par l'utilisateur. Doit être unique dans le référentiel d'instances de service :

```
@PathParam("id") 
```
{: codeblock}

Réponses :

Réussite. Renvoie l'URL qui peut être utilisée pour télécharger le fichier depuis le référentiel de l'instance de service :

```
@ApiResponse(code = 200)
```
{: codeblock}

Autre erreur. Un JSON d'exception est renvoyé :

```
@ApiResponse(code = 500)
```
{: codeblock}

DELETE /v1/file/{id}

Description : supprime du référentiel d'instances de service le fichier stocké sous l'ID spécifié.

Content Types:

```
@Produces({“application/json”})
```
{: codeblock}

Parameters:

Clé d'accès renvoyée en tant que données d'identification pour la mise à disposition ou la liaison :

```
@QueryParam("accesskey")
```
{: codeblock}

ID spécifié par l'utilisateur pour le fichier lors de son téléchargement :

```
@PathParam("id") 
```
{: codeblock}

Réponses :

Réussite. Le fichier indiqué a été supprimé :

```
@ApiResponse(code = 200)
```
{: codeblock}

Erreur. Un fichier stocké sous cet ID n'existait pas dans le référentiel d'instance de service :

```
@ApiResponse(code = 404)
```
{: codeblock}

Autre erreur. Un JSON d'exception est renvoyé :

```
@ApiResponse(code = 500)
```
{: codeblock}

GET /v1/file/

Description : renvoie la liste des ID de tous les fichiers gérés dans le référentiel d'instances de service pour le traitement de travail par lots.

Content Types:

```
@Produces({“application/json”})
```
{: codeblock}

Parameters:

Clé d'accès renvoyée en tant que données d'identification pour la mise à disposition ou la liaison :

```
@QueryParam("accesskey")  
```
{: codeblock}

Réponses :

Réussite. Renvoie un tableau JSON des valeurs d'ID de fichier :

```
@ApiResponse(code = 200)
```
{: codeblock}

Autre erreur. Un JSON d'exception est renvoyé :

```
@ApiResponse(code = 500)
```
{: codeblock}

GET /v1/file/{id}

Description : extrait le fichier de flux SPSS Modeler stocké pour utilisation dans le traitement de travail par lots sous l'ID spécifié.

Content Types:

```
@Produces({ "application/octet-stream" })
```
{: codeblock}

Parameters:

Clé d'accès renvoyée en tant que données d'identification pour la mise à disposition ou la liaison :

```
@QueryParam("accesskey")  
```
{: codeblock}

ID spécifié par l'utilisateur pour le fichier lors de son téléchargement :

```
@PathParam("id") 
```
{: codeblock}

Réponses :

Réussite. Le fichier SPSS Modeler est renvoyé :

```
@ApiResponse(code = 200)
```
{: codeblock}

Erreur. Un fichier stocké sous cet ID n'existait pas dans le référentiel d'instance de service :

```
@ApiResponse(code = 404)
```
{: codeblock}

Autre erreur. Un JSON d'exception est renvoyé :

```
@ApiResponse(code = 500)
```
{: codeblock}
