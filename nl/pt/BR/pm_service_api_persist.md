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

# Modelos persistentes


*  [Persistência
de modelo
e controle de versão](#model-persistence-and-version-control)

   *  [Gerando
o token de acesso](#generating-the-access-token)

   *  [Criando
metadados de pipeline](#creating-pipeline-metadata)

   *  [Criando
versão de pipeline](#creating-pipeline-version)

   *  [Fazendo
upload de conteúdo de pipeline](#uploading-pipeline-content)

   *  [Criando
metadados de
modelo de pipeline](#creating-pipeline-model-metadata)

   *  [Criando versão
de modelo de pipeline](#creating-pipeline-model-version)

   *  [Fazendo
upload de conteúdo de modelo de
pipeline](#uploading-pipeline-model-content)

Os cientistas de dados se esforçam continuamente para
melhorar seus modelos como parte de sua pesquisa e desenvolvimento. Eles podem incluir novos recursos em modelos existentes e
otimizar os parâmetros de modelo.
Quando os cientistas de dados
desenvolvem modelos em um modo iterativo, mantendo o controle de
mudanças, isso pode rapidamente se tornar um problema. Aqui
vamos ilustrar maneiras de ajudar os cientistas de dados,
fornecendo a versão do modelo e mantendo o processo inteiro bem
organizado.

Antes de iniciar, se você estiver interessado primeiramente em
desenvolvimento de modelo, consulte as seguintes notas:

*  [Desenvolvendo
modelos SparkML com Python](https://apsportal.ibm.com/exchange/public/entry/view/d80de77f784fed7915c14353512ef14d)

*  [Desenvolvendo
modelos SparkML com Scala](https://apsportal.ibm.com/exchange/public/entry/view/d80de77f784fed7915c1435351309e93)

## Persistência de modelo e controle de versão

### Gerando o token de acesso

Gere um token de acesso usando o usuário e a senha disponíveis na
guia Credenciais de serviço da instância de serviço do IBM® Watson™ Machine
Learning.

Exemplo de solicitação:

```
curl --basic --user username:password https://ibm-watson-ml.mybluemix.net/v2/identity/token
```
{: codeblock}

Exemplo de saída:

```
{"token":"**********"}
```
{: codeblock}

Use o comando de terminal a seguir para designar o valor do token à
variável de ambiente access_token:

```
access_token="<token_value>"
```
{: codeblock}

Em seguida, para salvar o pipeline e o modelo de pipeline,
você criará os metadados do modelo de pipeline e o pipeline,
criará a versão do modelo de pipeline e o pipeline e fará
upload da versão do modelo de pipeline e do pipeline. Consulte as seções a seguir para detalhes.

### Criando metadados de pipeline

Para criar metadados para seu pipeline, descreva as propriedades
básicas de seu pipeline em uma solicitação curl, conforme mostrado no
exemplo a seguir:

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

Resposta de exemplo:

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

### Criando a versão de pipeline

Para criar uma versão para seu pipeline, especifique o
parentVersionHref do pipeline em uma solicitação curl, conforme mostrado no
exemplo a seguir:

```
curl -i \
-X POST \
-H 'Content-Type: application/json' \
-H "Authorization: Bearer $access_token" \
-d '{}' \
https://ibm-watson-ml.mybluemix.net/v2/artifacts/pipelines/1314f74d-a2a7-46d3-8f11-89baa1d7ffea/versions
```
{: codeblock}

Resposta de exemplo:

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

### Fazendo upload de conteúdo de pipeline

Para fazer upload de conteúdo de pipeline, seu pipeline deve
estar no formato binário. Para fazer isso, chame o método de salvamento do pipeline
SparkML. É possível fazer
upload do seu conteúdo binário, fornecendo o ID do seu pipeline
e a versão em um terminal, conforme mostrado no
exemplo a seguir:

```
curl -i \
-X PUT \
-H 'Content-Type: application/json' \
-H "Authorization: Bearer $access_token" \
-d <path_to_your_binary_content> \
https://ibm-watson-ml.mybluemix.net/v2/artifacts/pipelines/1314f74d-a2a7-46d3-8f11-89baa1d7ffea/versions/a535417c-ba8f-43b5-aae5-7dd8af02f518/content
```
{: codeblock}

Resposta de exemplo:

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

Depois de salvar o pipeline, é possível continuar salvando
seu modelo de pipeline.

### Criando metadados de modelo de pipeline

Deve-se descrever o esquema de dados do seu modelo de pipeline,
conforme mostrado no exemplo a seguir:

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

Resposta de exemplo:

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

### Criando versão do modelo de pipeline

Para criar uma versão para o modelo de pipeline, use uma solicitação curl
para especificar detalhes, como o local que você armazena seus dados de
treinamento e o método de avaliação de modelo. Consulte o seguinte exemplo:

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

Resposta de exemplo:

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

### Fazendo upload do conteúdo do modelo de pipeline

Para fazer upload de conteúdo do modelo de pipeline, seu
modelo de pipeline deve estar no formato binário. Para fazer isso, chame o método de salvamento do
modelo de pipeline SparkML. É possível fazer
upload do seu conteúdo binário, fornecendo o ID do seu pipeline
e a versão em um terminal, conforme mostrado no
exemplo a seguir:

```
curl -i \
-X PUT \
-H 'Content-Type: application/json' \
-H "Authorization: Bearer $access_token" \
-d <path_to_your_binary_content> \
https://ibm-watson-ml.mybluemix.net/ v2/artifacts/pipelines/1314f74d-a2a7-46d3-8f11-89baa1d7ffea/versions/bb328cc3-b78d-4527-9dcc-f2172f43ae9e/content
```
{: codeblock}

Resposta de exemplo:

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
