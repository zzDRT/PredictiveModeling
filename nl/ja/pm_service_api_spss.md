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

# IBM SPSS Modeler モデル用の Machine Learning サービス API


Machine Learning サービスは、任意のプログラミング言語から呼び出すことができる一連の REST API で構成され、IBM SPSS Modeler で作成された分析をアプリケーションに統合できるようにします。Bluemix アプリケーションを Machine Learning サービス・インスタンスにバインドして、アプリケーションがより高い価値をユーザーに提供するために必要な予測分析を生成します。お使いの SPSS モデルは管理ダッシュボードで管理し、そのダッシュボードを使用して、アプリケーションの停止も再デプロイもすることなくモデルの更新や最新表示をしてください。


Machine Learning サービスのインスタンスにデプロイされる SPSS Modeler ファイルを以下のように管理します。

*  新規予測モデルのデプロイ

*  デプロイされた予測モデルの最新表示

*  デプロイされた予測モデルのコピーのダウンロード

*  デプロイされた予測モデルの削除

サービス・インスタンスにデプロイされる IBM SPSS Modeler ファイルに対して次のようにアプリケーションを開発します。


*  特定のデプロイ済み予測モデル用のメタデータの取得

*  デプロイされた予測モデルにスコアリング要求を出すことによる予測分析の生成

