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

# Développement et persistance du modèle personnalisé

## Utilisation de modèles personnalisés

### Nom du scénario : Pronostic de ligne de produits.

### Description du scénario : 

Notre client gère l'une des chaînes de magasins les plus connues en Europe. Il souhaite que nous identifions les intérêts de ses clients en ce qui concerne leurs lignes de produits, par exemple
Accessoires personnels, Matériel de camping, Vêtements de plein air.
Un spécialiste des données développe un modèle prédictif et le partage avec vous (le développeur). Votre tâche consiste à déployer le modèle et à générer des analyses prédictives en effectuant des requêtes de score à partir du modèle déployé.

### Développement et persistance du modèle personnalisé

1. Utilisez Data Science Experience pour créer des modèles personnalisés. Après vous être inscrit, connectez-vous et procédez comme suit.

2. A votre toute première connexion, vous serez invité à créer une organisation et un espace. Cliquez sur
**Continuer** et acceptez les valeurs par défaut.

3. Une fois que l'organisation a été créée, accédez à **Mes projets** et cliquez sur
**Créer un projet**.

4. Indiquez un nom et une description pour votre projet et cliquez sur **Créer**. Le nom de projet que vous spécifiez sera également utilisé comme nom de votre conteneur cible.

5. Une fois le projet créé, vous pouvez ajouter des dossiers et commencer à développer vos propres modèles ou télécharger l'un des exemples de dossier suivants :

   *  [Developing Spark MLlib models with Python](https://apsportal.ibm.com/analytics/notebooks/89492fd6-a641-4819-9176-3d9381561df9/view?access_token=d80bef1a172d1d83d3721b101886337158457281774186f181a2e6a5b57f5ec7) (Développement de modèles Spark MLlib avec Python)

   *  [Developing Spark MLlib models with Scala](https://apsportal.ibm.com/analytics/notebooks/c8652d2c-bfc9-4354-8168-f1c9f7f8dfc2/view?access_token=02a83fea8450a452c8de76af98dae078459d0f56810ddef4f4c62d5bc4fc72cf) (Développement de modèles Spark MLlib avec Scala)

   *  [Developing scikit-learn models with Python](https://apsportal.ibm.com/analytics/notebooks/5215a61a-16d7-4fa2-b060-e3e243ceebe3/view?access_token=70f48c95c5571a614ce97484d3f168b1d9b6aeebce015187d3d77ce6038f025e) (Développement de modèles scikit-learn avec Python)



### Déploiement et scoring du modèle personnalisé

Reportez-vous aux sections ci-après pour les instructions de déploiement et de scoring de modèles ou à la section
scoring des dossiers dont le lien a été indiqué plus haut.

*  [Déploiement de modèles en ligne](pm_service_api_spark_online.html)

*  [Déploiement de modèles de traitement par lots](pm_service_api_spark_batch.html)

*  [Déploiement de modèles de données en flux](pm_service_api_spark_streaming.html)
