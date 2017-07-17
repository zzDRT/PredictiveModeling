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

# 将 Machine Learning 服务用于 Spark 和 Python 模型


请注意以下有关 Machine
Learning 服务的重要信息：

*  支持的 Machine Learning 框架：

  *  Spark 2.0 MLlib
  *  带有 scikit-learn 0.17 的 Python 3

*  不支持无人管理的模型。

*  此阶段不支持 Python scikit-learn 模型的批量和流部署

*  Beta 中 Spark 模型的批量和流支持。如果您想要参与，请将您自己加入等待列表！
有关更多信息，请参阅：[https://www.ibm.biz/mlwaitlist](https://www.ibm.biz/mlwaitlist)。

您可以下载 Node.js 样本代码来尝试使用 Machine Learning 服务。要创建 Bluemix 应用程序并绑定 Machine Learning 服务，请完成以下步骤。以下示例使用 Node.js，因为它是最常用的运行时。可以使用其他运行时，例如 iOS、Ruby、Perl 或 Java。

请注意，还可以使用 Bluemix 图形界面（而不使用 Cloud Foundry 工具 (cf)）执行步骤 1-3。

1. 使用 cf create-service 命令创建服务实例：

   ```
   cf create-service pm-20 Free {local naming}
   ```
{: codeblock}

   例如：

   ```
   cf create-service pm-20 Free my_wml_free
   ```
{: codeblock}

   此命令将使用名为 ```my_wml_free``` 的免费套餐在 Bluemix 空间中创建一个 Machine Learning 服务实例。

2. 使用 cf create-service-key 命令创建服务凭证：

   ```
   cf create-service-key "{service instance name}" {vcap key name}
   ```
{: codeblock}

   例如：

   ```
   cf create-service-key "IBM Watson Machine Learning - my instance" Credentials-1
   ```
{: codeblock}

   此命令创建 Machine Learning 服务凭证。

3. 使用 cf bind-service 命令绑定服务实例
   ```my_wml_free``` to your application.

   ```
   cf bind-service {AppName} my_wml_free
   ```
{: codeblock}

   例如：

   ```
   cf bind-service my_app1 my_wml_free
   ```
{: codeblock}

   此命令将 Machine Learning 服务实例 my_wml_free 绑定到 Bluemix 应用程序 my_app1。



Machine Learning 凭证：

   在您将 Machine Learning 服务实例绑定到 Bluemix 应用程序后，系统会将 Machine Learning 凭证添加到 VCAP_SERVICES 环境变量中：

```
    {
     "pm-20-dev": [
       {
         "credentials": {
"url":
           "https://ibm-watson-ml.mybluemix.net",
           "access_key": "**********",
           "username": "**********",
           "password": "**********",
           "instance_id": "**********"
         },
           "label": "pm-20-dev",
           "plan": "Free",
           "name": "IBM Watson Machine Learning”
       }
     ]
    }
```
{: codeblock}

   VCAP_SERVICES 环境变量包含以下信息：

   * plan - 服务供应中使用的 Machine Learning 套餐。

   * url - Machine Learning 服务实例的地址。

   * access_key - 仅限 SPSS Modeler 流。

   * instance_id - Machine Learning 实例唯一标识。

   * username、password - 要生成将传入对此服务实例的所有请求中的访问令牌所需的基本授权。例如，可以使用 curl 生成访问令牌，如下所示：

请求示例：

```
       curl --basic --user username:password https://ibm-watson-ml.mybluemix.net/v2/identity/token

       输出示例：

       {"token":"**********"}
```
{: codeblock}

   以下 Node.js 代码是如何从 VCAP_SERVICES 环境变量获取 username 和 password 的示例：

```
if (process.env.VCAP_SERVICES) {
var env = JSON.parse(process.env.VCAP_SERVICES);
        var credentials = env['pm-20'][0].credentials;
        var username = credentials.username;
        var password = credentials.password;
        var instance_id = credentials.instance_id;
    }
```
{: codeblock}
