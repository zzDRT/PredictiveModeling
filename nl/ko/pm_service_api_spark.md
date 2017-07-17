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

# Spark 및 Python 모델에 대한 Machine Learning 서비스 API


Machine Learning 서비스는
프로그래밍 언어에서 호출된 [REST API](https://watson-ml-api.mybluemix.net/) 세트이며
애플리케이션에서 Data Science Experience 개발 분석의 통합을 허용합니다. Bluemix 애플리케이션을
Machine Learning 서비스 인스턴스에 바인드하고
애플리케이션이 사용자에게 더 높은 가치를 전달하기 위해 필요한 예측 분석을 생성하십시오. 
관리 대시보드에서 모델을 관리하고, 대시보드를 사용하여 애플리케이션과 통합된 온라인, 일괄처리 또는 스트리밍 배치를 작성합니다. 

Watson Machine Learning 대시보드를 통해 Machine Learning 서비스의 인스턴스에서 사용 가능한 Data Science Experience 파일(Spark 및 Python 모델)을
관리합니다. 

*  샘플 모델 가져오기

*  모델 세부사항 표시

*  다음으로 모델 배치: 

   *  온라인 배치(스코어링)

   *  일괄처리 배치(IBM Object Storage 및 DashDB 지원)

   *  스트리밍 배치(IBM MessageHub 지원)

*  배치 세부사항 가져오기

*  배치 삭제


강력한 [REST API](https://watson-ml-api.mybluemix.net/)를 통해
서비스 인스턴스에 배치된 Data Science Experience 파일(Spark and Python 모델)에 대한 애플리케이션을 개발합니다. 

*  지정된 예측 모델에 대한 메타데이터 검색

*  모델 배치 및 배치된 모델 관리

*  지정된 배치에 대한 메타데이터 검색

*  배치된 모델에 대해 스코어링 요청을 작성하여 예측 분석 생성

사용 가능한 [REST API의 Swagger 표시](https://watson-ml-api.mybluemix.net/)를 통해 Machine Learning 서비스 기능을 쉽게 탐색할 수 있습니다. 

REST API 예제에 대한
다음 섹션을 참조하십시오. 

*  [온라인 배치 및 스코어링](pm_service_api_spark_online.html)

*  [Object Storage를 사용하여 일괄처리 배치](pm_service_api_spark_batch.html)

*  [MessageHub를 사용하여 스트리밍 배치](pm_service_api_spark_streaming.html)
