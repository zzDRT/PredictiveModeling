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

# 온라인 모델 배치


**시나리오 이름**: 고객 만족도 예측

**시나리오 설명**: Telco Company에서는 어떤 고객이 떠날 위험성이 있는지 알려고 합니다. 회사는 해당 문제를 해결하는 데 도움이 되는 솔루션을 제공하도록 요청합니다. 데이터 과학자는 예측 모델을 준비하고
사용자(개발자)와 이를 공유합니다. 여러분의 태스크는
모델을 배치하고 배치된 모델에 대한 스코어 요청을 작성하여 예측 분석을 생성하는 것입니다. 

## 샘플 모델 사용

1. 여기 Git 저장소에서 샘플 모델을 다운로드하십시오. 

2. 새 모델을 업로드하려면 다음 요청을 사용하십시오. 

```
   curl -X PUT -H "Content-Type:multipart/form-data;" -F "model_file=@customer-satisfaction-prediction.str" https://ibm-watson-ml-dev.stage1.mybluemix.net/pm/v1/model/context_csp2?accesskey=pcB4lKG1brpgSCEonNoBdzew9kiOOzN8awh1cJ2sHAxf1yyjh50nnrQngWw4DD/tM13eGXGHaJ0voQU+cAi1t/nmJBaSgi+xeMY8Wia68PB227SsqjgA5nvrX+eU9Sbr
```
{: codeblock}

   응답: 

```
   {"flag":true,"message":"success to upload stream with given context id context_csp2","url":"http://pmdevlb.pmservice.ibmcloud.com:80/pm/v1/model/context_csp2?accesskey=pcB4lKG1brpgSCEonNoBdzew9kiOOzN8awh1cJ2sHAxf1yyjh50nnrQngWw4DD/tM13eGXGHaJ0voQU+cAi1t/nmJBaSgi+xeMY8Wia68PB227SsqjgA5nvrX+eU9Sbr"}
```
{: codeblock}

3. 기존 모델을 새로 고치려면 다음 요청을 사용하십시오. 

```
   curl -X PUT -H "Content-Type:multipart/form-data;" -F "model_file=@customer-satisfaction-prediction.str" https://ibm-watson-ml-dev.stage1.mybluemix.net/pm/v1/model/context_csp2?accesskey=pcB4lKG1brpgSCEonNoBdzew9kiOOzN8awh1cJ2sHAxf1yyjh50nnrQngWw4DD/tM13eGXGHaJ0voQU+cAi1t/nmJBaSgi+xeMY8Wia68PB227SsqjgA5nvrX+eU9Sbr
```
{: codeblock}

   응답: 

```
   {"flag":true,"message":"success to update stream with given context id context_csp2","url":"http://pmdevlb.pmservice.ibmcloud.com:80/pm/v1/model/context_csp2?accesskey=pcB4lKG1brpgSCEonNoBdzew9kiOOzN8awh1cJ2sHAxf1yyjh50nnrQngWw4DD/tM13eGXGHaJ0voQU+cAi1t/nmJBaSgi+xeMY8Wia68PB227SsqjgA5nvrX+eU9Sbr"}
```
{: codeblock}

4. 배치된 모든 모델의 목록을 가져오려면 다음 요청을 사용하십시오. 

```
   curl -X GET -i -H "Content-Type:*/*" https://ibm-watson-ml-dev.stage1.mybluemix.net/pm/v1/model?accesskey=pcB4lKG1brpgSCEonNoBdzew9kiOOzN8awh1cJ2sHAxf1yyjh50nnrQngWw4DD/tM13eGXGHaJ0voQU+cAi1t/nmJBaSgi+xeMY8Wia68PB227SsqjgA5nvrX+eU9Sbr
```
{: codeblock}

   응답: 

```
   [{"stream":"customer-satisfaction-prediction.str","id":"context_csp2"}]
```
{: codeblock}

5. 배치된 특정 모델의 사본을 다운로드하려면 다음 요청을 사용하십시오. 

```
   curl -X GET -v -H "Content-Type:*/*" https://ibm-watson-ml-dev.stage1.mybluemix.net/pm/v1/model/context_csp2?accesskey=pcB4lKG1brpgSCEonNoBdzew9kiOOzN8awh1cJ2sHAxf1yyjh50nnrQngWw4DD/tM13eGXGHaJ0voQU+cAi1t/nmJBaSgi+xeMY8Wia68PB227SsqjgA5nvrX+eU9Sbr >> output.str
```
{: codeblock}

   응답(모델의 컨텐츠를 output.str 파일로 다운로드함): 

```
   > GET /pm/v1/model/context_csp2?accesskey=pcB4lKG1brpgSCEonNoBdzew9kiOOzN8awh1cJ2sHAxf1yyjh50nnrQngWw4DD/tM13eGXGHaJ0voQU+cAi1t/nmJBaSgi+xeMY8Wia68PB227SsqjgA5nvrX+eU9Sbr HTTP/1.1
   
   > Host: ibm-watson-ml-dev.stage1.mybluemix.net
   
   > User-Agent: curl/7.50.3
   
   > Accept: */*
   
   > Content-Type:*/*

   >

   * STATE: DO => DO_DONE handle 0x600057960; line 1664 (connection #0)

   * STATE: DO_DONE => WAITPERFORM handle 0x600057960; line 1791 (connection #0)

   * STATE: WAITPERFORM => PERFORM handle 0x600057960; line 1801 (connection #0)

   { [5 bytes data]

   * HTTP 1.1 or later with persistent connection, pipelining supported

   < HTTP/1.1 200 OK

   < X-Backside-Transport: OK OK

   < Connection: Keep-Alive

   < Transfer-Encoding: chunked

   < Content-Disposition: attachment; filename=customer-satisfaction-prediction.str

   < Content-Type: application/octet-stream

   < Date: Fri, 17 Feb 2017 15:55:20 GMT

   * Server nginx/1.11.5 is not blacklisted

   < Server: nginx/1.11.5

   < Set-Cookie: NSC_qnefw-bggjojuz=ffffffff0987c39d45525d5f4f58455e445a4a421548;expires=Fri, 17-Feb-2017 15:57:20 GMT;path=/;httponly

   < X-Powered-By: Servlet/3.0

   < X-Global-Transaction-ID: 3450392319

   <
   
   0 0 0 0 0 0 0 0 --:--:-- 0:00:01 --:--:-- 0{ [5 bytes data]

   100 74410 0 74410 0 0 15966 0 --:--:-- 0:00:04 --:--:-- 15978* STATE: PERFORM => DONE handle 0x600057960; line 1965 (connection #0)

   * multi_done

   * Curl_http_done: called premature == 0

   100 99k 0 99k 0 0 21114 0 --:--:-- 0:00:04 --:--:-- 24508

   * Connection #0 to host ibm-watson-ml-dev.stage1.mybluemix.net left intact
```
{: codeblock}

6. 배치된 모델 삭제: 

```
   curl -X DELETE -v -H "Content-Type:*/*" https://ibm-watson-ml-dev.stage1.mybluemix.net/pm/v1/model/context_csp2?accesskey=pcB4lKG1brpgSCEonNoBdzew9kiOOzN8awh1cJ2sHAxf1yyjh50nnrQngWw4DD/tM13eGXGHaJ0voQU+cAi1t/nmJBaSgi+xeMY8Wia68PB227SsqjgA5nvrX+eU9Sbr
```
{: codeblock}

   응답: 

```
   > DELETE /pm/v1/model/context_csp2?accesskey=pcB4lKG1brpgSCEonNoBdzew9kiOOzN8awh1cJ2sHAxf1yyjh50nnrQngWw4DD/tM13eGXGHaJ0voQU+cAi1t/nmJBaSgi+xeMY8Wia68PB227SsqjgA5nvrX+eU9Sbr HTTP/1.1
   
   > Host: ibm-watson-ml-dev.stage1.mybluemix.net
   
   > User-Agent: curl/7.50.3
   
   > Accept: */*
   
   > Content-Type:*/*
   
   > 
   
   < HTTP/1.1 200 OK

   < X-Backside-Transport: OK OK

   < Connection: Keep-Alive

   < Transfer-Encoding: chunked

   < Content-Type: application/json

   < Date: Fri, 17 Feb 2017 16:21:20 GMT

   * Server nginx/1.11.5 is not blacklisted

   < Server: nginx/1.11.5

   < Set-Cookie: NSC_qnefw-bggjojuz=ffffffff0987c39d45525d5f4f58455e445a4a421548;expires=Fri, 17-Feb-2017 16:23:20 GMT;path=/;httponly

   < X-Powered-By: Servlet/3.0

   < X-Global-Transaction-ID: 2495867153

   <
   
   * STATE: PERFORM => DONE handle 0x600057960; line 1965 (connection #0)

   * multi_done

   * Curl_http_done: called premature == 0

   * Connection #0 to host ibm-watson-ml-dev.stage1.mybluemix.net left intact

   {"flag":true,"message":"success to delete stream with specified id context_csp2"}

   Response when trying to delete a model that doesn't exist (or
   was already deleted):

   > DELETE /pm/v1/model/context_csp2?accesskey=pcB4lKG1brpgSCEonNoBdzew9kiOOzN8awh1cJ2sHAxf1yyjh50nnrQngWw4DD/tM13eGXGHaJ0voQU+cAi1t/nmJBaSgi+xeMY8Wia68PB227SsqjgA5nvrX+eU9Sbr HTTP/1.1

   > Host: ibm-watson-ml-dev.stage1.mybluemix.net

   > User-Agent: curl/7.50.3
   
   > Accept: */*
   
   > Content-Type:*/*

   >

   * STATE: DO => DO_DONE handle 0x600057960; line 1664 (connection #0)

   * STATE: DO_DONE => WAITPERFORM handle 0x600057960; line 1791 (connection #0)

   * STATE: WAITPERFORM => PERFORM handle 0x600057960; line 1801 (connection #0)

   * HTTP 1.1 or later with persistent connection, pipelining supported

   < HTTP/1.1 404 Not Found

   < X-Backside-Transport: FAIL FAIL

   < Connection: Keep-Alive

   < Transfer-Encoding: chunked

   < Content-Type: application/json

   < Date: Fri, 17 Feb 2017 16:23:15 GMT

   * Server nginx/1.11.5 is not blacklisted

   < Server: nginx/1.11.5

   < Set-Cookie: NSC_qnefw-bggjojuz=ffffffff0987c39d45525d5f4f58455e445a4a421548;expires=Fri, 17-Feb-2017 16:25:15 GMT;path=/;httponly

   < X-Powered-By: Servlet/3.0

   < X-Global-Transaction-ID: 94445277

   <
   
   * STATE: PERFORM => DONE handle 0x600057960; line 1965 (connection #0)

   * multi_done

   * Curl_http_done: called premature == 0

   * Connection #0 to host ibm-watson-ml-dev.stage1.mybluemix.net left intact

   Not Found Model:context_csp2
```
{: codeblock}

7. 배치된 모델 스코어링: 

```
   curl -X POST -v -H "Content-Type:application/json;charset=UTF-8" -d '{"tablename":"Input data","header":["customerID","gender","SeniorCitizen","Partner","Dependents","tenure","PhoneService","MultipleLines","InternetService","OnlineSecurity","OnlineBackup","DeviceProtection","TechSupport","StreamingTV","StreamingMovies","Contract","PaperlessBilling","PaymentMethod","MonthlyCharges","TotalCharges","Churn","SampleWeight"],"data":[["9237-HQITU","Female",0,"No","No",2,"Yes","No","Fiber optic","No","No","No","No","No","No","Month-to-month","Yes","Electronic check",70.700,151.650,"Yes",1.000]]}' https://ibm-watson-ml-dev.stage1.mybluemix.net/pm/v1/score/context_csp2?accesskey=pcB4lKG1brpgSCEonNoBdzew9kiOOzN8awh1cJ2sHAxf1yyjh50nnrQngWw4DD/tM13eGXGHaJ0voQU+cAi1t/nmJBaSgi+xeMY8Wia68PB227SsqjgA5nvrX+eU9Sbr
```
{: codeblock}

   응답: 

```
   > POST /pm/v1/score/context_csp2?accesskey=pcB4lKG1brpgSCEonNoBdzew9kiOOzN8awh1cJ2sHAxf1yyjh50nnrQngWw4DD/tM13eGXGHaJ0voQU+cAi1t/nmJBaSgi+xeMY8Wia68PB227SsqjgA5nvrX+eU9Sbr HTTP/1.1

   > Host: ibm-watson-ml-dev.stage1.mybluemix.net

   > User-Agent: curl/7.50.3
   
   > Accept: */*
   
   > Content-Type:application/json;charset=UTF-8
   
   > Content-Length: 525
   
   > 
   
   < HTTP/1.1 200 OK

   < X-Backside-Transport: OK OK

   < Connection: Keep-Alive

   < Transfer-Encoding: chunked

   < Content-Type: application/json;charset=UTF-8

   < Date: Fri, 17 Feb 2017 16:40:05 GMT

   * Server nginx/1.11.5 is not blacklisted

   < Server: nginx/1.11.5

   < Set-Cookie: NSC_qnefw-bggjojuz=ffffffff0987c39d45525d5f4f58455e445a4a421548;expires=Fri, 17-Feb-2017 16:42:05 GMT;path=/;httponly

   < X-Powered-By: Servlet/3.0

   < X-Global-Transaction-ID: 3291552207



   [{"header":["customerID","Churn","Predicted Churn","Probability of Churn"],"data":[["9237-HQITU","Yes","Yes",0.8829830706957551]]}]
```
{: codeblock}
