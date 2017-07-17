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

# 予測モデルのデプロイまたは最新表示


PUT http://{PA Bluemix load balancer
URL}/pm/v1/model/{contextId}?accesskey={access_key for this bound
application}

この API 呼び出しを使用して、デプロイ対象の IBM SPSS Modeler の開発済みスコアリング枝が含まれるファイルをアップロードします。
これは、お使いのアプリケーションでデータをスコアリングするために使用可能です。
各モデル・ファイルには、以降のサービス呼び出しでデプロイ済みモデルを参照するために使用する便利な別名として context ID が与えられます。特定の context ID にモデルが存在する場合、既存のモデルは、アプリケーションで使用中の予測分析を最新表示する手段として、この PUT 呼び出しによって置き換えられます。

要求の例:

```
    Content-Type: multipart/form-data
    Parameters:
        Form parameters:
            model_file: the model file to upload
        Path parameters:
            contextId: the unique identifier to assign to your model or a reference to the deployed model to refresh
        Query Parameters:
            accesskey: access_key from env.VCAP_SERVICES
```
{: codeblock}

デプロイメントが成功した場合の応答:

```
    Content-Type: application/json
    Status code: 200
    body:
        {
           "flag":true,
           "message":"detailed information"
         }
```
{: codeblock}

デプロイメントが失敗した場合の応答:

```
    Content-Type: application/json
    Status code: 200
    body:
        {
           "flag":false,
           "message":"reason"
        }
```
{: codeblock}
