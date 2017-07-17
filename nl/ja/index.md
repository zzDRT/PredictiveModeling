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

<!-- This template is for getting started with a Bluemix service. It is a task template intended to document productive use of the service. It is not intended for discovery and conceptual information.  -->

# Watson Machine Learning の概要
{: #WMLgettingstarted}

IBM Watson™ Machine Learning を使用すると、予測分析をアプリケーションと統合できます。データ・サイエンティストは、機械学習を利用することで、よりスマートな意思決定、難しい問題の解決、ユーザーの成果の向上を実現するアプリケーションを開発できます。
{: shortdesc}

<!-- If overview content is required, do not include it here. Put it in a separate "## About" section below the task section. -->

<!-- Task section: REQUIRED
The task section includes steps to integrate the service into the app.  
- With task-based, technical information, reduce the conversational style in favor of succinct and direct instructions.
- DO include the basic, most-common-use scenario steps to use the service or integrate it into the app.
- DO NOT include steps to add the service from the Bluemix catalog; we assume that the user already took steps in the UI to add the service.
- DO include code snippets in all languages that can be copied, as well as VCAP service info.  
- For additional tasks like configuring, managing, etc., add a task section (## Gerund_task_title) below the task section or "About" section if used. Use a task title such as "Configuring x", "Administering y", "Managing z". -->

<!-- You can include an optional prerequisites paragraph for any prerequisites to be met before integrating the service. For example: -->

<!-- Include a sentence to briefly introduce the steps. Examples: -->



Machine Learning サービスは、任意のプログラミング言語から呼び出すことができる一連の REST API で構成されます。

Machine Learning サービスはデプロイメントに重点を置いていますが、モデルやパイプラインを作成して操作するには、IBM SPSS Modeler または Data Science Experience が必要であることに注意してください。SPSS Modeler と Data Science Experience (Spark MLlib および Python scikit-learn を利用) はどちらも、機械学習、人工知能、統計学から取得したさまざまなモデリング手法を提供します。

<!-- Related links section: REQUIRED.
Related links display in the upper right of the getting started page.
Ensure that you retain the lowercase anchor IDs (eg. {: #rellinks}) as shown in this template. These are used as IDs during transform and the doc framework keys off the IDs for display.
The headings coded here are not actually used. The doc framework provides the correct headings.
Also ensure that the related links stay in position at the end of this file or the doc framework will not display them properly.
Use {:new_window} for external links to open a new window.-->
<!-- Please delete all comments within the related links section to avoid breaking the build. Thanks. -->

<!--  Related Links
{: #rellinks} -->

<!-- ## Tutorials and Samples
{: #samples} -->

さあ始めましょう。サービス・インスタンスの作成またはアプリケーションのバインドについては、『[Spark モデルおよび Python モデルを用いたサービスの使用](using_pm_service_dsx.html)』または『[SPSS モデルを用いたサービスの使用](using_pm_service.html)』を参照してください。

API の探索に関心がある場合は、[Spark モデルおよび Python モデル用の Machine Learning サービス API](pm_service_api_spark.html) または [IBM SPSS Modeler モデル用の Machine Learning サービス API](pm_service_api_spss.html) を参照してください。

SPSS Modeler の概要と提供されるモデリング・アルゴリズムについて詳しくは、[IBM Knowledge Center](https://www.ibm.com/support/knowledgecenter/SS3RA7) を参照してください。

IBM Data Science Experience の概要と提供されるモデリング・アルゴリズムについて詳しくは、[https://datascience.ibm.com](https://datascience.ibm.com) を参照してください。
