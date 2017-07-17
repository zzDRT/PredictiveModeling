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

# Modelle als persistent definieren


*  [Modellpersistenz und Versionssteuerung](#model-persistence-and-version-control)

   *  [Zugriffstoken generieren](#generating-the-access-token)

   *  [Pipeline-Metadaten erstellen](#creating-pipeline-metadata)

   *  [Pipeline-Version erstellen](#creating-pipeline-version)

   *  [Pipeline-Inhalt hochladen](#uploading-pipeline-content)

   *  [Pipeline-Metadaten für Modell erstellen](#creating-pipeline-model-metadata)

   *  [Pipeline-Modellversion erstellen](#creating-pipeline-model-version)

   *  [Pipeline-Modellinhalt hochladen](#uploading-pipeline-model-content)

Data-Scientists bemühen sich darum, ihre Modelle im Rahmen
von Forschung und Entwicklung zu verbessern. Sie fügen unter
Umständen neue Funktionen zu vorhandenen Modellen hinzu und
optimieren die Modellparameter.
Wenn Data-Scientists Modelle iterativ
entwickeln, kann es schnell schwierig werden, einen Überblick über die
Änderungen zu behalten. Im Folgenden zeigen wir Wege, wie
Data-Scientists mithilfe von Modellversionierung und guter
Organisation des gesamten Prozesses die Kontrolle
behalten.

Wenn Sie sich zunächst für die Modellentwicklung interessieren,
sollten Sie die folgenden Notizbücher lesen:

*  [Entwickeln von SparkML-Modellen mit Python](https://apsportal.ibm.com/exchange/public/entry/view/d80de77f784fed7915c14353512ef14d)

*  [Entwickeln von SparkML-Modellen mit Scala](https://apsportal.ibm.com/exchange/public/entry/view/d80de77f784fed7915c1435351309e93)

## Modellpersistenz und Versionssteuerung

### Zugriffstoken generieren

Generieren Sie mithilfe des Benutzers und des Kennworts, die auf der Registerkarte
Serviceberechtigungsnachweise der IBM® Watson™ Machine Learning-Serviceinstanz
angegeben sind, ein Zugriffstoken (access token). 

Anforderungsbeispiel:

```
curl --basic --user username:password https://ibm-watson-ml.mybluemix.net/v2/identity/token
```
{: codeblock}

Ausgabebeispiel:

```
{"token":"**********"}
```
{: codeblock}

Verwenden Sie den folgenden Terminalbefehl, um Ihren Tokenwert der
Umgebungsvariablen access_token zuzuweisen: 

```
access_token="<tokenwert>"
```
{: codeblock}

Anschließend erstellen Sie die Pipeline und die
Pipeline-Metadaten für das Modell ebenso wie die
Pipeline-Modellversion und laden diese hoch, um die Pipeline und das
Pipeline-Modell zu speichern. Details finden Sie in den folgenden
Abschnitten.

### Pipeline-Metadaten erstellen

Zum Erstellen von Metadaten für Ihre Pipeline beschreiben Sie die
grundlegenden Eigenschaften Ihrer Pipeline in einer
curl-Anforderung, wie im folgenden Beispiel gezeigt: 

```
curl -i \
-X POST \
-H 'Content-Type: application/json' \
-H "Authorization: Bearer $access_token" \
-d '{ 
  "name": "sample pipeline",
  "description": "sample description",
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

Beispielantwort:

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
* Connection #0 to host ibm-watson-ml.stage1.mybluemix.net left intact
{"ok":"true"}
```
{: codeblock}

### Pipeline-Version erstellen

Zum Erstellen einer Version für Ihre Pipeline
geben Sie parentVersionHref für Ihre Pipeline in einer
curl-Anforderung an, wie im folgenden Beispiel gezeigt: 

```
curl -i \
-X POST \
-H 'Content-Type: application/json' \
-H "Authorization: Bearer $access_token" \
-d '{}' \
https://ibm-watson-ml.mybluemix.net/v2/artifacts/pipelines/1314f74d-a2a7-46d3-8f11-89baa1d7ffea/versions
```
{: codeblock}

Beispielantwort:

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
* Connection #0 to host ibm-watson-ml.stage1.mybluemix.net left intact
{"ok":"true"}
```
{: codeblock}

### Pipeline-Inhalt hochladen

Damit Sie Pipeline-Inhalt hochladen können, muss Ihre Pipeline
ein binäres Format aufweisen. Rufen Sie dafür die Methode
save aus der SparkML-Pipeline auf. Sie können
Ihren binären Inhalt hochladen, indem Sie die ID Ihrer Pipeline und
die Version in einem Endpunkt bereitstellen, wie im folgenden
Beispiel gezeigt:

```
curl -i \
-X PUT \
-H 'Content-Type: application/json' \
-H "Authorization: Bearer $access_token" \
-d <path_to_your_binary_content> \
https://ibm-watson-ml.mybluemix.net/v2/artifacts/pipelines/1314f74d-a2a7-46d3-8f11-89baa1d7ffea/versions/a535417c-ba8f-43b5-aae5-7dd8af02f518/content
```
{: codeblock}

Beispielantwort:

```
 HTTP/1.1 200 OK
 X-Backside-Transport: OK OK
 Connection: Keep-Alive
 Transfer-Encoding: chunked
 Content-Type: application/json
 Date: Fri, 17 Feb 2017 10:50:53 GMT
 Server: nginx/1.11.5
 X-Global-Transaction-ID: 980490895
* Connection #0 to host ibm-watson-ml.stage1.mybluemix.net left intact
{"ok":"true"}
```
{: codeblock}

Nachdem Sie die Pipeline gespeichert haben, können Sie mit dem
Speichern Ihres Pipeline-Modells fortfahren.

### Pipeline-Metadaten für Modell erstellen

Sie müssen das Datenschema Ihres Pipeline-Modells wie im folgenden
Beispiel beschreiben:

```
curl -i \
-X POST \
-H 'Content-Type: application/json' \
-H "Authorization: Bearer $access_token" \
-d '{
  "name": "sample-model",
  "description": "sample description",
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

Beispielantwort:

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
* Connection #0 to host ibm-watson-ml.stage1.mybluemix.net left intact
{"ok":"true"}
```
{: codeblock}

### Pipeline-Modellversion erstellen

Wenn Sie eine Version für Ihr Pipeline-Modell
erstellen möchten, verwenden
Sie eine curl-Anforderung, um Details wie den
Speicherort Ihrer Trainingsdaten und die Modellevaluierungsmethode
anzugeben. Siehe das folgende Beispiel:

```
curl -i \
-X POST \
-H 'Content-Type: application/json' \
-H "Authorization: Bearer $access_token" \
-d '{
  "trainingDataRef": {
    "connection": {
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
    "source": {
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

Beispielantwort:

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
* Connection #0 to host ibm-watson-ml.stage1.mybluemix.net left intact
{"ok":"true"}
```
{: codeblock}

### Pipeline-Modellinhalt hochladen

Damit Sie Pipeline-Modellinhalt hochladen können, muss Ihr
Pipeline-Modell ein
binäres Format aufweisen. Rufen Sie dafür die Methode
save aus dem SparkML-Pipeline-Modell auf. Sie können
Ihren binären Inhalt hochladen, indem Sie die ID Ihrer Pipeline und
die Version in einem Endpunkt bereitstellen, wie im folgenden
Beispiel gezeigt:

```
curl -i \
-X PUT \
-H 'Content-Type: application/json' \
-H "Authorization: Bearer $access_token" \
-d <path_to_your_binary_content> \
https://ibm-watson-ml.mybluemix.net/ v2/artifacts/pipelines/1314f74d-a2a7-46d3-8f11-89baa1d7ffea/versions/bb328cc3-b78d-4527-9dcc-f2172f43ae9e/content
```
{: codeblock}

Beispielantwort:

```
 HTTP/1.1 200 OK
 X-Backside-Transport: OK OK
 Connection: Keep-Alive
 Transfer-Encoding: chunked
 Content-Type: application/json
 Date: Fri, 17 Feb 2017 11:11:23 GMT
 Server: nginx/1.11.5
 X-Global-Transaction-ID: 605948113
* Connection #0 to host ibm-watson-ml.stage1.mybluemix.net left intact
{"ok":"true"}
```
{: codeblock}
