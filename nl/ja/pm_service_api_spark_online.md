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

# オンライン・モデルのデプロイ


**シナリオ名**: 製品ライン予測。

**シナリオの説明**: アウトドア用の備品を販売する企業が、自社の製品ラインに対する顧客の関心を予測するモデルを作成したいと考えています。データ・サイエンティストが、予測モデルを準備し、それを開発者と共有します。開発者のタスクは、モデルをデプロイし、デプロイ済みモデルに対してスコア要求を行うことにより、顧客の興味の予測を生成することです。

## サンプル・モデルの使用

1. IBM® Watson™ Machine Learning ダッシュボードの「サンプル」タブに移動します。

2. 「サンプル・モデル (Sample Models)」セクションで、「Product Line Prediction」タイルを見つけて、「モデルの追加 (Add model)」ボタン (+) をクリックします。

これで、「モデル (Models)」タブの使用可能なモデルのリストに、サンプルの「Product Line Prediction」モデルが表示されます。

## アクセス・トークンの生成

IBM Watson Machine Learning サービス・インスタンスの「サービス資格情報」タブで提供されているユーザーおよびパスワードを使用して、アクセス・トークンを生成します。

要求の例:

```
curl --basic --user username:password https://ibm-watson-ml.mybluemix.net/v3/identity/token
```
{: codeblock}

出力例:

```
{"token":"**********"}```
{: codeblock}

## オンライン・デプロイメントの作成

以下の API 呼び出しを使用して、予測モデルのオンライン・デプロイメントを作成します。これは、お使いのアプリケーションでデータをスコアリングするために使用可能です。
オンライン・デプロイメントの作成に必要なエンドポイントは、「WML ダッシュボード」->「モデルの詳細 (Model Details)」->「URL」で入手できます。また、「published_model_id」値も IBM Watson Machine Learning ダッシュボードの「モデルの詳細 (Model Details)」ページの「ID」フィールドで見つけることができます。 これを使用して Watson Machine Learning インスタンスの VCAP 資格情報から「instance_id」を取得できます。

要求の例:

```
curl -X POST --header "Content-Type: application/json" --header "Accept: application/json" --header "Authorization: token" -d '{"name": "Product Line Prediction", "description": "Product Line Prediction Deployment", "type": "online"}' 'https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/{published_model_id}/deployments'
```
{: codeblock}

出力例:

```
{  
   "metadata":{
      "guid":"b97072ec-3ef8-4705-a1e7-c264e270e49a",
      "url":"https://ibm-watson-ml.mybluemix.net/v3/wml_instances/21208fa4-f5b8-4fb7-b162-878e455e4be2/published_models/1ab15cdb-4f9e-4d35-8077-c0f6fff10196/deployments/b97072ec-3ef8-4705-a1e7-c264e270e49a",
      "created_at":"2017-06-27T13:47:49.534Z",
      "modified_at":"2017-06-27T13:47:58.347Z"
   },
   "entity":{
      "runtime_environment":"spark-2.0",
      "name":"Product Line Prediction TMNL",
      "instance_href":"/v2/scoring/online/jobs/b97072ec-3ef8-4705-a1e7-c264e270e49a",
      "scoring_url":"https://ibm-watson-ml.mybluemix.net/v3/wml_instances/21208fa4-f5b8-4fb7-b162-878e455e4be2/published_models/1ab15cdb-4f9e-4d35-8077-c0f6fff10196/deployments/b97072ec-3ef8-4705-a1e7-c264e270e49a/online",
      "description":"Product Line Prediction Deployment",
      "published_model":{
         "author":{
            "name":"IBM",
            "email":""
         },
         "name":"Product Line Prediction",
         "url":"https://ibm-watson-ml.mybluemix.net/v3/wml_instances/21208fa4-f5b8-4fb7-b162-878e455e4be2/published_models/1ab15cdb-4f9e-4d35-8077-c0f6fff10196",
         "guid":"1ab15cdb-4f9e-4d35-8077-c0f6fff10196",
         "description":"Predicts clients' interests in terms of sport product lines for chain stores in Europe.",
         "created_at":"2017-06-27T11:54:24.170Z"
      },
      "model_type":"sparkml-model-2.0",
      "status":"INITIALIZING",
      "type":"online",
      "deployed_version":{
         "url":"https://ibm-watson-ml.mybluemix.net/v2/artifacts/models/1ab15cdb-4f9e-4d35-8077-c0f6fff10196/versions/3ab34346-f195-4ee4-b378-1204f674a725",
         "guid":"3ab34346-f195-4ee4-b378-1204f674a725",
         "created_at":"2017-06-27T11:54:07.507Z"
      }
   }
}
```
{: codeblock}

## デプロイメントの詳細の取得

デプロイメント・モデルに関連した状況、スコアリング・エンドポイント・アドレス (scoringHref)、およびパラメーターを確認できます。

要求の例:

```
curl -X GET --header "Content-Type: application/json" --header "Accept: application/json" --header "Authorization: $token" 'https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/{published_model_id}/deployments/{deployment_id}'
```
{: codeblock}

出力例:

```
{  
   "metadata":{
      "guid":"b97072ec-3ef8-4705-a1e7-c264e270e49a",
      "url":"https://ibm-watson-ml.mybluemix.net/v3/wml_instances/21208fa4-f5b8-4fb7-b162-878e455e4be2/published_models/1ab15cdb-4f9e-4d35-8077-c0f6fff10196/deployments/b97072ec-3ef8-4705-a1e7-c264e270e49a",
      "created_at":"2017-06-27T13:47:49.534Z",
      "modified_at":"2017-06-27T13:47:58.347Z"
   },
   "entity":{
      "runtime_environment":"spark-2.0",
      "name":"Product Line Prediction TMNL",
      "instance_href":"/v2/scoring/online/jobs/b97072ec-3ef8-4705-a1e7-c264e270e49a",
      "scoring_url":"https://ibm-watson-ml.mybluemix.net/v3/wml_instances/21208fa4-f5b8-4fb7-b162-878e455e4be2/published_models/1ab15cdb-4f9e-4d35-8077-c0f6fff10196/deployments/b97072ec-3ef8-4705-a1e7-c264e270e49a/online",
      "description":"Product Line Prediction Deployment",
      "published_model":{
         "author":{
            "name":"IBM",
            "email":""
         },
         "name":"Product Line Prediction",
         "url":"https://ibm-watson-ml.mybluemix.net/v3/wml_instances/21208fa4-f5b8-4fb7-b162-878e455e4be2/published_models/1ab15cdb-4f9e-4d35-8077-c0f6fff10196",
         "guid":"1ab15cdb-4f9e-4d35-8077-c0f6fff10196",
         "description":"Predicts clients' interests in terms of sport product lines for chain stores in Europe.",
         "created_at":"2017-06-27T11:54:24.170Z"
      },
      "model_type":"sparkml-model-2.0",
      "status":"ACTIVE",
      "type":"online",
      "deployed_version":{
         "url":"https://ibm-watson-ml.mybluemix.net/v2/artifacts/models/1ab15cdb-4f9e-4d35-8077-c0f6fff10196/versions/3ab34346-f195-4ee4-b378-1204f674a725",
         "guid":"3ab34346-f195-4ee4-b378-1204f674a725",
         "created_at":"2017-06-27T11:54:07.507Z"
      }
   }
}
```
{: codeblock}

## スコア要求の実行

スコアリング・エンドポイント (scoringHref) が作成されたため、スコア要求を行うことによって、予測を生成できるようになりました。このシナリオでは、顧客レコードが予測モデルに渡され、スポーツ製品予測が返されます。

サンプル・レコード・ヘッダー:

```
GENDER,AGE,MARITAL_STATUS,PROFESSION```
{: codeblock}

サンプル・レコード:

```
M,27,Single,Professional
F,56,Unspecified,Hospitality
M,45,Married,Retired
```
{: codeblock}

要求の例:

```
curl -X POST --header 'Content-Type: application/json' --header 'Accept: application/json' --header 'Authorization: token' -d '{"fields": ["GENDER","AGE","MARITAL_STATUS","PROFESSION"],"values": [["M",23,"Single","Student"],["M",55,"Single","Executive"]]}' 'https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}//published_models/{published_model_id}/deployments/{deployment_id}/online'
```
{: codeblock}

出力例:

```
{
   "fields":[
      "GENDER",
      "AGE",
      "MARITAL_STATUS",
      "PROFESSION",
      "PROFESSION_IX",
      "GENDER_IX",
      "MARITAL_STATUS_IX",
      "features",
      "rawPrediction",
      "probability",
      "prediction",
      "predictedLabel"
   ],
   "records":[
      [
         "M",
         23,
         "Single",
         "Student",
         6.0,
         0.0,
         1.0,
         [
            0.0,
            23.0,
            1.0,
            6.0
         ],
         [
            5.600716147152702,
            6.482458372136143,
            6.048004170730676,
            0.20929155492307386,
            1.6595297550574055
         ],
         [
            0.2800358073576351,
            0.32412291860680714,
            0.3024002085365338,
            0.010464577746153694,
            0.08297648775287028
         ],
         1.0,
         "Personal Accessories"
      ],
      [
         "M",
         55,
         "Single",
         "Executive",
         3.0,
         0.0,
         1.0,
         [
            0.0,
            55.0,
            1.0,
            3.0
         ],
         [
            6.227653744886563,
            4.325198862164969,
            8.031953760146816,
            1.2319759269281225,
            0.1832177058735289
         ],
         [
            0.3113826872443282,
            0.2162599431082485,
            0.40159768800734086,
            0.06159879634640614,
            0.009160885293676447
         ],
         2.0,
         "Mountaineering Equipment"
      ]
   ]
}
```
{: codeblock}

例えば、55 歳の会社役員は「登山用備品 (Mountaineering Equipment)」に関心があり、一方、23 歳の学生は「個人用アクセサリー (Personal Accessories)」に関心があることが分かります。
