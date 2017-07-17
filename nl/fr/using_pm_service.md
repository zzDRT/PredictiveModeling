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

# Utilisation du service Machine Learning avec des modèles IBM SPSS Modeler

Les méthodes de modélisation disponibles dans la palette de modélisation de SPSS Modeler
vous permettent de puiser de nouvelles informations dans vos données et de développer des
modèles prédictifs. Chaque méthode possède ses propres avantages et
est donc plus adaptée à certains types de problème spécifiques.
{: .shortdesc}

Pour plus d'informations
sur SPSS Modeler et les algorithmes de modélisation qu'il utilise, reportez-vous à la documentation du site
[IBM Knowledge Center](https://www.ibm.com/support/knowledgecenter/SS3RA7).

Une fois que vous avez implémenté les exigences en matière d'entrée et de sortie de votre application Bluemix et de conception de la branche de scoring SPSS Modeler, votre analyste de données peut modifier n'importe quel aspect interne de cette branche. Il peut même modifier le ou les algorithmes de modélisation utilisés dans une opération d'actualisation afin que vous puissiez ensuite affiner vos analyses prédictives sans devoir réécrire vos applications.

Prenez en compte les informations importantes ci-dessous relatives à l'utilisation du service Machine
Learning avec des modèles créés dans SPSS Modeler :

*  Lorsqu'une branche d'évaluation est préparée
pour être utilisée dans une évaluation en temps réel,
les données d'entrée dans la demande d'évaluation doivent
remplacer le noeud source conçu dans la branche d'évaluation
et la sortie de l'analyse prédictive doit être renvoyée dans le flux de
réponse (afin de remplacer le noeud terminal dans la conception de branche d'évaluation).

*  La branche d'évaluation étant préparée pour une exécution en temps réel dans Bluemix,
elle ne peut pas nécessiter de connexion avec un
service externe. Par exemple, une conception de branche d'évaluation
IBM Analytical Decision Management ne peut pas contenir de références
à des règles ou des modèles stockés dans un référentiel IBM SPSS Collaboration and
Deployment Services.

*  L'exécution d'une branche d'évaluation pour l'évaluation en temps réel dans
Bluemix ne peut pas nécessiter de connexion avec un service externe. Par exemple, vous
ne pouvez pas effectuer de déploiement et d'évaluation par rapport à des algorithmes
de modèle qui nécessitent un serveur IBM SPSS
Analytic Server et un magasin de données Apache Hadoop en temps réel.

*  Machine Learning prend en charge les scripts Python imbriqués dans Modeler.

   Quelques restrictions sont présentes en raison de la méthode utilisée pour le traitement
des flux avant leur exécution dans Machine Learning. En règle générale, si un utilisateur choisit de
contrôler l'exécution du flux, il fait référence au noeud terminal de la branche.

   Pour Machine Learning, lorsque nous traitons le flux, nous identifions les noeuds
issus de JSON qui seront remplacés, puis effectuons le remplacement avant l'exécution
du flux. Cela provoque l'échec du flux dans le script car l'entrée référencée et les
noeuds d'exportation n'existent plus. La solution consiste à utiliser l'ID d'un autre
noeud afin d'identifier de manière unique la branche lors de l'exécution. Cela garantit
que le flux s'exécute conformément à sa définition dans le script
Python imbriqué.

Pour plus d'informations sur la prise en charge actuelle des modèles prédictifs sous apprentissage IBM SPSS Analytic
Server, reportez-vous à la section Analytic Server sur le site IBM Knowledge Center.

1. Vous pouvez télécharger l'exemple de code Node.js pour vous familiariser avec le service Machine
Learning. Pour créer votre application Bluemix et lier le service Machine Learning, procédez comme suit.

Les exemples mentionnés ici utilisent Node.js car c'est un contexte d'exécution couramment utilisé.

   Vous pouvez en utiliser d'autres tels que iOS, Ruby, Perl ou Java.

2. Utilisez la commande cf create-service pour créer une instance de service :

   ```
   cf create-service pm-20 Free {local naming}
   ```

   Par exemple :

   ```
   cf create-service pm-20 Free my_pm_free
   ```

   Cette commande dans votre espace Bluemix crée une instance de service Machine Learning nommée
my_pm_free avec le plan gratuit (Free).

3. Utilisez la commande `cf create-service-key` pour créer des données d'identification pour le service :

   ```cf create-service-key "{service instance name}" {vcap key name}```

   Par exemple :

   ```cf create-service-key "IBM Watson Machine Learning - my instance" Credentials-1```

Cette commande crée les données d'identification du service Machine Learning.

4. Utilisez la commande cf bind-service pour lier l'instance de service my_pm_free à votre application.

   ```cf bind-service AppName my_pm_service```

   Par exemple :

   ```cf bind-service my_app1 my_pm_free```

   Cette commande lie l'instance de service Machine Learning nommée `my_pm_free` à l'application Bluemix my_app1.

5. Données d'identification Machine Learning :

   Après avoir lié l'instance de service Machine Learning à votre application Bluemix, les données d'identification Machine Learning sont ajoutées à la variable d'environnement `VCAP_SERVICES` :

```
    {   
        "pm-20": {
            "name": "pm20-1",
            "label": "pm-20",
            "plan": "Free",
            "credentials": {
                "url": "https://ibm-watson-ml.mybluemix.net",
                "access_key": "XXXXXXXXXXXXX"
            }
        }       
    } 
```

   La variable d'environnement `VCAP_SERVICES` contient les informations suivantes :

   <dl>
   
   <dt>plan</dt>
   <dd>Plan Machine Learning utilisé pour la mise à disposition du service.</dd>

   <dt>url</dt>
   <dd>Adresse de l'instance de service Machine Learning.</dd>

   <dt>access_key</dt>
   <dd>Paramètre de requête accessKey via lequel sont transmises toutes les requêtes à cette instance de service.</dd>

   </dl>
            
Par exemple :             

```
Get https://ibm-watson-ml.mybluemix.net/pm/v1/model/sales_model2?accesskey=XXXXXXXXXXXXX 
```

   Exemple de code Node.js illustrant comment obtenir la valeur de accessKey depuis la variable d'environnement `VCAP_SERVICES` :

```
   if (process.env.VCAP_SERVICES) {
        var env = JSON.parse(process.env.VCAP_SERVICES);
        var credentials = env['pm-20'][0].credentials;
        var accessKey = credentials.access_key;
    }
```
