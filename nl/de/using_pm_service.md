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

# Machine Learning-Service mit IBM SPSS Modeler-Modellen verwenden

Die in der SPSS Modeler-Modellierungspalette verfügbaren
Modellierungsmethoden
ermöglichen Ihnen die Ableitung neuer Informationen aus Ihren Daten
und die Entwicklung von Vorhersagemodellen. Jede Methode weist bestimmte Stärken auf und eignet sich für spezielle Problemtypen. 
{: .shortdesc}

Details zum SPSS Modeler und den von ihm bereitgestellten Modellierungsalgorithmen finden Sie im [IBM
Knowledge Center](https://www.ibm.com/support/knowledgecenter/SS3RA7).

Nachdem die Ein- und Ausgabeanforderungen Ihrer Bluemix-Anwendung
und das Design der Scoring-Verzweigung für SPSS Modeler
implementiert wurden, kann der Datenanalyst jeden internen Aspekt der
Scoring-Verzweigung ändern. Der Datenanalyst kann sogar die Modellalgorithmen ändern, die in einer Aktualisierungsoperation verwendet werden und so sicherstellen, dass die Vorhersageanalyse optimiert werden kann, ohne dass die Anwendungen neu programmiert werden müssen.

Beachten Sie die folgenden wichtigen Informationen bezüglich des Machine Learning-Service,
wenn er mit in SPSS Modeler erstellten Modellen eingesetzt wird: 

*  Beim Vorbereiten einer Scoring-Verzweigung für die Verwendung im Echtzeitscoring müssen Eingabedaten, die mit der Scoring-Anforderung eingehen, den Quellenknoten ersetzen, der in der Scoring-Verzweigung entworfen wurde, und die sich ergebene Vorhersageanalysee muss wieder in den Antwortablauf übergeben werden (wobei sie den Endknoten in der Gestaltung der Scoring-Verzweigung ersetzt).

*  Da die Scoring-Verzweigung für die Echtzeitausführung in Bluemix vorbereitet ist, kann sie keine Verbindung zu einem externen Service anfordern. Ein IBM Analytical Decision Management-Scoring-Verzweigungsdesign kann keine Referenzen auf Regeln oder andere Modelle enthalten, die in einem IBM SPSS Collaboration- oder
Deployment Services-Repository gespeichert sind.

*  Die Ausführung einer Scoring-Verzweigung für Echtzeitscoring in Bluemix kann keinen externen Service erfordern. Sie können keine Modellalgorithmen bereitstellen oder bewerten, die einen IBM SPSS
Analytic Server und Apache Hadoop-Data-Store in Echtzeit erfordern.

*  Machine Learning unterstützt Modeler-integriertes Python-Scripting. Es gibt einige Einschränkungen wegen der Methode,
die für die Verarbeitung von Datenströmen vor deren Ausführung in Machine Learning verwendet wird. Wenn ein Benutzer auswählt, dass er die Ausführung des Datenstroms steuert, wird in der Regel der Endknoten des Zweigs referenziert.
   Wenn bei Machine Learning der Datenstrom verarbeitet wird, kennzeichnen wir die Knoten von JSON,
die überschrieben werden, und ersetzen diese dann, bevor der Datenstrom ausgeführt
wird. Auf diese Weise schlägt der Datenstrom im Script fehl, da die referenzierten Eingabe- und Ausgabeknoten nicht mehr vorhanden sind. Die Lösung ist die Verwendung der ID eines anderen Knotens, um den Zweig während der Ausführung eindeutig anzugeben. Dies stellt sicher, dass der Datenstrom wie im integrierten Python-Script definiert ausgeführt wird.

Weitere Details zur aktuellen Unterstützung für IBM SPSS Analytic
Server-trainierte Vorhersagemodelle finden Sie im Abschnitt Analytic Server
im IBM Knowledge Center.

1. Sie können Node.js-Beispielcode herunterladen, um den
   Machine Learning-Service zu testen. Führen Sie die folgenden Schritte aus, um Ihre Bluemix-Anwendung
   zu erstellen und den Machine Learning-Service zu binden. Die hier aufgeführten Beispiele verwenden Node.js, weil es sich dabei um eine gängige Laufzeitkomponente handelt.
   Es können stattdessen auch andere Komponenten verwendet werden, beispielsweise iOS, Ruby, Perl oder Java.

2. Verwenden Sie den Befehl cf create-service, um eine Serviceinstanz zu erstellen: 

   ```
   cf create-service pm-20 Free {local naming}
   ```

   Beispiel:

   ```
   cf create-service pm-20 Free my_pm_free
   ```

   Dieser Befehl erstellt eine Machine Learning-Serviceinstanz mit einem kostenlosen Plan (Free),
dem in Ihrem Bluemix-Bereich der Name my_pm_free zugeordnet ist. 

3. Verwenden Sie den Befehl `cf
create-service-key`, um Serviceberechtigungsnachweise zu
erstellen:

   ```cf create-service-key "{service instance name}" {vcap key name}```

   Beispiel:

   ```cf create-service-key "IBM Watson Machine Learning - my instance" Credentials-1```

Dieser Befehl erstellt Machine Learning-Serviceberechtigungsnachweise.

4. Verwenden Sie den Befehl cf bind-service, um die Serviceinstanz my_pm_free an Ihre Anwendung
zu binden. 

   ```cf bind-service AppName my_pm_service```

   Beispiel:

   ```cf bind-service my_app1 my_pm_free```

Dieser Befehl bindet die Machine Learning-Serviceinstanz `my_pm_free`
an die Bluemix-Anwendung my_app1.

5. Machine Learning-Berechtigungsnachweise:

   Nachdem Sie die Machine Learning-Serviceinstanz an Ihre Bluemix-Anwendung gebunden haben, werden die Machine Learning-Berechtigungsnachweise zur Umgebungsvariablen `VCAP_SERVICES` hinzugefügt: 

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

   Die Umgebungsvariable `VCAP_SERVICES` umfasst die folgenden Informationen:

   <dl>
   
   <dt>plan</dt>
   <dd>Der Machine Learning-Plan, der für die Servicebereitstellung verwendet wird. </dd>

   <dt>url</dt>
   <dd>Die Adresse der Machine Learning-Serviceinstanz. </dd>

   <dt>access_key</dt>
   <dd>Der Abfrageparameter accessKey, der in allen Anforderungen an diese Serviceinstanz übergeben
werden muss. </dd>

   </dl>
            
Beispiel:             

```
Get https://ibm-watson-ml.mybluemix.net/pm/v1/model/sales_model2?accesskey=XXXXXXXXXXXXX 
```

   Node.js-Beispielcode, in dem dargestellt ist, wie der Zugriffsschlüssel (accessKey)
aus der Umgebungsvariablen `VCAP_SERVICES` abgerufen werden kann: 

```
   if (process.env.VCAP_SERVICES) {
        var env = JSON.parse(process.env.VCAP_SERVICES);
        var credentials = env['pm-20'][0].credentials;
        var accessKey = credentials.access_key;
    }
```
