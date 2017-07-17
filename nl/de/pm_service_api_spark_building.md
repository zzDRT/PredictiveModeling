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

# Vorhersageanalyseanwendungen erstellen


In diesem Abschnitt wird der Prozess der Bereitstellung
und Verwendung einer Beispielanwendung beschrieben.

**Szenarioname:** Produktlinienvorhersage.

**Szenariobeschreibung:** Unser Kunde führt
eine der bekanntesten Handelsketten in Europa. Er möchte, dass wir herausfinden, an welcher Produktlinie die Kunden jeweils interessiert sind, z. B. persönliche Accessoires, Campingausrüstung oder Outdoorschutz.
Ein Data-Scientist entwickelt ein Vorhersagemodell und
teilt es mit Ihnen (dem Entwickler). Ihre Aufgabe ist es, die
Node.js-Anwendung vorzubereiten und zu implementieren,
die Sportproduktlinien empfiehlt, indem Scoring-Anforderungen an das
bereitgestellte Modell gestellt werden.

1. Melden Sie sich bei Bluemix an und erstellen Sie eine Instanz von Machine Learning. 

2. Starten Sie das Machine Learning-Dashboard. 

3. Wechseln Sie zur Registerkarte Beispiele. 

4. Suchen Sie im Abschnitt Beispielmodelle die Kachel für die Produktlinienvorhersage
und klicken Sie auf die Schaltfläche Modell hinzufügen (+). Jetzt sehen Sie das Beispielmodell
zur Produktlinienvorhersage in der Liste mit verfügbaren Modellen auf der Registerkarte
Modelle. 

   **Hinweis:** Wenn Sie statt der bereitgestellten Beispiele Ihr
eigenes Data Science Experience-Projekt und Ihre eigenen Modelle verwenden möchten,
müssen Sie ein angepasstes Modell im Machine Learning-Repository als persistent definieren. Dies können Sie entweder mithilfe der
REST-API oder mithilfe der Clientbibliotheken realisieren. Detaillierte Informationen hierzu finden Sie unter
Entwicklung und Persistenz des angepassten Modells.

5. Wählen Sie im Menü Aktion den Eintrag Bereitstellung erstellen aus,
um das Modell zur Produktlinienvorhersage als Onlinebereitstellung zu implementieren. 

6. Geben Sie den Bereitstellungsnamen an (z. B. Product Line Prediction) und klicken Sie auf
Speichern. Sie sollten jetzt die Bereitstellung Product Line Prediction in
der Liste von Bereitstellungen sehen. 

7. Wählen Sie im Menü Aktion den Eintrag Details anzeigen für Ihre Bereitstellung aus. Der im Detailteilfenster angezeigte
Scoring-Endpunkt wird zum Ausführen von Scoring-Anforderungen verwendet. 

8. Zum Ausführen von Scoring-Anforderungen über die REST-API, sind
ein Scoring-Endpunkt und ein Berechtigungstoken erforderlich. Weitere Informationen
finden Sie unter Online-Modelle bereitstellen.

9. Sie können jetzt mit der Node.js-Beispielanwendung experimentieren, die sich unter
folgender Adresse befindet: 
   https://github.com/pmservice/product-line-prediction.

   Zum automatischen Bereitstellen von Beispielanwendungscode in Ihrem Bluemix-Bereich
wechseln Sie zur Registerkarte Beispiele, wählen im Abschnitt Beispielanwendungen die Anwendung
Produktlinienvorhersage aus und implementieren sie, indem Sie auf die Schaltfläche
(+) klicken.
Authentifizieren Sie sich im DeployToBluemix-Formular, falls Sie dazu
aufgefordert werden.

   Jetzt sollten Sie die Beispielanwendung im Bluemix-Dashboard im Abschnitt
Alle Apps sehen. 

10. Klicken Sie auf die Anwendung, um Details einzublenden. Hier können
Sie Anwendungsdetails wie die Anzahl von Instanzen, den Speicher
usw, ändern.

11. Jetzt können Sie Ihre Anwendung an die
Machine Learning-Instanz binden. Klicken Sie auf der Registerkarte
Verbindungen auf Vorhandenen verbinden. Nach
dem erneuten Staging ist Ihre Anwendung mit der Serviceinstanz
verbunden.

12. Führen Sie die Anwendung mithilfe der Routenadresse aus.

13. Wählen Sie in der Anwendung Ihre Bereitstellung 'Product Line
Prediction' aus. Jetzt können Sie sich mit Beispieldatensätzen und
Vorhersageergebnissen vertraut machen.
