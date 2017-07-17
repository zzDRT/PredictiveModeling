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

<!-- This template is for getting started with a Bluemix service. It is a task template intended to document productive use of the service. It is not intended for discovery and conceptual information.  -->

# Initiation à Watson Machine Learning
{: #WMLgettingstarted}

Vous pouvez utiliser IBM Watson™ Machine Learning pour intégrer l'analyse prédictive avec vos applications. Les spécialistes des données utilisent l'apprentissage automatique pour développer des applications qui adoptent des décisions plus intelligentes, résolvent des problèmes plus épineux et améliorent les retombées pour l'utilisateur.
{: shortdesc}

<!-- If overview content is required, do not include it here. Put it in a separate "## About" section below the task section. -->

<!-- Task section: REQUIRED
The task section includes steps to integrate the service into the app.  
- With task-based, technical information, reduce the conversational style in favor of succinct and direct instructions.
- DO include the basic, most-common-use scenario steps to use the service or integrate it into the app.
- DO NOT include steps to add the service from the Bluemix catalog; we assume that the user already took steps in the UI to add the service.
- DO include code snippets in all languages that can be copied, as well as VCAP service info.  
- For additional tasks like configuring, managing, etc., add a task section (## Gerund_task_title) below the task section or "About" section if used. Use a task title such as "Configuring x", "Administering y", "Managing z". -->

<!-- You can include an optional prerequisites paragraph for any prerequisites to be met before integrating the service. For example: -->

<!-- Include a sentence to briefly introduce the steps. Examples: -->



Le service Machine Learning est un ensemble d'API pouvant être appelé depuis n'importe quel langage de programmation.

Le service Machine Learning met l'accent sur le déploiement, mais notez que
qu'IBM SPSS Modeler ou Data Science Experience sont requis pour la création et l'utilisation de modèles et de pipelines. SPSS
Modeler et Data Science Experience (qui exploite Spark MLlib et Python scikit-learn)
proposent tous deux diverses méthodes de modélisation issues de l'apprentissage automatique, de l'intelligence artificielle et des statistiques.

<!-- Related links section: REQUIRED.
Related links display in the upper right of the getting started page.
Ensure that you retain the lowercase anchor IDs (eg. {: #rellinks}) as shown in this template. These are used as IDs during transform and the doc framework keys off the IDs for display.
The headings coded here are not actually used. The doc framework provides the correct headings.
Also ensure that the related links stay in position at the end of this file or the doc framework will not display them properly.
Use {:new_window} for external links to open a new window.-->
<!-- Please delete all comments within the related links section to avoid breaking the build. Thanks. -->

<!--  Related Links
{: #rellinks} -->

<!-- ## Tutorials and Samples
{: #samples} -->

Prêt à commencer ? Pour créer une instance d'un service ou lier une application, voir
[Utilisation du service avec des modèles Spark et Python](using_pm_service_dsx.html) ou
[Utilisation du service avec des modèles SPSS](using_pm_service.html).

Si vous désirez explorer l'API, voir [API du service Machine Learning
pour les modèles Spark et Python](pm_service_api_spark.html) ou [API du service Machine Learning pour les modèles
IBM SPSS Modeler](pm_service_api_spss.html).

Pour plus d'informations sur SPSS Modeler et les algorithmes de modélisation qu'il utilise, reportez-vous à la documentation du site [IBM
Knowledge Center](https://www.ibm.com/support/knowledgecenter/SS3RA7).

Pour plus d'informations sur IBM Data Science Experience et les algorithmes de modélisation qu'il propose, accédez au site
[https://datascience.ibm.com](https://datascience.ibm.com).
