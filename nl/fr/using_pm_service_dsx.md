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

# Utilisation du service Machine Learning avec des modèles Spark et Python


Prenez en compte les informations importantes ci-dessous relatives au service Machine Learning :

*  Infrastructures Machine Learning prises en charge :

  *  Spark 2.0 MLlib
  *  Python 3 avec scikit-learn 0.17

*  Les modèles non supervisés ne sont pas pris en charge.

*  Le déploiement par lots et en flux de modèles Python scikit-learn n'est pas pris en charge à ce stade. 

*  La prise en charge de lots et de flux en est actuellement à la version bêta. Si vous désirez participer, inscrivez-vous sur la liste d'attente. Pour plus d'informations,  voir : [https://www.ibm.biz/mlwaitlist](https://www.ibm.biz/mlwaitlist).

Vous pouvez télécharger l'exemple de code Node.js pour vous familiariser avec le service Machine
Learning. Pour créer votre application Bluemix et lier le service Machine Learning, procédez comme suit. Les exemples mentionnés ici utilisent Node.js car c'est un contexte d'exécution couramment utilisé. Vous pouvez en utiliser d'autres tels que iOS, Ruby, Perl ou Java.

Notez que vous pouvez également réaliser les étapes 1 à 3 via l'interface graphique de Bluemix au lieu de l'outil Cloud Foundry
(cf).

1. Utilisation de la commande cf create-service pour créer une instance de service :

   ```
   cf create-service pm-20 Free {désignation locale}
   ```
{: codeblock}

   Par exemple :

   ```
   cf create-service pm-20 Free my_wml_free
   ```
{: codeblock}

   Cette commande crée dans votre espace Bluemix une instance de service Machine Learning nommée ```my_wml_free``` avec le plan gratuit (Free).

2. Utilisez la commande cf create-service-key pour créer les données d'identification du service :

   ```
   cf create-service-key "{nom_instance_service}" {nom_clé_vcap}
   ```
{: codeblock}

   Par exemple :

   ```
   cf create-service-key "IBM Watson Machine Learning - my instance" Credentials-1
   ```
{: codeblock}

   Cette commande crée les données d'identification du service Machine Learning.

3. Utilisez la commande cf bind-service pour lier l'instance de service : 
   ```my_wml_free``` to your application.

   ```
   cf bind-service {nom_application} my_wml_free
   ```
{: codeblock}

   Par exemple :

   ```
   cf bind-service my_app1 my_wml_free
   ```
{: codeblock}

   Cette commande lie l'instance de service Machine Learning nommée my_wml_free à l'application Bluemix my_app1.



Données d'identification Machine Learning :

   Après avoir lié l'instance de service Machine Learning à votre application Bluemix, les données d'identification Machine Learning sont ajoutées à la variable d'environnement VCAP_SERVICES :

```
    {
     "pm-20-dev": [
       {
         "credentials": {
           "url":
           "https://ibm-watson-ml.mybluemix.net",
           "access_key": "**********",
           "username": "**********",
           "password": "**********",
           "instance_id": "**********"
         },
           "label": "pm-20-dev",
           "plan": "Free",
           "name": "IBM Watson Machine Learning”
       }
     ]
    }
```
{: codeblock}

   La variable d'environnement VCAP_SERVICES inclut les informations suivantes :

   * plan - plan Machine Learning utilisé pour mise à disposition du service.

   * url - adresse de l'instance de service Machine Learning.

   * access_key - (clé d'accès) pour flux SPSS Modeler uniquement.

   * instance_id - identificateur unique de l'instance Machine Learning.

   * username, password (nom utilisateur, mot de passe) - autorisation de base requise pour générer un jeton d'accès pour transmettre toutes les requêtes à cette instance de service. Vous pourriez, par exemple, générer un jeton d'accès avec une commande curl similaire à ceci :

Exemple de requête :

```
       curl --basic --user nom_utilisateur:mot_de_passe https://ibm-watson-ml.mybluemix.net/v2/identity/token

       Exemple de sortie :

       {"token":"**********"}
```
{: codeblock}

   Le code Node.js ci-après illustre comment obtenir le nom d'utilisateur et le mot de passe depuis la variable d'environnement VCAP_SERVICES :

```
   if (process.env.VCAP_SERVICES) {
        var env = JSON.parse(process.env.VCAP_SERVICES);
        var credentials = env['pm-20'][0].credentials;
        var username = credentials.username;
        var password = credentials.password;
        var instance_id = credentials.instance_id;
    }
```
{: codeblock}
