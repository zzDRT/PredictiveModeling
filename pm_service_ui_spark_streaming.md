---

copyright:
  years: 2016, 2017
lastupdated: "2017-08-04"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Deploying streaming models <span class='tag--beta'>Beta</span>

**Note**: This functionality is currently in beta and only available
for use with Spark MLlib. If you're interested in participating, add yourself to the wait list! For more information, see: [https://www.ibm.biz/mlwaitlist](https://www.ibm.biz/mlwaitlist).

**Scenario name**: Sentiment Analysis.

**Scenario description**: A marketing agency wants to know the
sentiment about a particular topic. The agency would like us to
develop a model that classifies a given statement as POSITIVE or
NEGATIVE. A Data Scientist prepares a predictive model and shares
it with you (the developer). Your task is to deploy the model and
generate predictive analytics by making score requests against
the deployed model.

See this [document](https://github.com/pmservice/tweet-sentiment-prediction) for more information.

## Using the sample model

1. Go to the Samples tab of the IBM® Watson™ Machine Learning
   Dashboard.
2. In the Sample Models section, find the Sentiment Prediction
   tile and click the Add model button (+).

Now you'll see the sample Sentiment Prediction model in the list
of available models on the Models tab.


## Creating a streaming deployment with IBM Message Hub

1.  Go to the Models tab of the IBM® Watson™ Machine Learning Dashboard.
2.  From ACTIONS menu select Create Deployment.
3.  In Create Deployment form provide Name, Description and Stream Type.
4.  You must provide the following inputs:

    **Input Connection**: IBM Message Hub topics details which are used as input (tweets) for the model and storage for the model output  (prediction results).

    ```
  {
     "connection":{
        "kafka_brokers_sasl":[
           "kafka01-prod01.messagehub.services.us-south.bluemix.net:9093",
           "kafka02-prod01.messagehub.services.us-south.bluemix.net:9093",
           "kafka03-prod01.messagehub.services.us-south.bluemix.net:9093",
           "kafka04-prod01.messagehub.services.us-south.bluemix.net:9093",
           "kafka05-prod01.messagehub.services.us-south.bluemix.net:9093"
        ],
        "kafka_admin_url":"https://kafka-admin-prod01.messagehub.services.us-south.bluemix.net:443",
        "api_key":"Dv5kEVNNsbuJ9RFEKdUhIn2hruipIrsBolge6v1uQmTzEQti",
        "mqlight_lookup_url":"https://mqlight-lookup-prod01.messagehub.services.us-south.bluemix.net/Lookup?serviceId=5448397d-cb22-4698-8a2b-ffb04f43a4cb",
        "kafka_rest_url":"https://kafka-rest-prod01.messagehub.services.us-south.bluemix.net:443",
        "user":"Dv5kEVNNsbuJ9RFE",
        "password":"KdahIn2hruipIrsBolge6v1uQmTzEQti"
     },
     "source":{
        "topic":"sinput",
        "type":"kafka"
     }
  }
    ```
    {: codeblock}

    **Output Connection**

    ```
 {
    "connection":{
       "kafka_brokers_sasl":[
          "kafka01-prod01.messagehub.services.us-south.bluemix.net:9093",
          "kafka02-prod01.messagehub.services.us-south.bluemix.net:9093",
          "kafka03-prod01.messagehub.services.us-south.bluemix.net:9093",
          "kafka04-prod01.messagehub.services.us-south.bluemix.net:9093",
          "kafka05-prod01.messagehub.services.us-south.bluemix.net:9093"
       ],
       "kafka_admin_url":"https://kafka-admin-prod01.messagehub.services.us-south.bluemix.net:443",
       "api_key":"Dv5kEVNNsbuJ9RFEKdUhIn2hruipIrsBolge6v1uQmTzEQti",
       "mqlight_lookup_url":"https://mqlight-lookup-prod01.messagehub.services.us-south.bluemix.net/Lookup?serviceId=5448397d-cb22-4698-8a2b-ffb04f43a4cb",
       "kafka_rest_url":"https://kafka-rest-prod01.messagehub.services.us-south.bluemix.net:443",
       "user":"Dv5kEVNNsbuJ9RFE",
       "password":"KdahIn2hruipIrsBolge6v1uQmTzEQti"
    },
    "target":{
       "topic":"soutput",
       "type":"kafka"
    }
 }
    ```
    {: codeblock}

    **Spark Connection**: Spark service Credentials can be found on the Service Credentials tab of the Bluemix Spark service dashboard.

     ```
{
     "credentials":{
       "tenant_id": "s745-299dcf850a6390-35c9a7ecf27a",
       "tenant_id_full": "ba3dde5a-ee64-4057-9749-299dcf850a63_4c55eb1c-d6fe-4f0a-9390-35c9a7ecf27a",
       "cluster_master_url": "https://spark.bluemix.net",
       "instance_id": "ba3dde5a-ee64-4057-9749-299dcf850a63",
       "tenant_secret": "c0cba7a4-7b19-46e6-9326-44c4f48aaf08",
       "plan": "ibm.SparkService.PayGoPersonal"
     },
     "version":"2.0"
}
     ```
     {: codeblock}

5. Click **Save**.

The prediction result is sent to defined MessageHub topic (Output connection).

## Obtaining deployment details

You can check the status, and parameters related to the deployed model.

1. Go to the Deployments tab of the IBM® Watson™ Machine Learning
   Dashboard.
2. From **Actions** menu, click **View Details**.

## Deleting a streaming deployment

You can delete the deployment if it's no longer needed using a
query such as the following sample.

1. Go to the Deployments tab of the IBM® Watson™ Machine Learning
   Dashboard.
2. From ACTIONS menu select Delete.
