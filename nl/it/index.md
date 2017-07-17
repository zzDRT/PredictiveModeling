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

# Introduzione a Watson Machine Learning
{: #WMLgettingstarted}

Utilizza IBM Watson™ Machine Learning per integrare l'analisi predittiva con le tue
applicazioni. I data scientist utilizzano il machine learning per sviluppare applicazioni che effettuano decisioni più mirate, risolvono problemi complicati e migliorano i risultati degli utenti.
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



Il servizio Machine Learning consiste in un insieme di API REST che possono essere richiamate
da qualsiasi linguaggio di programmazione.

L'obiettivo del servizio Machine Learning è la distribuzione, ma
tieni presente che è necessario IBM SPSS Modeler o Data Science Experience per
la creazione e l'utilizzo di modelli e pipeline. SPSS
Modeler e Data Science Experience (che utilizza Spark MLlib e Python scikit-learn)
offrono entrambi diversi metodi di modellazione derivanti dal machine
learning, dall'intelligenza artificiale e dalle statistiche.

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

Sei pronto a iniziare? Per creare un'istanza di un servizio o per eseguire il bind
di un'applicazione, vedi [Utilizzo del servizio con i modelli Spark e Python](using_pm_service_dsx.html) oppure
[Utilizzo del servizio con i modelli SPSS](using_pm_service.html).

Se sei interessato ad esplorare l'API, vedi [API del servizio Machine Learning
per i modelli Spark e Python](pm_service_api_spark.html) oppure [API del servizio Machine Learning
per i modelli IBM SPSS Modeler](pm_service_api_spss.html).

Per i dettagli su SPSS Modeler e sugli algoritmi di modellazione che fornisce, consulta [IBM
Knowledge Center](https://www.ibm.com/support/knowledgecenter/SS3RA7).

Per i dettagli su IBM Data Science Experience e sugli algoritmi di
modellazione che fornisce, vedi [https://datascience.ibm.com](https://datascience.ibm.com).
