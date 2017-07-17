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

# 持續保存模型


*  [模型持續性及版本控制](#model-persistence-and-version-control)

   *  [產生存取記號](#generating-the-access-token)

   *  [建立管線 meta 資料](#creating-pipeline-metadata)

   *  [建立管線版本](#creating-pipeline-version)

   *  [上傳管線內容](#uploading-pipeline-content)

   *  [建立管線模型 meta 資料](#creating-pipeline-model-metadata)

   *  [建立管線模型版本](#creating-pipeline-model-version)

   *  [上傳管線模型內容](#uploading-pipeline-model-content)

資料科學家不斷致力於在研究和開發的同時改進他們的模型。他們可能會新增特性至現有的模型，以及對模型參數進行最佳化。資料科學家以反覆運算方式開發模式時，追蹤變更可能很快變成是個問題。在這裡，我們將說明如何藉由提供模型版本化及保持整個處理程序井井有條而協助資料科學家。


開始之前，如果您首先對模型開發感興趣，請參閱下列筆記本：

*  [Developing SparkML models with Python](https://apsportal.ibm.com/exchange/public/entry/view/d80de77f784fed7915c14353512ef14d)

*  [Developing SparkML models with Scala](https://apsportal.ibm.com/exchange/public/entry/view/d80de77f784fed7915c1435351309e93)

## 模型持續性及版本控制

### 產生存取記號

使用 IBM® Watson™ Machine Learning 服務實例的「服務認證」標籤上所提供之使用者和密碼，產生存取記號。

要求範例：

```
curl --basic --user username:password https://ibm-watson-ml.mybluemix.net/v2/identity/token
```
{: codeblock}

輸出範例：

```
{"token":"**********"}
```
{: codeblock}

請使用下列終端機指令來指派記號值給 access_token 環境變數：

```
access_token="<token_value>"
```
{: codeblock}

接下來，為了儲存管線及管線模型，您將建立管線和管線模型 meta 資料、建立管線及管線模型版本，並上傳管線及管線模型版本。如需詳細資訊，請參閱下面各節。

### 建立管線 meta 資料

若要建立管線的 meta 資料，請在 curl 要求中說明管線的基本內容，如下列範例中所示：

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

範例回應：

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

### 建立管線版本

若要建立管線的版本，請在 curl 要求中指定管線的 parentVersionHref，如下列範例中所示：

```
curl -i \
-X POST \
-H 'Content-Type: application/json' \
-H "Authorization: Bearer $access_token" \
-d '{}' \
https://ibm-watson-ml.mybluemix.net/v2/artifacts/pipelines/1314f74d-a2a7-46d3-8f11-89baa1d7ffea/versions
```
{: codeblock}

範例回應：

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

### 上傳管線內容

若要上傳管線內容，您的管線必須是二進位格式。要這麼做，請從 SparkML 管線呼叫 save 方法。您可以透過在端點中提供管線的 ID 和版本來上傳二進位內容，如下列範例中所示：


```
curl -i \
-X PUT \
-H 'Content-Type: application/json' \
-H "Authorization: Bearer $access_token" \
-d <path_to_your_binary_content> \
https://ibm-watson-ml.mybluemix.net/v2/artifacts/pipelines/1314f74d-a2a7-46d3-8f11-89baa1d7ffea/versions/a535417c-ba8f-43b5-aae5-7dd8af02f518/content
```
{: codeblock}

範例回應：


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

儲存管線之後，您可以繼續儲存管線模型。

### 建立管線模型 meta 資料

您必須說明管線模型的資料綱目，如下列範例中所示：


```
curl -i \
-X POST \
-H 'Content-Type: application/json' \
-H "Authorization: Bearer $access_token" \
-d ' {
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

範例回應：


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

### 建立管線模型版本

若要為您的管線模型建立版本，請使用 curl 要求來指定詳細資料，例如儲存訓練資料的位置及模型評估方法。請參閱下列範例：


```
curl -i \
-X POST \
-H 'Content-Type: application/json' \
-H "Authorization: Bearer $access_token" \
-d ' {
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

範例回應：

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

### 上傳管線模型內容

若要上傳管線模型內容，您的管線模型必須是二進位格式。要這麼做，請從 SparkML 管線模型呼叫 save 方法。您可以透過在端點中提供管線的 ID 和版本來上傳二進位內容，如下列範例中所示：

```
curl -i \
-X PUT \
-H 'Content-Type: application/json' \
-H "Authorization: Bearer $access_token" \
-d <path_to_your_binary_content> \
https://ibm-watson-ml.mybluemix.net/ v2/artifacts/pipelines/1314f74d-a2a7-46d3-8f11-89baa1d7ffea/versions/bb328cc3-b78d-4527-9dcc-f2172f43ae9e/content
```
{: codeblock}

範例回應：

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
