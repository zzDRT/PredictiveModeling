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

# 사용자 정의 모델의 개발 및 지속성

## 사용자 정의 모델에 대해 작업

### 시나리오 이름: 제품 라인 예측

### 시나리오 설명:

고객은 유럽에서 가장 유명한 체인점 중 하나를 운영하고 있습니다.
개인 용품, 캠핑 장비 및 아웃도어 보호 장비와 같은 제품 라인에 관해서 고객의 관심 분야를 확인할 수 있도록
사용자에게 요청합니다. 데이터 과학자는 예측 모델을 개발하고 이를 사용자(개발자)와 공유합니다. 여러분의 태스크는
모델을 배치하고 배치된 모델에 대한 스코어 요청을 작성하여 예측 분석을 생성하는 것입니다. 

### 사용자 정의 모델의 개발 및 지속성

1. Data Science Experience를 사용하여 사용자 정의 모델을 작성합니다. 등록한 이후에 로그인하여 다음 단계를 완료하십시오. 

2. 처음으로 로그인하는 경우 조직 및 영역을 작성하도록 프롬프트가 표시됩니다. **계속**을 클릭하고
기본값을 허용하십시오. 

3. 조직이 작성되고 나면, **내 프로젝트**로 이동하여
**프로젝트 작성**을 클릭하십시오. 

4. 프로젝트에 이름 및 설명을 지정한 후 **작성**을 클릭하십시오. 사용자가 지정한
프로젝트 이름도 대상 컨테이너 이름으로 사용됩니다. 

5. 프로젝트가 작성되고 나면, 노트북을 추가하고 고유 모델을 개발하거나
다음 샘플 노트북 중 하나를 업로드할 수 있습니다. 

   *  [Python으로 Spark MLlib 모델 개발](https://apsportal.ibm.com/analytics/notebooks/89492fd6-a641-4819-9176-3d9381561df9/view?access_token=d80bef1a172d1d83d3721b101886337158457281774186f181a2e6a5b57f5ec7)

   *  [Scala로 Spark MLlib 모델 개발](https://apsportal.ibm.com/analytics/notebooks/c8652d2c-bfc9-4354-8168-f1c9f7f8dfc2/view?access_token=02a83fea8450a452c8de76af98dae078459d0f56810ddef4f4c62d5bc4fc72cf)

   *  [Python으로 scikit-learn 모델 개발](https://apsportal.ibm.com/analytics/notebooks/5215a61a-16d7-4fa2-b060-e3e243ceebe3/view?access_token=70f48c95c5571a614ce97484d3f168b1d9b6aeebce015187d3d77ce6038f025e)



### 사용자 정의 모델의 배치 및 스코어링

모델 배치 및 스코어링에 관련된 지시사항은 다음 섹션을 참조하거나 이전에 연결된 노트북의 스코어링 섹션을 참조하십시오. 

*  [온라인 모델 배치](pm_service_api_spark_online.html)

*  [일괄처리 모델 배치](pm_service_api_spark_batch.html)

*  [스트리밍 모델 배치](pm_service_api_spark_streaming.html)
