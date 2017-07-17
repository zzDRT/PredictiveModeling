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

# Spark 및 Python 모델과 함께 Machine Learning 서비스 사용


Machine Learning 서비스와 관련된 다음의 중요한 정보를 참고하십시오. 

*  지원되는 Machine Learning 프레임워크:

  *  Spark 2.0 MLlib
  *  scikit-learn 0.17이 있는 Python 3

*  비관리 모델은 지원되지 않습니다. 

*  Python scikit-learn 모델에 대한 일괄처리 및 스트림 배치가 이 단계에서는 지원되지 않습니다.

*  Spark 모델에 대한 일괄처리 및 스트림 지원은 베타에 있습니다. 참여하는 데 관심이 있으면 자신을 대기 목록에 추가하십시오!
자세한 정보는 [https://www.ibm.biz/mlwaitlist](https://www.ibm.biz/mlwaitlist)를 참조하십시오.

Node.js 샘플 코드를 다운로드하여 Machine Learning 서비스를 시도할 수 있습니다. Bluemix 애플리케이션을 작성하고
Machine Learning 서비스를 바인드하려면 다음 단계를 완료하십시오. 이 예제에서는
인기 있는 런타임인 Node.js를 사용합니다. iOS, Ruby, Perl 또는 Java와 같은 다른 런타임도 사용할 수 있습니다. 

또한 Cloud Foundry 도구(cf) 대신에 Bluemix 그래픽 인터페이스를 사용하여 1 - 3단계를 수행할 수도 있음을 참고하십시오. 

1. 서비스 인스턴스를 작성하려면 cf create-service 명령을 사용하십시오. 

   ```
   cf create-service pm-20 Free {local naming}
   ```
{: codeblock}

   예를 들어, 

   ```
   cf create-service pm-20 Free my_wml_free
   ```
{: codeblock}

   이 명령은 Bluemix 영역에서 ```my_wml_free```로 이름 지정된 무료 사용제로
Machine Learning 서비스 인스턴스 한 개를 작성합니다. 

2. 서비스 신임 정보를 작성하려면 cf create-service-key 명령을
사용하십시오. 

   ```
   cf create-service-key "{service instance name}" {vcap key name}
   ```
{: codeblock}

   예:

   ```
   cf create-service-key "IBM Watson Machine Learning - my instance" Credentials-1
   ```
{: codeblock}

   이 명령은 Machine Learning 서비스 신임 정보를 작성합니다. 

3. 서비스 인스턴스를 바인드하려면 cf bind-service 명령을 사용하십시오. 
   ```my_wml_free``` to your application.

   ```
   cf bind-service {AppName} my_wml_free
   ```
{: codeblock}

   예:

   ```
   cf bind-service my_app1 my_wml_free
   ```
{: codeblock}

   이 명령은 Machine Learning 서비스 인스턴스 my_wml_free를 Bluemix 애플리케이션 my_app1에 바인드합니다. 



Machine Learning 신임 정보:

   Machine Learning 서비스 인스턴스를 Bluemix 애플리케이션에 바인드한 후에 Machine Learning 신임 정보가 VCAP_SERVICES 환경 변수에 추가됩니다.

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

   VCAP_SERVICES 환경 변수에는 다음 정보가 포함되어 있습니다.


   * plan - 서비스 프로비저닝에 사용되는 Machine Learning 플랜입니다. 

   * url - Machine Learning 서비스 인스턴스의 주소입니다. 

   * access_key - SPSS Modeler 스트림의 경우에만 해당합니다.

   * instance_id - Machine Learning 인스턴스 고유 ID입니다.

   * username, password - 모든 요청에서 이 서비스 인스턴스로 전달하기 위한 액세스 토큰을 생성하는 데 필요한 기본 권한입니다. 예를 들어, 다음과 같이 curl을 사용하여 액세스 토큰을 생성할 수 있습니다. 

요청 예제: 

```
       curl --basic --user username:password https://ibm-watson-ml.mybluemix.net/v2/identity/token

       Output example:

       {"token":"**********"}
```
{: codeblock}

   다음 Node.js 코드는 VCAP_SERVICES 환경 변수에서 사용자 이름 및 비밀번호를 확보하는 방법에 대한 예제입니다. 

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
