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

# Construction d'applications d'analyse prédictive


Cette section décrit la marche à suivre pour déployer et utiliser un exemple d'application.

**Nom du scénario **:  Pronostic de ligne de produits.

**Description du scénario **: notre client gère l'une des chaînes de magasins les plus connues en
Europe. Il souhaite que nous identifions les intérêts de ses clients en ce qui concerne leurs lignes de produits, par exemple
Accessoires personnels, Matériel de camping, Vêtements de plein air.
Un spécialiste des données développe un modèle prédictif et le partage avec vous (le développeur). Votre tâche consiste à préparer et à déployer l'application Node.js, laquelle recommande des lignes de produits sportifs en émettant des requêtes de score au modèle déployé.

1. Connectez-vous à Bluemix et créez une instance de Machine Learning.

2. Lancez le tableau de bord Machine Learning.

3. Accédez à l'onglet Exemples.

4. Dans la section Exemples de modèles, recherchez la vignette Pronostics de ligne de produits et cliquez sur le bouton Ajouter le modèle (+).Vous pouvez observer à présent le modèle Pronostics de ligne de produits dans la liste des modèles disponibles sous l'onglet Modèles.

   **Remarque **: vous désirez utiliser votre propre projet et modèles Data Science Experience au lieu des exemples, vous devez pérenniser un modèle personnalisé dans le référentiel Machine Learning. Vous pouvez effectuer cette opération à l'aide de l'API
REST ou de bibliothèques client. Pour plus d'informations, voir Développement et persistance du modèle personnalisé.

5. Dans le menu Action, sélectionnez Créer un déploiement pour déployer le modèle
Pronostic de ligne de produits en tant que déploiement en ligne.

6. Indiquez un nom pour le déploiement (par exemple, Pronostic de ligne de produits) et cliquez sur
Sauvegarder. Le déploiement Pronostics de ligne de produits devrait à présent être affiché dans la liste Déploiements.

7. Dans le menu Action, sélectionnez Afficher les détails pour votre déploiement. Le noeud final d'évaluation présenté dans le panneau d'informations détaillées sera utilisé pour exécuter des requêtes d'évaluation.

8. Pour exécuter des requêtes d'évaluation via l'API REST, un noeud final d'évaluation et un jeton d'autorisation sont requis. Pour plus d'informations, voir Déploiement de modèles en ligne.

9. Vous pouvez à présent vous faire la main avec l'exemple d'application Node.js, disponible sur 
   https://github.com/pmservice/product-line-prediction.

   Pour déployer automatiquement le code de l'exemple d'application dans votre espace Bluemix, accédez à l'onglet Exemples et, dans la section Exemples d'applications, sélectionnez l'application Pronostic de ligne de produits et déployez-la en cliquant sur le bouton (+).
Authentifiez-vous sur le formulaire DeployToBluemix si vous y êtes invité.

   L'exemple d'application devrait maintenant être affiché sur le Tableau de bord Bluemix dans la section Toutes les applis.

10. Cliquez sur l'application pour afficher ses détails. Vous pouvez modifier ici des éléments comme le nombre d'instances, la mémoire, etc.

11. Vous pouvez à présent lier votre application à l'instance Machine Learning. Dans l'onglet Connexions, cliquez sur Connecter un existant. Après reconstitution, votre application est connectée à l'instance de service.

12. Exécutez l'application en utilisant l'adresse de routage.

13. Dans l'application, sélectionnez votre déploiement Pronostic de ligne de produits. Vous pouvez maintenant vous exercer avec des exemples d'enregistrement et des résultats de pronostic.
