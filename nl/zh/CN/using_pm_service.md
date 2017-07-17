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

# 将 Machine Learning 服务用于 IBM SPSS Modeler 模型

SPSS Modeler 建模选用板上提供的这些建模方法支持从数据派生新信息和开发预测模型。每种方法各有所长，最适合于解决特定类型的问题。
{: .shortdesc}

有关 SPSS Modeler 及其提供的建模算法的详细信息，请参阅 [IBM Knowledge Center](https://www.ibm.com/support/knowledgecenter/SS3RA7)。

在 Bluemix 应用程序和 SPSS Modeler 评分分支设计的输入和输出需求得到实现后，数据分析人员可以更改评分分支的任何内部方面。数据分析人员甚至可以更改刷新操作中使用的模型算法，以确保您能够优化预测性分析，而无需重写应用程序。

将 Machine Learning 服务用于 SPSS Modeler 中创建的模型时，请注意以下有关该服务的重要信息：

*  当评分分支准备使用实时评分时，应评分请求传入的输入数据必须替换设计到评分分支中的源节点，而产生的预测分析输出必须流回响应流（有效地替换评分分支设计中的终端节点）。


*  当评分分支准备在 Bluemix 中实时执行时，它无法要求与外部服务的连接。
例如，IBM Analytical Decision Management 评分分支设计无法包含 IBM SPSS Collaboration 和 Deployment Services 存储库中存储的规则或模型的参考。

*  在 Bluemix 中执行实时评分的评分分支无法要求外部服务。
例如，您无法针对实时需要 IBM SPSS
Analytic Server 和 Apache Hadoop 数据存储库的模型算法进行部署和评分。

*  Machine Learning 支持 Modeler 嵌入式 Python 脚本编制。
由于用于处理流的方法，在 Machine Learning 中运行流之前，存在几个限制。通常，如果用户选择控制流的执行，那么他们将参考分支的终端节点。
对于 Machine Learning，当我们处理流时，会识别 JSON 中将覆盖的节点，然后在流运行之前，执行替换。
这会导致流在脚本中失败，因为参考的输入和导出节点不再存在。
解决方案是使用其他节点的标识，在执行期间唯一识别分支。
这可确保流如嵌入式 Python 脚本中所定义的那样执行。

有关 IBM SPSS Analytic Server 培训预测模型当前支持的更多详细信息，请参阅 IBM Knowledge Center 的 Analytic Server 一节。

1. 您可以下载 Node.js 样本代码来尝试使用 Machine Learning 服务。要创建 Bluemix 应用程序并绑定 Machine Learning 服务，请完成以下步骤。以下示例使用 Node.js，因为它是最常用的运行时。可以使用其他运行时，例如 iOS、Ruby、Perl 或 Java。

2. 使用 cf create-service 命令创建服务实例：

   ```
   cf create-service pm-20 Free {local naming}
   ```

   例如：

   ```
   cf create-service pm-20 Free my_pm_free
   ```

   此命令将使用名为 my_pm_free 的免费套餐在 Bluemix 空间中创建一个 Machine Learning 服务实例。

3. 使用 `cf create-service-key` 命令创建服务凭证：

   ```cf create-service-key "{service instance name}" {vcap key name}```

   例如：

   ```cf create-service-key "IBM Watson Machine Learning - my instance" Credentials-1```

   此命令创建 Machine Learning 服务凭证。

4. 使用 cf bind-service 命令将服务实例 my_pm_free 绑定到应用程序。

   ```cf bind-service AppName my_pm_service```

   例如：

   ```cf bind-service my_app1 my_pm_free```

   此命令将 Machine Learning 服务实例 `my_pm_free` 绑定到 Bluemix 应用程序 my_app1。

5. Machine Learning 凭证：

   在您将 Machine Learning 服务实例绑定到 Bluemix 应用程序后，系统会将 Machine Learning 凭证添加到 `VCAP_SERVICES` 环境变量中：

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

   `VCAP_SERVICES` 环境变量包含以下信息：

   <dl>
   
   <dt>plan</dt>
   <dd>服务供应中使用的 Machine Learning 套餐。</dd>

   <dt>url</dt>
   <dd>Machine Learning 服务实例的地址。</dd>

   <dt>access_key</dt>
   <dd>要在对此服务实例的所有请求中传递的查询参数 accessKey。</dd>

   </dl>
            
例如：             

```
Get https://ibm-watson-ml.mybluemix.net/pm/v1/model/sales_model2?accesskey=XXXXXXXXXXXXX
```

   以下示例 Node.js 代码显示如何从 `VCAP_SERVICES` 环境变量获取 accessKey：

```
if (process.env.VCAP_SERVICES) {
var env = JSON.parse(process.env.VCAP_SERVICES);
        var credentials = env['pm-20'][0].credentials;
        var accessKey = credentials.access_key;
    }
```
