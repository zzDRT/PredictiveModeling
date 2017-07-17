---

copyright:
  years: 2016, 2017
lastupdated: "2017-06-21"

---
<!-- Copyright info and last updated date at top of file: REQUIRED
    The copyright and lastupdated info is YAML content that must occur at the top of the MD file, before attributes are listed.
    It must be --- surrounded by 3 dashes ---
    The value "years" can contain just one year or a two years separated by a comma. (years: 2014, 2016)
    The value "lastupdated" must be followed by a machine date in quotes in the following format: "YYYY-MM-DD"
    The value for "years" must be indented 2 spaces under "copyright", followed by "lastupdated" which should start on its own non-indented line.

-->

<!-- Common attributes used in the template are defined as follows: -->
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# カスタム・モデルの開発および永続性

## カスタム・モデルの処理

### シナリオ名: 製品ライン予測。

### シナリオの説明:

クライアントは、ヨーロッパで最も有名なチェーン・ストアの 1 つを経営しています。そのクライアントは、個人用装飾品、キャンプ用品、およびアウトドア用保護用品などの製品ラインの観点で顧客の興味を把握してほしいと考えています。データ・サイエンティストが、予測モデルを開発し、それを開発者と共有します。開発者のタスクは、モデルをデプロイし、デプロイ済みモデルに対してスコア要求を行うことにより、予測分析を生成することです。

### カスタム・モデルの開発および永続性

1. Data Science Experience を使用して、カスタム・モデルを作成します。登録してから、サインインして以下の手順を実行してください。

2. 初回のサインイン時に、組織およびスペースを作成するように求められます。
**「続行」**をクリックして、デフォルト値を受け入れます。

3. 組織の作成後に、**「マイ・プロジェクト (My Projects)」**に移動し、**「プロジェクトの作成 (create project)」**をクリックします。

4. プロジェクトの名前および説明を入力し、**「作成」**をクリックします。指定したプロジェクト名は、ターゲット・コンテナーの名前としても使用されます。

5. プロジェクトの作成後に、ノートブックを追加して独自のモデルの開発を開始するか、または以下のサンプル・ノートブックのいずれかをアップロードすることができます。

   *  [Python を使用した Spark MLlib モデルの開発](https://apsportal.ibm.com/analytics/notebooks/89492fd6-a641-4819-9176-3d9381561df9/view?access_token=d80bef1a172d1d83d3721b101886337158457281774186f181a2e6a5b57f5ec7)

   *  [Scala を使用した Spark MLlib モデルの開発](https://apsportal.ibm.com/analytics/notebooks/c8652d2c-bfc9-4354-8168-f1c9f7f8dfc2/view?access_token=02a83fea8450a452c8de76af98dae078459d0f56810ddef4f4c62d5bc4fc72cf)

   *  [Python を使用した scikit-learn モデルの開発](https://apsportal.ibm.com/analytics/notebooks/5215a61a-16d7-4fa2-b060-e3e243ceebe3/view?access_token=70f48c95c5571a614ce97484d3f168b1d9b6aeebce015187d3d77ce6038f025e)



### カスタム・モデルのデプロイメントおよびスコアリング

以下のセクションでモデルのデプロイおよびスコアリングに関する説明を参照するか、上記でリンクが示されているノートブックのスコアリングのセクションを参照してください。

*  [オンライン・モデルのデプロイ](pm_service_api_spark_online.html)

*  [バッチ・モデルのデプロイ](pm_service_api_spark_batch.html)

*  [ストリーミング・モデルのデプロイ](pm_service_api_spark_streaming.html)
