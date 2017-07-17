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

# IBM SPSS Modeler 모델과 함께 Machine Learning 서비스 사용

SPSS Modeler 모델링 팔레트에서 사용 가능한 모델링 메소드를 통해
데이터에서 새 정보를 이끌어내고 예측 모델을
개발할 수 있습니다. 각 메소드는 확실한 장점을 포함하고 있으며
특정 유형의 문제점에
가장 적합합니다.
{: .shortdesc}

SPSS Modeler 및 여기서 제공하는 모델링 알고리즘에 대한 세부사항은
[IBM Knowledge Center](https://www.ibm.com/support/knowledgecenter/SS3RA7)를 참조하십시오. 

Bluemix 애플리케이션 및 SPSS Modeler 스코어링 분기 디자인이 구현되고 나면
데이터 분석가는 스코어링 분기의 내부 측면을 변경할 수 있습니다. 데이터 분석가는 새로 고치기 오퍼레이션에서 사용되는 모델 알고리즘을
변경할 수도 있으며 애플리케이션을 다시 작성하지 않고 예측 분석을 정밀 조정할 수 있습니다.


SPSS Modeler에 작성된 모델과 함께 사용된 경우에는 Machine Learning 서비스와 관련된 다음의 중요한 정보를 참고하십시오. 

*  실시간 스코어링에서 사용할 수 있도록 스코어링 분기를 준비할 때
스코어 요청에서 수신되는 입력 데이터는 스코어링 분기로 디자인된 소스 노드를 대체해야 하며,
최종 예측 분석 결과물은 응답 플로우로 반환되어야 합니다(실제로 스코어링 분기 디자인의 터미널 노드를 대체함). 

*  Bluemix에서 스코어링 분기가 실시간 실행을 위해 준비되어 있으므로 이는 외부 서비스에 대한 연결이 필요하지 않습니다. 
예를 들어, IBM Analytical Decision Management 스코어링 분기 디자인에는
IBM SPSS Collaboration and Deployment Services 저장소에 저장된 규칙이나 모델에 대한 참조가 포함될 수 없습니다. 

*  Bluemix에서 실시간 스코어링에 대한 스코어링 분기의 실행은 외부 서비스를 요구할 수 없습니다. 
예를 들어, 실시간으로 IBM SPSS Analytic Server 및 Apache Hadoop 데이터 저장소가 필요한
모델 알고리즘에 대해 배치하고 스코어링할 수 없습니다. 

*  Machine Learning에서는 Modeler 임베디드 Python 스크립팅을 지원합니다. Machine Learning에서 실행되기 전에 스트림 처리에 사용된 방법으로 인한
몇 가지 제한사항이 있습니다. 일반적으로, 스트림의 실행을 제어하도록 선택한 경우 사용자는 분기의 터미널 노드를 참조합니다. Machine Learning의 경우,
스트림을 처리할 때는 대체될 JSON의 노드를 식별한 후에 스트림이 실행되기 전에 대체를 실행합니다. 
이에 따라 참조된 입력 및 내보내기 노드가 더 이상 존재하지 않으므로 스트림은 스크립트에서 실패합니다. 
솔루션은 다른 노드의 ID를 사용하여 실행 중에 분기를 고유하게 식별하는 것입니다. 
이는 임베디드 Python 스크립트에서 정의된 대로 스트림이 실행되도록 보장합니다. 

IBM SPSS Analytic Server 훈련 예측 모델의 최신 지원에 대한 세부사항은 IBM Knowledge Center의 Analytic Server 섹션을 참조하십시오. 

1. Node.js 샘플 코드를 다운로드하여 Machine Learning 서비스를 시도할 수 있습니다. Bluemix 애플리케이션을 작성하고
Machine Learning 서비스를 바인드하려면 다음 단계를 완료하십시오. 이 예제에서는
인기 있는 런타임인 Node.js를 사용합니다. iOS, Ruby, Perl 또는 Java와 같은 다른 런타임도 사용할 수 있습니다. 

2. 서비스 인스턴스를 작성하려면 cf create-service 명령을 사용하십시오. 

   ```
   cf create-service pm-20 Free {local naming}
   ```

   예:

   ```
   cf create-service pm-20 Free my_pm_free
   ```

   이 명령은 Bluemix 영역에서 my_pm_free로 이름 지정된 무료 사용제로
Machine Learning 서비스 인스턴스 한 개를 작성합니다. 

3. 서비스 신임 정보를 작성하려면 `cf create-service-key` 명령을
사용하십시오. 

   ```cf create-service-key "{service instance name}" {vcap key name}```

   예:

   ```cf create-service-key "IBM Watson Machine Learning - my instance" Credentials-1```

   이 명령은 Machine Learning 서비스 신임 정보를 작성합니다.

4. 서비스 인스턴스 my_pm_free를 애플리케이션에 바인드하려면
cf bind-service 명령을 사용하십시오. 

   ```cf bind-service AppName my_pm_service```

   예:

   ```cf bind-service my_app1 my_pm_free```

   이 명령은 Machine Learning 서비스 인스턴스
   `my_pm_free`를 Bluemix 애플리케이션 my_app1에 바인드합니다.

5. Machine Learning 신임 정보:

   Machine Learning 서비스 인스턴스를 Bluemix 애플리케이션에 바인드한 후에
   Machine Learning 신임 정보가 `VCAP_SERVICES` 환경 변수에 추가됩니다.


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

   `VCAP_SERVICES` 환경 변수에는 다음 정보가 포함되어 있습니다.


   <dl>
   
   <dt>plan</dt>
   <dd>서비스 프로비저닝에 사용되는 Machine Learning 플랜입니다. </dd>

   <dt>url</dt>
   <dd>Machine Learning 서비스 인스턴스의 주소입니다. </dd>

   <dt>access_key</dt>
   <dd>모든 요청에서 이 서비스 인스턴스로 전달하는 조회 매개변수 accessKey입니다.
</dd>

   </dl>
            
예를 들어,              

```
Get https://ibm-watson-ml.mybluemix.net/pm/v1/model/sales_model2?accesskey=XXXXXXXXXXXXX
```

   `VCAP_SERVICES` 환경 변수에서 accessKey를 얻는 방법을 보여주는 예제 Node.js 코드입니다. 

```
   if (process.env.VCAP_SERVICES) {
        var env = JSON.parse(process.env.VCAP_SERVICES);
        var credentials = env['pm-20'][0].credentials;
        var accessKey = credentials.access_key;
    }
```
