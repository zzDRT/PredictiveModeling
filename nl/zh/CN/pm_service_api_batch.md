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

# IBM SPSS Modeler 模型的 Machine Learning 服务批处理作业 API


*  [删除作业](#deleting-jobs)

*  [检查作业的状态](#checking-the-status-of-a-job)

*  [重新提交作业](#resubmit-a-job)

*  [根据已上传的 Modeler 流文件提交作业](#submit-a-job-against-an-uploaded-modeler-stream-file)

*  [上传要在作业中使用的流文件](#upload-a-stream-file-to-use-in-your-jobs)

*  [作业类型](#job-types)

*  [作业状态](#job-status)

*  [作业 API 详细信息](#job-api-details)

*  [作业定义 JSON](#job-definition-json)

*  [批处理作业 API 详细信息](#batch-job-api-details)

Machine Learning 服务的批处理作业 API 支持与模型培训、模型评估和批量评分相关的长期运行的任务。
此 API 可管理两种资产类型：用于批处理作业的 SPSS Modeler 流文件和提交的作业定义。
对于上传的每一个 Modeler 流文件，可能提交了许多类型的许多作业。
如果作业可能会更新 Modeler 流文件内容，那么修改的文件可能会包含在作业结果附件中。
在模型评估作业类型中，生成的所有评估文件将会处于作业结果附件中。


有关采用的批处理作业示例，请参阅以下配置页：[使用 Python 从 SPSS 流到批量评分](https://apsportal.ibm.com/analytics/notebooks/9d7ce38e-9417-4c76-a6b9-5bc8cf40938a/view?access_token=5ca87e3007804e5b2bbbce77c20e99ac3c164d66f2d28dfffb54aa365caaef37)。

## 删除作业

您可以删除作业，如果作业当前正在运行，则会取消作业。在作业标识上调用 DELETE（您可以同时取消多个作业）：

DELETE https://{PA Bluemix load balancer URL}/pm/v1/jobs/{job ID
1, job ID 2,...}?accesskey=xxxxxxxxxx

返回的结果将指示已删除请求中该标识所参考的作业数。如果此数字与请求中传递的列表不匹配，那么您必须检验个别作业的状态。


## 检查作业的状态

您可以在任何时间点 GET 作业标识的状态：

GET https://{PA Bluemix load balancer URL}/pm/v1/jobs/{job
ID}/status?accesskey=xxxxxxxxxx

返回的 JSON 指示 jobstatus，如果作业成功完成，则指示您可用于获取所有已生成文件内容的 dataUrl。

## 重新提交作业

针对 <job ID> 调用 PUT。该作业不得处于正在运行状态：


PUT https://{PA Bluemix load balancer URL}/pm/v1/jobs/{job
ID}?accesskey=xxxxxxxxxx

同时通过 content_type "application/json"，在请求的主体中，包括新的或更新的作业定义 JSON。

如果所参考的标识不存在，此调用实际上会创建新的作业，同时返回 201 或 200，以指出发生的是哪一种情况。
您必须传递要在此执行中使用的作业定义 JSON。


## 根据已上传的 Modeler 流文件提交作业

针对您的作业定义调用 PUT，以将其置于执行队列中：

PUT https://{PA Bluemix load balancer URL}/pm/v1/jobs/{job
ID}?accesskey=xxxxxxxxxx

同时通过 content_type "application/json"，在请求的主体中，包括作业定义 JSON。

如果作业定义置于执行队列中，那么此请求会立即返回，指示成功。
不会对成功执行 Modeler 流的能力进行测试，如作业定义中所配置。
该作业将由云中的其中一个 Machine Learning 作业服务器执行，您可以监视其状态。
如果执行模型培训并指示您要自动刷新，那么该作业将会在成功执行时替换原始 Modeler 流文件。


有关作业定义 JSON 的更多信息，请参阅[作业定义 JSON](#job-definition-json)。

## 上传要在作业中使用的流文件

注：Machine Learning 仪表板仅用于实时评分。
您无法将其用于运行作业（批量评分）。

调用 PUT 以使 Modeler 流文件可供作业访问：

PUT https://{PA Bluemix load balancer URL}/pm/v1/file/{file
ID}?accesskey=xxxxxxxxxx

同时通过 content_type "multipart/form-data"，在请求中传递文件。


所使用的唯一名称（PUT 调用中的文件标识）是您要在文件 API DELETE 调用中参考的名称，以及作业定义中的模型参考。
请注意，在作业中使用的文件名称空间完全受您控制，因此相同 <file ID> 下文件的 PUT 会隐式地替换当前保留的副本。


您可以通过省略 <file ID> 来 GET 为您作业上传的所有文件列表，或者使用其标识，通过 GET 检索特定文件。您还可以 DELETE 已上传的文件（当然，这会在参考该文件的任何暂挂作业执行中导致错误）。


请求示例：

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

部署成功时的响应：

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

部署失败时的响应：

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

## 作业类型

培训：此作业类型指出所有“模型构建器”终端节点都应该在 Modeler 流中执行。
作业成功完成之后，具有新培训模型块的更新 Modeler 流将处于可检索的作业结果中。
如果 Modeler 流文件在评估和评分分支中具有从模型构建节点到受训模型块的链接，那么将会导致刷新那些节点。


评估：此作业类型会触发执行所有“文档构建器”终端节点（主要来自 Modeler 客户端的“图形”和“输出”选项卡），以生成可传递回调用者的静态报告文件内容。
评分分支不会视为此作业类型的一部分。

自动刷新：作业成功完成时，将更新批处理文件列表中原始 Modeler 流文件的 TRAINING 作业类型的版本。
假设使用针对实时评分部署的 Modeler 流刷新事件相关的评估和明确决策，但此时并未包含在自动刷新中。


批量评分：执行已应用“用作评分分支”选项的终端节点，指出这是此 Modeler 流设计中的评分分支。
作业定义必须指定“导出”以及“评分”详细信息。

运行流：执行类似于在已选中流属性“执行”选项卡上“运行此脚本”选项的情况下，单击 Modeler 中的绿色“运行”按钮。使用情况涵盖需要模型培训或其他作业类型的脚本执行。
脚本的所有动态控制必须由流参数（在作业定义中传递参数值）处理。


## 作业状态

暂挂：已提交作业定义，但是作业服务器尚未声明执行。


正在运行：作业服务器已声明作业定义并在执行中。


正在取消：正在取消作业。

已取消：已取消作业。

失败：作业失败。失败原因的详细信息在对作业状态执行 GET 时返回。

成功：作业成功运行。针对此事件连接的任何结果会在作业状态的 GET 的 JSON 中返回。

## 作业 API 详细信息

POST /v1/jobs/{id}

描述：提交作业定义以便执行。如果 <job ID> 已存在，那么将会发生错误。

内容类型：

```
@Consumes({“application/json”})
@Produces({“application/json”})
```
{: codeblock}

参数：

作为供应或绑定中凭证返回的访问键：


```
@QueryParam("accesskey")
```
{: codeblock}

用户指定的作业标识。对于 Machine Learning 服务实例来说必须唯一：


```
@PathParam("id")
```
{: codeblock}

JSON 作业定义（请参阅作业定义 JSON）：

```
@BodyParam
```
{: codeblock}

响应：

成功。已提交作业以便执行，并返回描述所提交作业的 JSON。


```
@ApiResponse(code = 200)
```
{: codeblock}

“作业已存在”错误。已提交具有此标识的作业。返回的消息会描述此错误。


```
@ApiResponse(code = 409)
```
{: codeblock}

其他错误。返回异常的 JSON

```
@ApiResponse(code = 500)
```
{: codeblock}

PUT /v1/jobs/{id}

描述：创建或更新作业。如果具有此标识的作业不存在，那么将会创建该作业；否则，更新该作业（有效地重新提交该作业以便执行）。


内容类型：

```
@Consumes({“application/json”})
@Produces({“application/json”})
```
{: codeblock}

参数：

作为供应或绑定中凭证返回的访问键：


```
@QueryParam("accesskey")
```
{: codeblock}

用户指定的作业标识：

```
@PathParam("id")
```
{: codeblock}

JSON 作业定义（请参阅作业定义 JSON）：

```
@BodyParam
```
{: codeblock}

响应：

成功。已更新并提交作业以便执行。返回描述所提交作业的 JSON。


```
@ApiResponse(code = 200)
```
{: codeblock}

成功。已创建并提交作业以便执行。返回描述所提交作业的 JSON。


```
@ApiResponse(code = 201)
```
{: codeblock}

其他错误。返回异常的 JSON。

```
@ApiResponse(code = 500)
```
{: codeblock}

GET /v1/jobs

描述：返回此 Machine
Learning 服务实例上所有已定义作业的列表。

内容类型：

```
@Produces({“application/json”})
```
{: codeblock}

参数：

作为供应或绑定中凭证返回的访问键：


```
@QueryParam("accesskey")
```
{: codeblock}

响应：

成功。返回所有作业定义的 JSON 数组：


```
@ApiResponse(code = 200)
```
{: codeblock}

其他错误。返回异常的 JSON：

```
@ApiResponse(code = 500)
```
{: codeblock}

GET /v1/jobs/{id}

描述：请求返回特定作业定义。

内容类型：

```
@Produces({“application/json”})
```
{: codeblock}

参数：

作为供应或绑定中凭证返回的访问键：


```
@QueryParam("accesskey")
```
{: codeblock}

提交的作业标识：

```
@PathParam("id")
```
{: codeblock}

响应：

成功。返回描述所参考作业的 JSON：


```
@ApiResponse(code = 200)
```
{: codeblock}

“找不到作业”错误。返回的消息的详细信息：


```
@ApiResponse(code = 404)
```
{: codeblock}

其他错误。返回异常的 JSON：

```
@ApiResponse(code = 500)
```
{: codeblock}

GET /v1/jobs/{id}/status

描述：检索特定作业的状态。

内容类型：

```
@Produces({“application/json”})
```
{: codeblock}

参数：

作为供应或绑定中凭证返回的访问键：


```
@QueryParam("accesskey")
```
{: codeblock}

提交的作业标识：

```
@PathParam("id")
```
{: codeblock}

响应：

成功。返回描述作业状态的 JSON：


```
@ApiResponse(code = 200)
```
{: codeblock}

“找不到作业”错误，返回的消息的详细信息：

```
@ApiResponse(code = 404)
```
{: codeblock}

其他错误。返回异常的 JSON：

```
@ApiResponse(code = 500)
```
{: codeblock}

DELETE /v1/jobs/{ids}

描述：删除一个或多个作业。如果作业正在运行，那么将取消该作业。


内容类型：

```
@Produces({“application/json”})
```
{: codeblock}

参数：

作为供应或绑定中凭证返回的访问键：


```
@QueryParam("accesskey")
```
{: codeblock}

提交的作业标识或带有值的作业标识列表以“,”拆分


```
@PathParam("id" or “id,id,id”)
```
{: codeblock}

响应：

成功。返回已删除作业的数量：

```
@ApiResponse(code = 200)
```
{: codeblock}

其他错误。返回异常的 JSON：

```
@ApiResponse(code = 500)
```
{: codeblock}

## 作业定义 JSON

作业定义 JSON 包含以下一般部分：

作业类型和预测模型参考

操作类型：

*  TRAINING – 执行模型构建器节点培训或刷新模型块。
会在作业结果中检索更新的 Modeler 流。

*  EVALUATION – 用于评估受训模型的文档构建器和分析节点。


*  AUTO_REFRESH – 执行 TRAINING 并更新批处理文件上传空间中的文件内容。


*  BATCH_SCORE – 如作业定义所指示的那样，执行评分分支并导出所产生的数据。


*  RUN_STREAM – 如流属性中所指示的那样，执行 Modeler 流；常用在需要脚本执行时。


模型：在文件上传操作中针对批处理作业参考指定的标识。有关更多信息，请参阅“上传要在作业中使用的流文件”。
请注意，使用的是 /pm/v1/file。


```
"action": "TRAINING",
"model": {
     "id": "str2" 
     "name":"stream1.str" 
},
```
{: codeblock}

请注意，id 应该与 PUT API 中使用的文件标识相同。name 不是必要的，但是对于模型培训和自动刷新，将会使用在这里定义的名称来保存作业结果。
如果未定义 name，那么 Machine Learning 服务将会根据预定义的命名规则生成结果。


作业设置

运行此作业所需的所有设置。


```
"setting": {          
… Export settings
… Import settings
… Parameter value settings
… Document control settings
}
```
{: codeblock}

数据库连接定义

数据库类型：DB2、DashDB、Informix、Oracle、Sybase、SQLServer、MySQL。

数据库服务实例中指定的连接，许多数据库类型需要在“Options”中传递特定设置

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

源节点设置

用于在源节点名称所识别的 Modeler 流中查找给定分支源的参考数据库连接和表。


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

table 是数据库表名称。此表用于覆盖流源节点。
node 是流的数据源名称。dbRef 参考 dbDefinitions 对象定义的数据库连接，而 table 是用作 Modeler 流中给定分支输入的数据库表。
node 识别 Modeler 流中的原始源节点，该节点要由使用这些提供参数所构造的数据库源节点替换。


导出节点设置

用于在终端节点名称所识别的 Modeler 流中持久存储给定分支的数据的参考数据库连接和表。


通过 insertMode 属性连接持久存储数据时使用的控制方法：

*  Append – 表必须存在且适合插入

*  Create – 创建表（如果表已经存在，则会返回错误）

*  Drop – 如果参考的表存在，则会废弃并重建

*  Refresh – 在插入新行之前先从表删除现有的行

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

table 是要将作业结果写入的数据库表名称。node 是流的终端节点名称。与源节点设置类似，node 识别 Modeler 流中的原始输出节点，该节点要由使用这些提供参数所构造的数据库导出节点替换。


注：导出节点设置和源节点设置对于成功运行作业都非常重要。


参数值覆盖

要在作业执行之前覆盖流级别参数的缺省值，您可以指定要在作业定义中使用的名称/值对。


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

执行评估作业类型时返回的评估文档的预期报告格式

通过执行评估作业生成的所有文档会捆绑到一个 .zip 文件中。只有能够支持所指示 reportFormat 的文档构建器节点才会执行，并在成功执行作业之后返回其内容。


支持的格式包括 HTML、JPG、PNG、RTF、SAV、TAB 和 XML。

```
"reportFormat": "HTML"
```
{: codeblock}

完成 JSON 示例

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

## 批处理作业 API 详细信息

以下各部分提供了批处理作业 SPSS Modeler 文件管理 API 详细信息。

PUT /v1/file/{id}

描述：上传在批处理作业中使用的 SPSS Modeler 流文件。


注：如果在指定标识下已经存储文件，那么将会覆盖该文件。


内容类型：

```
@Consumes({ "multipart/form-data"  })
@Produces({“application/json”})
```
{: codeblock}

参数：

作为供应或绑定中凭证返回的访问键：


```
@QueryParam("accesskey")
```
{: codeblock}

用户指定的文件标识。在服务实例存储库中必须唯一：


```
@PathParam("id")
```
{: codeblock}

响应：

成功。返回可用于从服务实例存储库中下载文件的 URL：


```
@ApiResponse(code = 200)
```
{: codeblock}

其他错误。返回异常的 JSON：

```
@ApiResponse(code = 500)
```
{: codeblock}

DELETE /v1/file/{id}

描述：从服务实例存储库删除指定标识下存储的文件。


内容类型：

```
@Produces({“application/json”})
```
{: codeblock}

参数：

作为供应或绑定中凭证返回的访问键：


```
@QueryParam("accesskey")
```
{: codeblock}

上传时文件的用户指定标识：

```
@PathParam("id")
```
{: codeblock}

响应：

成功。已删除指定的文件：

```
@ApiResponse(code = 200)
```
{: codeblock}

错误。服务实例存储库中不存在此标识下存储的文件：


```
@ApiResponse(code = 404)
```
{: codeblock}

其他错误。返回异常的 JSON：

```
@ApiResponse(code = 500)
```
{: codeblock}

GET /v1/file/

描述：返回服务实例存储库中管理的所有文件的标识列表，以进行批处理作业处理。


内容类型：

```
@Produces({“application/json”})
```
{: codeblock}

参数：

作为供应或绑定中凭证返回的访问键：


```
@QueryParam("accesskey")
```
{: codeblock}

响应：

成功。返回文件标识值的 JSON 数组：


```
@ApiResponse(code = 200)
```
{: codeblock}

其他错误。返回异常的 JSON：

```
@ApiResponse(code = 500)
```
{: codeblock}

GET /v1/file/{id}

描述：检索指定标识下存储用于批处理作业处理的 SPSS Modeler 流文件。


内容类型：

```
@Produces({ "application/octet-stream" })
```
{: codeblock}

参数：

作为供应或绑定中凭证返回的访问键：


```
@QueryParam("accesskey")
```
{: codeblock}

上传时文件的用户指定标识：

```
@PathParam("id")
```
{: codeblock}

响应：

成功。返回 SPSS Modeler 文件：

```
@ApiResponse(code = 200)
```
{: codeblock}

错误。服务实例存储库中不存在此标识下存储的文件：


```
@ApiResponse(code = 404)
```
{: codeblock}

其他错误。返回异常的 JSON：

```
@ApiResponse(code = 500)
```
{: codeblock}
