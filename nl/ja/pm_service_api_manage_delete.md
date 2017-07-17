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

# デプロイされた予測モデルの削除


DELETE http://{service
instance}/pm/v1/model/{contextId}?accesskey={access_key for this
bound application}

この API 呼び出しを使用して、Machine Learning サービス・インスタンスから予測モデルを削除します。この呼び出しの実行後は、予測モデルはアプリケーション内のダウンロードにもデータのスコアリングにも使用できなくなります。


要求の例:

```
    Content-Type: */*
    Parameters:
        Path parameters:
            contextId: the identifier of deployed model you want to un-deploy and delete
        Query Parameters:
            accesskey: access_key from env.VCAP_SERVICES
```
{: codeblock}

アンデプロイが成功した場合の応答:

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

アンデプロイが失敗した場合の応答:

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
