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

# Spark モデルおよび Python モデル用の Machine Learning サービス API


Machine Learning サービスは、任意のプログラミング言語から呼び出すことができる一連の [REST API](https://watson-ml-api.mybluemix.net/) で構成され、Data Science Experience で作成された分析をアプリケーションに統合できるようにします。Bluemix アプリケーションを Machine Learning サービス・インスタンスにバインドして、アプリケーションがより高い価値をユーザーに提供するために必要な予測分析を生成します。管理ダッシュボードでモデルを管理し、そのダッシュボードを使用して、アプリケーションと統合されたオンライン・デプロイメント、バッチ・デプロイメント、またはストリーミング・デプロイメントを作成します。

Watson Machine Learning ダッシュボードを使用して、Machine Learning サービスのインスタンスで使用可能な Data Science Experience ファイル (Spark モデルおよび Python モデル) を管理します。

*  サンプル・モデルのインポート

*  モデルの詳細の表示

*  以下のいずれかとしてモデルをデプロイ

   *  オンライン・デプロイメント (スコアリング)

   *  バッチ・デプロイメント (IBM オブジェクト・ストレージおよび DashDB をサポート)

   *  ストリーミング・デプロイメント (IBM MessageHub をサポート)

*  デプロイメントの詳細の取得

*  デプロイメントの削除


強力な [REST API](https://watson-ml-api.mybluemix.net/) を使用して、サービス・インスタンスにデプロイされた Data Science Experience ファイル (Spark モデルおよび Python モデル) に対するアプリケーションを開発します。

*  特定の予測モデル用のメタデータの取得

*  モデルのデプロイおよびデプロイされたモデルの管理

*  特定のデプロイメント用のメタデータの取得

*  デプロイされたモデルにスコア要求を出すことによる予測分析の生成

使用可能な [Swagger で表現された REST API](https://watson-ml-api.mybluemix.net/) を介して、Machine Learning サービスの機能を容易に探索できます。

次のような REST API の例については、以降のセクションを参照してください。

*  [オンライン・デプロイメントおよびスコアリング](pm_service_api_spark_online.html)

*  [オブジェクト・ストレージを使用するバッチ・デプロイメント](pm_service_api_spark_batch.html)

*  [MessageHub を使用するストリーミング・デプロイメント](pm_service_api_spark_streaming.html)
