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

# IBM SPSS Modeler モデルを用いた Machine Learning サービスの使用

SPSS Modeler の「モデル作成」パレットにあるモデリング手法を使用して、データから新しい情報を引き出したり、予測モデルを作成したりすることができます。各手法によって、強みや適した問題の種類が異なります。
{: .shortdesc}

SPSS Modeler の概要と提供されるモデリング・アルゴリズムについて詳しくは、[IBM Knowledge Center](https://www.ibm.com/support/knowledgecenter/SS3RA7) を参照してください。

お使いの Bluemix アプリケーションや SPSS Modeler スコアリング枝設計の入出力要件が実装されたら、データ・アナリストはスコアリング枝の内部を変更できます。
データ・アナリストは、ユーザーがアプリケーションを書き換えることなく予測分析を微調整できる機能を確保しながら、最新表示操作で使用されるモデル・アルゴリズムまでも変更することができます。


SPSS Modeler で作成されたモデルと共に使用する場合、Machine Learning サービスに関する以下の重要な情報に注意してください。

*  スコアリング枝をリアルタイム・スコアリングで使用するために準備する場合、スコア要求で取得される入力データによって、スコアリング枝に入るように設計されたソース・ノードが置き換えられる必要があります。また、その結果の予測分析出力は、応答フローに戻る必要があります (事実上、スコアリング枝設計の端末ノードが置き換えられます)。

*  Bluemix でのリアルタイム実行のためにスコアリング枝を準備するときには、外部サービスへの接続を要求できません。例えば、IBM Analytical Decision Management スコアリング枝設計には、IBM SPSS Collaboration and Deployment Services リポジトリーに保管されたルールやモデルへの参照を含めることはできません。

*  スコアリング枝を Bluemix でリアルタイム・スコアリングのために実行する場合、外部サービスを要求できません。例えば、リアルタイムで IBM SPSS
Analytic Server および Apache Hadoop データ・ストアを必要とするモデル・アルゴリズムのデプロイとスコアリングを行うことはできません。

*  Machine Learning では、Modeler の組み込みの Python スクリプトがサポートされます。ストリームを Machine Learning での実行前に処理するために使用する方法が原因で、いくつかの制限があります。通常、ユーザーがストリームの実行を制御することを選択した場合、枝の端末ノードが参照されます。Machine Learning では、ストリームの処理時に、オーバーライドされる JSON のノードを識別してから、ストリームの実行前に置換を行います。これによって、参照される入力ノードとエクスポート・ノードが存在しなくなるためにストリームはスクリプトで失敗します。解決方法は、別のノードの ID を使用して、実行中に枝を一意的に識別することです。これで、組み込みの Python スクリプトで定義されたようにストリームが実行されるようになります。

IBM SPSS Analytic Server の学習された予測モデルの現行サポートに関する詳細については、IBM Knowledge Center の『Analytic Server』セクションを参照してください。

1. Node.js サンプル・コードをダウンロードして、Machine Learning サービスを試行することができます。以下のステップを実行して、Bluemix アプリケーションを作成し、Machine Learning サービスをバインドします。これらの例では、一般的なランタイムである Node.js を使用しています。
iOS、Ruby、Perl、あるいは Java など他のものも使用できます。

2. cf create-service コマンドを使用して、サービス・インスタンスを作成します。

   ```
   cf create-service pm-20 Free {local naming}
   ```

   例:

   ```
   cf create-service pm-20 Free my_pm_free
   ```

   このコマンドは、Bluemix スペースに my_pm_free という名前で、Free プランを使用する 1 つの Machine Learning サービス・インスタンスを作成します。

3. `cf create-service-key` コマンドを使用して、サービス資格情報を作成します。

   ```cf create-service-key "{service instance name}" {vcap key name}```

   例:

   ```cf create-service-key "IBM Watson Machine Learning - my instance" Credentials-1```

   このコマンドにより、Machine Learning サービス資格情報が作成されます。

4. cf bind-service コマンドを使用して、サービス・インスタンス my_pm_free をお使いのアプリケーションにバインドします。

   ```cf bind-service AppName my_pm_service```

   例:

   ```cf bind-service my_app1 my_pm_free```

   このコマンドは、Machine Learning サービス・インスタンス
   `my_pm_free` を Bluemix アプリケーション my_app1 にバインドします。

5. Machine Learning の資格情報:

   Machine Learning サービス・インスタンスを Bluemix アプリケーションにバインドすると、Machine Learning の資格情報が `VCAP_SERVICES` 環境変数に追加されます。

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

   `VCAP_SERVICES` 環境変数には以下の情報が含まれます。


   <dl>
   
   <dt>plan</dt>
   <dd>サービス・プロビジョニングで使用される Machine Learning プラン。</dd>

   <dt>url</dt>
   <dd>Machine Learning サービス・インスタンスのアドレス。</dd>

   <dt>access_key</dt>
   <dd>このサービス・インスタンスへのすべての要求に渡される照会パラメーター accessKey。</dd>

   </dl>
            
例:             

```
Get https://ibm-watson-ml.mybluemix.net/pm/v1/model/sales_model2?accesskey=XXXXXXXXXXXXX
```

   以下は、`VCAP_SERVICES` 環境変数から accessKey を取得する方法を示す Node.js サンプル・コードです。

```
   if (process.env.VCAP_SERVICES) {
        var env = JSON.parse(process.env.VCAP_SERVICES);
        var credentials = env['pm-20'][0].credentials;
        var accessKey = credentials.access_key;
    }
```
