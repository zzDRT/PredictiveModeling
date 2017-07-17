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

# Watson Machine Learning 入门
{: #WMLgettingstarted}

使用 IBM Watson™ Machine Learning，可以将预测性分析与您的应用程序相集成。
数据研究员使用 Machine Learning 来开发可进行更智能决策、解决困难问题并改进用户成果的应用程序。
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



Machine Learning 服务是一组 REST API，可通过任何编程语言进行调用。

Machine Learning 服务的重点是部署，但是请注意，需要 IBM SPSS Modeler 或 Data Science Experience 才能进行创作并使用模型和管道。SPSS Modeler 和 Data Science Experience（利用 Spark MLlib 和 Python scikit-learn）提供了多种建模方法，它们来源于机器学习、人工智能和统计信息。

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

准备好开始了吗？要创建服务的实例或绑定应用程序，请参阅[将服务用于 Spark 和 Python 模型](using_pm_service_dsx.html)或[将服务用于 SPSS 模型](using_pm_service.html)。


如果您想要探索 API，请参阅 [Spark 和 Python 模型的 Machine Learning 服务 API](pm_service_api_spark.html) 或 [IBM SPSS Modeler 模型的 Machine Learning 服务 API](pm_service_api_spss.html)。


有关 SPSS Modeler 及其提供的建模算法的详细信息，请参阅 [IBM Knowledge Center](https://www.ibm.com/support/knowledgecenter/SS3RA7)。

有关 IBM Data Science Experience 及其提供的建模算法的详细信息，请参阅 [https://datascience.ibm.com](https://datascience.ibm.com)。
