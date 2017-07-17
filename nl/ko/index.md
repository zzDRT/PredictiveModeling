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

# Watson Machine Learning 시작하기
{: #WMLgettingstarted}

IBM Watson™ Machine Learning을 사용하여 예측 분석을 애플리케이션과
통합하십시오. 데이터 과학자는 Machine Learning을 사용하여 보다 스마트한 의사결정을 내리고 까다로운 문제점을 해결하며 사용자 성과를 개선하는 애플리케이션을 개발합니다.
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



Machine Learning 서비스는 프로그래밍 언어에서 호출할 수 있는 REST API 세트입니다. 

Machine Learning 서비스의 초점은
배치에 있지만, 모델 및 파이프라인 작성 및 관련 작업에는 IBM SPSS Modeler 또는
Data Science Experience가 필요하다는 점을 참고하십시오. SPSS Modeler 및 Data Science Experience(Spark MLlib 및 Python scikit-learn 활용)는 모두 기계 학습, 인공 지능 및 통계에서 가져온 다양한 모델링 메소드를 제공합니다. 

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

시작할 준비가 되셨습니까? 서비스의 인스턴스를 작성하거나 애플리케이션을
바인드하려면 [Spark 및 Python 모델과 함께 서비스 사용](using_pm_service_dsx.html) 또는
[SPSS 모델과 함께 서비스 사용](using_pm_service.html)을 참조하십시오.

API 탐색에 관심이 있는 경우, [Spark 및 Python 모델용 Machine Learning
서비스 API](pm_service_api_spark.html) 또는 [IBM SPSS Modeler 모델용 Machine Learning 서비스
API](pm_service_api_spss.html)를 참조하십시오. 

SPSS Modeler 및 여기서 제공하는 모델링 알고리즘에 대한 세부사항은
[IBM Knowledge Center](https://www.ibm.com/support/knowledgecenter/SS3RA7)를 참조하십시오. 

IBM Data Science Experience 및 여기서 제공하는 모델링 알고리즘에 대한 세부사항은 [https://datascience.ibm.com](https://datascience.ibm.com)을 참조하십시오. 
