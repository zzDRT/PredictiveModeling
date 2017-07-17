---

copyright:
  years: 2016, 2017
lastupdated: "2017-07-14"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Deploying batch models <span class='tag--beta'>Beta</span>

**Note**: This functionality is currently in beta and only available
for use with Spark MLlib. If you're interested in participating, add yourself to the wait list! For more information, see: [https://www.ibm.biz/mlwaitlist](https://www.ibm.biz/mlwaitlist).

**Scenario name**: Customer satisfaction prediction.

**Scenario description**: A telecommunications company wants to know
which customers are at risk of leaving. The presented model
predicts customer churn. A data scientist develops a predictive
model and shares it with you (the developer). Your task is to
deploy the model and generate predictive analytics by making
score requests against the deployed model.

## Using the sample model

1.  Go to the Samples tab of the IBM® Watson™ Machine Learning Dashboard.

2.  In the Sample Models section, find the Customer Satisfaction Prediction tile and click the Add model button (+).

Now you'll see the sample Customer Satisfaction Prediction model
in the list of available models on the Models tab.

## Creating a batch deployment with Object Storage

1.  Go to the Models tab of the IBM® Watson™ Machine Learning Dashboard.

2.  From the **Actions** menu, click **Create Deployment**.

3.  In Create Deployment form provide Name, Description and Batch Type.

4.  You must provide the following inputs:

    **Input Connection**: Object Storage details, which will be used as input (customer data to score) for the model and storage for the model output (results.csv in this case, which is automatically created).

    ```
       {
          "source":{
             "fileformat":"csv",
             "firstlineheader":"true",
             "container":"batchjob",
             "inferschema":"1",
             "filename":"TelcoCustomerData.csv",
             "type":"bluemixobjectstorage"
          },
          "connection":{
             "projectid":"252341ed707d4558b5b2da245e785cd7",
             "userid":"b2d83cf6056e040ddb91ca00a2686c7d3",
             "region":"dallas",
             "authurl":"https://identity.open.softlayer.com",
             "password":"eJ_y9R^OE{j?8Ub!!"
          }
       }
    ```
    {: codeblock}
    
    **Output Connection**
    
    ```
       {
          "target":{
             "fileformat":"csv",
             "firstlineheader":"true",
             "container":"batchjob",
             "inferschema":"1",
             "filename":"result.csv",
             "type":"bluemixobjectstorage"
          },
          "connection":{
             "projectid":"252341ed707d4558b5b2da245e785cd7",
             "userid":"b2d83cf6056e040ddb91ca00a2686c7d3",
             "region":"dallas",
             "authurl":"https://identity.open.softlayer.com",
             "password":"eJ_y9R^OE{j?8Ub!!"
          }
       }
    ```
    {: codeblock}
    
    **Spark Connection**: Spark service Credentials can be found on the Service Credentials tab of the Bluemix Spark service dashboard.
    
    ```
{
      "tenant_id": "s745-299dcf850a6390-35c9a7ecf27a",  
      "tenant_id_full": "ba3dde5a-ee64-4057-9749-299dcf850a63_4c55eb1c-d6fe-4f0a-9390-35c9a7ecf27a",  
      "cluster_master_url": "https://spark.bluemix.net",  
      "instance_id": "ba3dde5a-ee64-4057-9749-299dcf850a63",  
      "tenant_secret": "c0cba7a4-7b19-46e6-9326-44c4f48aaf08",  
      "plan": "ibm.SparkService.PayGoPersonal"
}
    ```
    {: codeblock}
    
5.  Click **Save**.

The prediction result is saved to a .csv file in IBM Object Storage. Following is a sample row.
    
Input file preview:
    
```
customerID,gender,SeniorCitizen,Partner,Dependents,tenure,PhoneService,MultipleLines,InternetService,OnlineSecurity,OnlineBackup,DeviceProtection,TechSupport,StreamingTV,StreamingMovies,Contract,PaperlessBilling,PaymentMethod,MonthlyCharges,TotalCharges,Churn
7590-VHVEG,Female,0,Yes,No,1,No,No phone service,DSL,No,Yes,No,No,No,No,Month-to-month,Yes,Electronic check,29.85,29.85,No
5575-GNVDE,Male,0,No,No,34,Yes,No,DSL,Yes,No,Yes,No,No,No,One year,No,Mailed check,56.95,1889.5,No
3668-QPYBK,Male,0,No,No,2,Yes,No,DSL,Yes,Yes,No,No,No,No,Month-to-month,Yes,Mailed check,53.85,108.15,Yes
```
{: codeblock}

Output file preview:

```
InternetService, Contract, tenure, MonthlyCharges, Churn
Fiber optic, Month-to-month, 2, 70.7, 1
DSL, Two year, 58, 59.9, 0
DSL, Month-to-month, 1, 30.2, 1
DSL, Month-to-month, 17, 64.7, 1
DSL, Month-to-month, 13, 76.2, 0
DSL, Month-to-month, 25, 69.5, 1
Fiber optic, Month-to-month, 8, 80.65, 1
No, One year, 52, 20.4, 0
Fiber optic, Two year, 64, 111.6, 0
Fiber optic, Month-to-month, 1, 79.35, 1
```
{: codeblock}


## Obtaining deployment details

You can check the status, and parameters related to the deployed model.

1. Go to the Deployments tab of the IBM® Watson™ Machine Learning
   Dashboard.

2. From ACTIONS menu select View Details.


## Deleting a batch deployment

You can delete the deployment if it's no longer needed using a
query such as the following sample.

1. Go to the Deployments tab of the IBM® Watson™ Machine Learning
   Dashboard.

2. From ACTIONS menu select Delete.
