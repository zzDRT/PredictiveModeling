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

# 使用 Machine Learning 服務搭配 IBM SPSS Modeler 模型

SPSS Modeler 建模選用區上的可用建模方法可讓您從資料中衍生新資訊，以及開發預測模型。每一個方法有特定強度，最適合特定類型的問題。
{: .shortdesc}

如需 SPSS Modeler 及其提供之建模演算法的詳細資料，請參閱 [IBM Knowledge Center](https://www.ibm.com/support/knowledgecenter/SS3RA7)。

在實作 Bluemix 應用程式及 SPSS Modeler 評分分支設計的輸入及輸出需求之後，「資料分析師」可以變更評分分支的任何內部層面。「資料分析師」甚至可以變更某個重新整理作業中使用的模型演算法，確保您有能力細部調整預測分析，而不需要重寫應用程式。

搭配在 SPSS Modeler 中建立之模型使用時，請注意下列有關 Machine Learning 服務的重要資訊：

*  準備好評分分支以用於即時評分時，評分要求上進入的輸入資料必須取代設計到評分分支中的來源節點，而且產生的預測分析輸出必須流回回應流程（實際上會取代評分分支設計中的終端機節點）。

*  因為評分分支是為了在 Bluemix 中進行即時執行而準備，所以不需要連線至外部服務。例如，IBM Analytical Decision Management 評分分支設計無法包含 IBM SPSS Collaboration and Deployment Services 儲存庫中所儲存規則或模型的參照。

*  為了在 Bluemix 中進行即時評分而執行評分分支時，不需要外部服務。例如，您無法針對需要 IBM SPSS Analytic Server 及 Apache Hadoop 資料儲存庫的模型演算法，即時進行部署及評分。

*  Machine Learning 支援 Modeler 內嵌式 Python Scripting。由於在 Machine Learning 中執行之前用於處理串流的方法之故，會有一些限制。一般來說，如果使用者選擇控制串流的執行，則會參照分支的終端機節點。對於 Machine Learning，我們在處理串流時，會識別 JSON 中將置換的節點，然後在串流執行之前執行取代。這會導致串流在 Script 中失敗，因為所參照的輸入及匯出節點已不存在。解決方案是在執行期間使用另一個節點的 ID 來唯一識別分支。這可確保串流會如內嵌式 Python Script 中所定義的方式執行。

如需 IBM SPSS Analytic Server 訓練之預測模型最新支援的詳細資料，請參閱 IBM Knowledge Center 的「分析伺服器」小節。

1. 您可以下載 Node.js 範例程式碼，以嘗試 Machine Learning 服務。請完成下列步驟來建立 Bluemix 應用程式及連結 Machine Learning 服務。這些範例使用 Node.js，因為它是熱門的運行環境。可使用其他的運行環境，例如 iOS、Ruby、Perl 或 Java。

2. 使用 cf create-service 指令來建立服務實例：

   ```
   cf create-service pm-20 Free {local naming}
   ```

   例如：

   ```
   cf create-service pm-20 Free my_pm_free
   ```

   這個指令會在 Bluemix 空間建立一個 Machine Learning 服務實例，其中包含名為 my_pm_free 的 Free 方案。

3. 使用 `cf create-service-key` 指令來建立服務認證：

   ```cf create-service-key "{service instance name}" {vcap key name}```

   例如：

   ```cf create-service-key "IBM Watson Machine Learning - my instance" Credentials-1```

   這個指令會建立 Machine Learning 服務認證。

4. 使用 cf bind-service 指令，將服務實例 my_pm_free 連結到應用程式。

   ```cf bind-service AppName my_pm_service```

   例如：

   ```cf bind-service my_app1 my_pm_free```

   這個指令將 Machine Learning 服務實例 `my_pm_free` 連結到 Bluemix 應用程式 my_app1。

5. Machine Learning 認證：

   將 Machine Learning 服務實例連結到 Bluemix 應用程式之後， 認證會新增至 `VCAP_SERVICES` 環境變數：

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

   `VCAP_SERVICES` 環境變數包括下列資訊：

   <dl>
   
   <dt>plan</dt>
   <dd>使用於服務供應的 Machine Learning 方案。</dd>

   <dt>url</dt>
   <dd>Machine Learning 服務實例的位址。</dd>

   <dt>access_key</dt>
   <dd>查詢參數 accessKey，它能將所有要求傳入這個服務實例。</dd>

   </dl>
            
例如：             

```
Get https://ibm-watson-ml.mybluemix.net/pm/v1/model/sales_model2?accesskey=XXXXXXXXXXXXX
```

   下列範例 Node.js 程式碼顯示如何從 `VCAP_SERVICES` 環境變數取得 accessKey：

```
   if (process.env.VCAP_SERVICES) {
        var env = JSON.parse(process.env.VCAP_SERVICES);
        var credentials = env['pm-20'][0].credentials;
        var accessKey = credentials.access_key;
    }
```
