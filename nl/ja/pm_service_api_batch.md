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

# IBM SPSS Modeler モデル用の Machine Learning サービスのバッチ・ジョブ API


*  [ジョブの削除](#deleting-jobs)

*  [ジョブの状況の確認](#checking-the-status-of-a-job)

*  [ジョブの再実行依頼](#resubmit-a-job)

*  [アップロードされた Modeler ストリーム・ファイルに対するジョブの実行依頼](#submit-a-job-against-an-uploaded-modeler-stream-file)

*  [ジョブで使用するストリーム・ファイルのアップロード](#upload-a-stream-file-to-use-in-your-jobs)

*  [ジョブ・タイプ](#job-types)

*  [ジョブ状況](#job-status)

*  [ジョブ API の詳細](#job-api-details)

*  [ジョブ定義 JSON](#job-definition-json)

*  [バッチ・ジョブ API の詳細](#batch-job-api-details)

Machine Learning サービスのバッチ・ジョブ API は、モデル学習、モデル評価、およびバッチ・スコアリングに関連した長時間実行タスクをサポートします。この API は、バッチ・ジョブで使用される SPSS Modeler ストリーム・ファイルと、実行依頼されるジョブ定義の 2 つの資産タイプを管理します。アップロードされる Modeler ストリーム・ファイルごとに、多数のタイプの多数のジョブが実行依頼されることがあります。ジョブで Modeler ストリーム・ファイルの内容が更新される可能性がある場合、変更されたファイルがジョブ結果の添付ファイルに含められます。モデル評価ジョブ・タイプでは、生成されたすべての評価ファイルがジョブ結果の添付ファイルに含められます。

バッチ・ジョブの導入の例については、ノートブック [From SPSS stream to batch scoring with Python](https://apsportal.ibm.com/analytics/notebooks/9d7ce38e-9417-4c76-a6b9-5bc8cf40938a/view?access_token=5ca87e3007804e5b2bbbce77c20e99ac3c164d66f2d28dfffb54aa365caaef37) を参照してください。

## ジョブの削除

ジョブを削除できます。これによって、現在実行中のジョブは取り消されます。ジョブ ID で DELETE を呼び出します (一度に複数のジョブを取り消すことができます)。

DELETE https://{PA Bluemix load balancer URL}/pm/v1/jobs/{job ID
1, job ID 2,...}?accesskey=xxxxxxxxxx

戻りは、要求内で ID によって参照されたジョブの削除された数を示します。この数値が、要求で渡されたリストと一致しない場合、個々のジョブの状況を検査する必要があります。

## ジョブの状況の確認

任意の時点でジョブ ID の状況を GET できます。

GET https://{PA Bluemix load balancer URL}/pm/v1/jobs/{job
ID}/status?accesskey=xxxxxxxxxx

返される JSON は、jobstatus と、ジョブが正常に完了した場合は、dataUrl を示します。この dataUrl を使用すると、生成されたすべてのファイル内容を取得できます。

## ジョブの再実行依頼

<job ID> に対する PUT を呼び出します。これは、実行状態であってはなりません。

PUT https://{PA Bluemix load balancer URL}/pm/v1/jobs/{job
ID}?accesskey=xxxxxxxxxx

content_type "application/json" を使用し、要求の本体に、新規または更新されたジョブ定義 JSON を含めます。

この呼び出しは実際には、参照される ID が存在しない場合は新規ジョブを作成します。この場合は 200 ではなく 201 を返すことで、どちらが発生したかが示されます。この実行で使用するジョブ定義 JSON を渡す必要があります。

## アップロードされた Modeler ストリーム・ファイルに対するジョブの実行依頼

ジョブ定義を実行キューに入れるために、以下のように、ジョブ定義に対して PUT を呼び出します。

PUT https://{PA Bluemix load balancer URL}/pm/v1/jobs/{job
ID}?accesskey=xxxxxxxxxx

content_type "application/json" を使用し、要求の本体に、ジョブ定義 JSON を含めます。

ジョブ定義が実行キューに入れられた場合、要求は即時に戻り、成功を示します。ジョブ定義で構成されたように Modeler ストリームを正常に実行できるかどうかのテストはありません。ジョブは、クラウドで Machine Learning ジョブ・サーバーの 1 つによって実行され、ユーザーはその状況をモニターできます。モデル学習を実行するときに、自動最新表示を行うことを指定した場合、ジョブは、ジョブが正常に実行されると元の Modeler ストリーム・ファイルを置き換えます。

ジョブ定義 JSON の詳細については、[ジョブ定義 JSON](#job-definition-json) を参照してください。

## ジョブで使用するストリーム・ファイルのアップロード

注: Machine Learning ダッシュボードはリアルタイム・スコアリング専用です。これは、実行中のジョブ (バッチ・スコアリング) には使用できません。

Modeler ストリーム・ファイルをジョブでアクセス可能にするために、以下のように PUT を呼び出します。

PUT https://{PA Bluemix load balancer URL}/pm/v1/file/{file
ID}?accesskey=xxxxxxxxxx

要求で content_type "multipart/form-data" を使用して、ファイルを渡します。

使用される固有の名前 (PUT 呼び出し内のファイル ID) は、ファイル API に対する DELETE 呼び出しの参照先です。またこれは、ジョブ定義でのモデル参照でもあります。ユーザーは、ジョブで使用されるファイルの名前空間を完全に制御することができます。そのため、同じ <file ID> でファイルを PUT することによって、保持されている現行コピーが暗黙的に置き換えられます。

<file ID> を省略することで、ジョブでアップロードされたすべてのファイルのリストを GET することも、ID を指定して GET を行うことで、特定のファイルを取得することもできます。また、アップロードされたファイルの DELETE を行うこともできます (この操作による当然の結果として、そのファイルを参照する保留中のジョブを実行するとエラーが発生します)。

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

## ジョブ・タイプ

学習 (Training): このジョブ・タイプは、すべての「モデル・ビルダー」端末ノードを Modeler ストリームで実行する必要があることを示します。ジョブが正常に完了した後で、新たに学習されたモデル・ナゲットで更新された Modeler ストリームが、取得可能なジョブ結果内に入れられます。Modeler ストリーム・ファイルに、モデル・ビルド・ノードから、評価枝とスコアリング枝内で学習されたモデル・ナゲットへのリンクが含まれている場合、これによってこれらのノードが最新表示されます。

評価 (Evaluation): このジョブ・タイプは、呼び出し元に戻すことができる静的なレポート・ファイルの内容を生成する、すべての「文書ビルダー」端末ノードの実行を (主に、Modeler クライアントの 「グラフ」タブと「出力」タブから) トリガーします。スコアリング枝は、このジョブ・タイプの一部とはみなされません。

自動最新表示 (Auto-Refresh): TRAINING ジョブ・タイプの 1 つのバージョン。このタイプでは、ジョブが正常に完了したときに、バッチ・ファイル・リスト内の元の Modeler ストリーム・ファイルが更新されます。リアルタイム・スコアリングのためにデプロイされた Modeler ストリームの最新表示イベントに関する評価と明示的な決定が想定され、この時点では自動最新表示の対象となりません。

バッチ・スコア (Batch Score): 「スコアリング枝として使用」オプションを適用した端末ノードの実行。これにより、端末ノードがこの Modeler ストリーム設計のスコアリング枝であることが示されます。ジョブ定義では、「エクスポート」および「ソース」の詳細を指定する必要があります。

ストリームの実行 (Run Stream): 実行は、ストリーム・プロパティーの「実行」タブで「このスクリプトを実行」オプションを選択した状態で、Modeler の緑色の「実行 (run)」ボタンをクリックすることと似ています。使用法では、モデル学習やその他のジョブ・タイプのスクリプト化された実行の必要性がカバーされています。スクリプトの動的な制御はすべて、ジョブ定義でパラメーター値を渡してストリーム・パラメーターによって処理する必要があります。

## ジョブ状況

保留中 (Pending): ジョブ定義は実行依頼されましたが、ジョブ・サーバーによって実行がまだ要求されていません。

実行中 (Running): ジョブ定義がジョブ・サーバーによって要求されており、実行中です。

キャンセル中 (Canceling): ジョブはキャンセル中です。

キャンセル済み (Canceled): ジョブはキャンセルされました。

失敗 (Failed): ジョブが失敗しました。失敗の原因に関する詳細が、ジョブ状況の GET 時に返されます。

成功 (Success): ジョブが正常に実行されました。このイベントについて通信されたすべての結果が、ジョブ状況の GET の JSON で返されます。

## ジョブ API の詳細

POST /v1/jobs/{id}

説明: ジョブ定義を実行依頼します。<job ID> が既に存在する場合はエラーになります。

コンテンツ・タイプ:

```
@Consumes({“application/json”})
@Produces({“application/json”})
```
{: codeblock}

パラメーター:


プロビジョンまたはバインドで資格情報として返されるアクセス・キー:

```
@QueryParam("accesskey")  ```
{: codeblock}

ユーザー指定のジョブ ID。Machine Learning サービス・インスタンスに固有でなければなりません。

```
@PathParam("id") ```
{: codeblock}

JSON ジョブ定義 (『ジョブ定義 JSON』を参照):

```
@BodyParam```
{: codeblock}

応答:

成功。ジョブは実行依頼され、実行依頼されたジョブを説明する JSON を返します。

```
@ApiResponse(code = 200)```
{: codeblock}

ジョブは既に存在しているというエラー。この ID を持つジョブは既に実行依頼されています。返されるメッセージではこのエラーが説明されています。

```
@ApiResponse(code = 409)```
{: codeblock}

その他のエラー。返される例外の JSON

```
@ApiResponse(code = 500)```
{: codeblock}

PUT /v1/jobs/{id}

説明: ジョブを作成または更新します。この ID を持つジョブが存在しない場合は作成します。存在する場合は更新します (事実上、再実行依頼します)。

コンテンツ・タイプ:

```
@Consumes({“application/json”})
@Produces({“application/json”})
```
{: codeblock}

パラメーター:


プロビジョンまたはバインドで資格情報として返されるアクセス・キー:

```
@QueryParam("accesskey")  ```
{: codeblock}

ユーザー指定のジョブ ID:

```
@PathParam("id") ```
{: codeblock}

JSON ジョブ定義 (『ジョブ定義 JSON』を参照):

```
@BodyParam```
{: codeblock}

応答:

成功。ジョブが更新され、実行依頼されます。実行依頼されたジョブを説明する JSON を返します。

```
@ApiResponse(code = 200)```
{: codeblock}

成功。ジョブが作成され、実行依頼されます。実行依頼されたジョブを説明する JSON を返します。

```
@ApiResponse(code = 201)```
{: codeblock}

その他のエラー。返される例外の JSON。

```
@ApiResponse(code = 500)```
{: codeblock}

GET /v1/jobs

説明: この Machine Learning サービス・インスタンスで定義されたすべてのジョブのリストを返します。

コンテンツ・タイプ:

```
@Produces({“application/json”})```
{: codeblock}

パラメーター:


プロビジョンまたはバインドで資格情報として返されるアクセス・キー:

```
@QueryParam("accesskey")  ```
{: codeblock}

応答:

成功。すべてのジョブ定義の JSON 配列を返します。

```
@ApiResponse(code = 200)```
{: codeblock}

その他のエラー。返される例外の JSON:

```
@ApiResponse(code = 500)```
{: codeblock}

GET /v1/jobs/{id}

説明: 特定のジョブ定義の戻りを要求します。

コンテンツ・タイプ:

```
@Produces({“application/json”})```
{: codeblock}

パラメーター:


プロビジョンまたはバインドで資格情報として返されるアクセス・キー:

```
@QueryParam("accesskey")  ```
{: codeblock}

実行依頼されたジョブ ID:

```
@PathParam("id") ```
{: codeblock}

応答:

成功。参照されるジョブを説明する JSON を返します。

```
@ApiResponse(code = 200)```
{: codeblock}

ジョブが見つからないというエラー。返されるメッセージの詳細:

```
@ApiResponse(code = 404)```
{: codeblock}

その他のエラー。返される例外の JSON:

```
@ApiResponse(code = 500)```
{: codeblock}

GET /v1/jobs/{id}/status

説明: 特定のジョブの状況を取得します。

コンテンツ・タイプ:

```
@Produces({“application/json”})```
{: codeblock}

パラメーター:


プロビジョンまたはバインドで資格情報として返されるアクセス・キー:

```
@QueryParam("accesskey")  ```
{: codeblock}

実行依頼されたジョブ ID:

```
@PathParam("id") ```
{: codeblock}

応答:

成功。ジョブ状況を説明する JSON を返します。

```
@ApiResponse(code = 200)```
{: codeblock}

ジョブが見つからないというエラー。返されるメッセージの詳細:

```
@ApiResponse(code = 404)```
{: codeblock}

その他のエラー。返される例外の JSON:

```
@ApiResponse(code = 500)```
{: codeblock}

DELETE /v1/jobs/{ids}

説明: 1 つ以上のジョブを削除します。現在ジョブが実行中の場合は、そのジョブを取り消します。

コンテンツ・タイプ:

```
@Produces({“application/json”})```
{: codeblock}

パラメーター:


プロビジョンまたはバインドで資格情報として返されるアクセス・キー:

```
@QueryParam("accesskey")  ```
{: codeblock}

実行依頼されたジョブ ID、または ID 値が「,」で分けられたジョブ ID リスト

```
@PathParam("id" or “id,id,id”)```
{: codeblock}

応答:

成功。削除されたジョブの数を返します。

```
@ApiResponse(code = 200)```
{: codeblock}

その他のエラー。返される例外の JSON:

```
@ApiResponse(code = 500)```
{: codeblock}

## ジョブ定義 JSON

ジョブ定義 JSON には、以下の一般セクションが含まれています。

ジョブ・タイプおよび予測モデル参照

action のタイプ:

*  TRAINING – モデル・ナゲットを学習または最新表示するモデル・ビルダー・ノードを実行します。更新された Modeler ストリームがジョブ結果で取得されます。

*  EVALUATION – 学習されたモデルを評価する文書ビルダー・ノードおよび分析ノードを実行します。

*  AUTO_REFRESH – TRAINING を実行し、バッチ・ファイルのアップロード・スペースでファイル内容を更新します。

*  BATCH_SCORE – スコアリング枝を実行し、ジョブ定義で指示されたとおりに結果のデータをエクスポートします。

*  RUN_STREAM – ストリーム・プロパティーで示されたとおりに Modeler ストリームを実行します。ほとんどの場合、スクリプト化された実行が必要な場合に使用されます。

model: バッチ・ジョブ参照のためにファイルのアップロード・アクションで指定された ID。詳しくは、『ジョブで使用するストリーム・ファイルのアップロード』を参照してください。/pm/v1/file が使用されることに注意してください。

```
"action": "TRAINING",
"model": {
     "id": "str2"
     "name":"stream1.str"
},
```
{: codeblock}

id は、PUT API で使用されるファイル ID と同じでなければならないことに注意してください。name は必須ではありませんが、モデルの学習と自動最新表示のために、ジョブ結果はここで定義された名前を使用して保存されます。name が定義されていない場合、 サービスは事前定義された命名規則に従って結果を生成します。

ジョブ設定

このジョブを実行するために必要なすべての設定。

```
"setting": {          
… Export settings
… Import settings
… Parameter value settings
… Document control settings
}
```
{: codeblock}

データベース接続定義

データベース・タイプ: DB2、DashDB、Informix、Oracle、Sybase、SQLServer、MySQL。

DB サービス・インスタンスで指定された接続。多くの DB タイプでは、特定の設定を「Options」で渡す必要があります。

```
"dbDefinitions":{
"db1":{
"type":"DashDB",
          "host":"dashdb-enterprise-yp-dal09-11.services.dal.bluemix.net",
          "port":50001,
          "db":"BLUDB",
          "username":"bluadmin",
          "password":"NmI4MDViMzBkNDUz",
          "options":"Security=SSL"
     },
     "db2": {
          "type": "SQLServer",
          "db": "qatest",
          "host": "9.20.27.37",
          "port": 1433,
          "username": "sa",
          "password": "Pass1234",
          "options": "EnableQuotedIdentifiers=1"
     }
},
```
{: codeblock}

ソース・ノード設定

Modeler ストリームの、ソース・ノード名で指定された特定の枝を提供するために使用される参照 DB 接続と表。

```
"inputs": [
     {
          "odbc": {
"dbRef”; “db2”,
               "table": "DRUG1N",
          },
          "node": "ScoreInput",
          "attributes": []
     }
],
```
{: codeblock}

table はデータベース表名です。この表は、ストリーム・ソース・ノードをオーバーライドするために使用されます。node は、ストリームのデータ・ソース名です。dbRef は、dbDefinitions オブジェクトで定義された DB 接続を参照し、table は、Modeler ストリーム内の特定の枝の入力として使用されるデータベース表です。node は、指定されたこれらのパラメーターを使用して構成される DB ソース・ノードによって置き換えられる、Modeler ストリーム内の元のソース・ノードを示しています。

エクスポート・ノード設定

Modeler ストリームの、端末ノード名で指定された特定の枝のデータを保持するために使用される参照 DB 接続と表。

データの保持中に使用される制御方法は、以下の insertMode 属性を使用して伝えられます。

*  Append – 表が存在し、挿入のための互換性がある必要があります

*  Create – 表が作成されます (表が既に存在する場合はエラーが返されます)

*  Drop – 参照される表が存在する場合は除去され、再作成されます

*  Refresh – 表の既存の行は、新しい行が挿入される前に削除されます

```
"exports": [
     {
          "odbc": {
"dbRef”; “db1”,
               "table": "DRUGSCORES",
               “insertMode”:”Append”
          },
          "node": "ExportScores",
          "attributes": [],
     }
],
```
{: codeblock}

table は、ジョブ結果の書き込み先のデータベース表名です。
node は、ストリームの端末ノード名です。ソース・ノード設定と同様に、node は、指定されたこれらのパラメーターを使用して構成される DB エクスポート・ノードによって置き換えられる、Modeler ストリーム内の元の出力ノードを示しています。

注: エクスポート・ノード設定とソース・ノード設定は、ジョブを正常に実行するために重要です。

パラメーター値のオーバーライド

ジョブの実行前にストリーム・レベル・パラメーターのデフォルト値をオーバーライドするには、ジョブ定義で使用する名前/値のペアを指定します。

```
"parameterOverride": [
     {
          "name": "p1",
          "value": "v1"
     },
     {
          "name": "p2",
          "value": "v2"
     }
],
```
{:codeblock}

評価ジョブ・タイプの実行時に返される評価文書の望ましいレポート・フォーマット

評価ジョブの実行から生成されるすべての文書が 1 つの .zip ファイルに組み込まれます。示されている reportFormat をサポートできる文書ビルダー・ノードのみが実行され、ジョブが正常に実行された後でその内容が返されます。

サポートされるフォーマットは、HTML、JPG、PNG、RTF、SAV、TAB、および XML です。

```
"reportFormat": "HTML"```
{: codeblock}

完全な JSON の例

```
{ 
     "action": "TRAINING",
     "model": {
          "id": "str2"
          "name": "stream1.str"
     },
     "dbDefinitions":{
          "db":{
                    "type":"DashDB",
                    "host":"dashdb-enterprise-yp-dal09-11.services.dal.bluemix.net",
                    "port":50001,
                    "db":"BLUDB",
                    "username":"bluadmin",
                    "password":"NmI4MDViMzBkNDUz",
                    "options":"Security=SSL"
               }
     },
     "setting": {
          "inputs": [
                    {
                         "odbc": {
"dbRef”; “db”,
                                   "table": "DRUG1N",
                         },
                         "node": "ScoreInput",
                         "attributes": []
                    }
          ],
          "parameterOverride": [
                    {
                        "name": "p1",
                        "value": "v1"
                    },
                    {
                        "name": "p2",
                        "value": "v2"
                    }
          ],
     }
}
```
{: codeblock}

## バッチ・ジョブ API の詳細

以降のセクションでは、バッチ・ジョブの SPSS Modeler ファイル管理 API の詳細を示します。

PUT /v1/file/{id}

説明: バッチ・ジョブで使用するために SPSS Modeler ストリーム・ファイルをアップロードします。

注: 指定された ID で保管されたファイルが既にある場合、そのファイルは上書きされます。

コンテンツ・タイプ:

```
@Consumes({ "multipart/form-data"  })
@Produces({“application/json”})```
{: codeblock}

パラメーター:


プロビジョンまたはバインドで資格情報として返されるアクセス・キー:

```
@QueryParam("accesskey")  ```
{: codeblock}

ユーザー指定のファイル ID。サービス・インスタンス・リポジトリーで固有でなければなりません。

```
@PathParam("id") ```
{: codeblock}

応答:

成功。サービス・インスタンスのリポジトリーからファイルをダウンロードするために使用できる URL を返します。

```
@ApiResponse(code = 200)```
{: codeblock}

その他のエラー。返される例外の JSON:

```
@ApiResponse(code = 500)```
{: codeblock}

DELETE /v1/file/{id}

説明: 指定された ID で保管されたファイルをサービス・インスタンス・リポジトリーから削除します。

コンテンツ・タイプ:

```
@Produces({“application/json”})```
{: codeblock}

パラメーター:


プロビジョンまたはバインドで資格情報として返されるアクセス・キー:

```
@QueryParam("accesskey")  ```
{: codeblock}

アップロード時にユーザーが指定したファイルの ID:

```
@PathParam("id") ```
{: codeblock}

応答:

成功。指定されたファイルは削除されています。

```
@ApiResponse(code = 200)```
{: codeblock}

エラー。この ID で保管されたファイルはサービス・インスタンス・リポジトリーに存在しませんでした。

```
@ApiResponse(code = 404)```
{: codeblock}

その他のエラー。返される例外の JSON:

```
@ApiResponse(code = 500)```
{: codeblock}

GET /v1/file/

説明: バッチ・ジョブの処理のためにサービス・インスタンス・リポジトリーで管理されるすべてのファイルの ID のリストを返します。

コンテンツ・タイプ:

```
@Produces({“application/json”})```
{: codeblock}

パラメーター:


プロビジョンまたはバインドで資格情報として返されるアクセス・キー:

```
@QueryParam("accesskey")  ```
{: codeblock}

応答:

成功。ファイル ID 値の JSON 配列を返します。

```
@ApiResponse(code = 200)```
{: codeblock}

その他のエラー。返される例外の JSON:

```
@ApiResponse(code = 500)```
{: codeblock}

GET /v1/file/{id}

説明: バッチ・ジョブの処理で使用するために、指定された ID で保管された SPSS Modeler ストリーム・ファイルを取得します。

コンテンツ・タイプ:

```
@Produces({ "application/octet-stream" })```
{: codeblock}

パラメーター:


プロビジョンまたはバインドで資格情報として返されるアクセス・キー:

```
@QueryParam("accesskey")  ```
{: codeblock}

アップロード時にユーザーが指定したファイルの ID:

```
@PathParam("id") ```
{: codeblock}

応答:

成功。SPSS Modeler ファイルを返します。

```
@ApiResponse(code = 200)```
{: codeblock}

エラー。この ID で保管されたファイルはサービス・インスタンス・リポジトリーに存在しませんでした。

```
@ApiResponse(code = 404)```
{: codeblock}

その他のエラー。返される例外の JSON:

```
@ApiResponse(code = 500)```
{: codeblock}
