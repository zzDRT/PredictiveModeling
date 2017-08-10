---

copyright:
  years: 2016, 2017
lastupdated: "2017-08-09"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Using the service

Note the following important information regarding the Machine
Learning service.



## Supported machine learning frameworks and features

*  Supported frameworks:
  *  Spark 2.0 MLlib
  *  Python 3 with scikit-learn 0.17
*  Classification and Regression pipeline models are currently supported.
*  For scikit-learn models predictions without probabilities are returned at this stage.
*  Batch and stream deployment for Python scikit-learn models is not supported at this stage.
*  Batch and stream support for Spark models is in beta. If you're interested in participating, add yourself to the wait list! For more information, see: [https://www.ibm.biz/mlwaitlist](https://www.ibm.biz/mlwaitlist).



## Steps to bind the service with Bluemix application

You can download Node.js sample code to try the Machine
Learning service. Complete the following steps to create your Bluemix application and bind the Machine Learning service. These examples use Node.js because it is a popular runtime. Others can be used such as iOS, Ruby, Perl, or Java.

Note that you can also perform steps 1-3 using the Bluemix Graphical Interface instead of the Cloud Foundry tool (cf).

1. Use the cf create-service command to create a service instance:

   ```
   cf create-service pm-20 Free {local naming}
   ```
   {: codeblock}

   For example:

   ```
   cf create-service pm-20 Free my_wml_free
   ```
   {: codeblock}

   This command creates one Machine Learning service instance
   with the Free plan named `my_wml_free` in your Bluemix space.

2. Use the cf create-service-key command to create service
   credentials:

   ```
   cf create-service-key "{service instance name}" {vcap key name}
   ```
   {: codeblock}

   For example:

   ```
   cf create-service-key "IBM Watson Machine Learning - my instance" Credentials-1
   ```
   {: codeblock}

   This command creates Machine Learning service credentials.

3. Use the cf bind-service command to bind the service instance
   `my_wml_free` to your application.

   ```
   cf bind-service {AppName} my_wml_free
   ```
   {: codeblock}

   For example:

   ```
   cf bind-service my_app1 my_wml_free
   ```
   {: codeblock}

   This command binds the Machine Learning service instance
   my_wml_free to the Bluemix application my_app1.



## Machine Learning credentials

After you bind the Machine Learning service instance to your Bluemix application, the Machine Learning credentials are added to the VCAP_SERVICES environment variable:

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

   The VCAP_SERVICES environment variable includes the following
   information:

   * ``plan`` - the Machine Learning plan that is used in the service provisioning.
   * ``url`` - the address of the Machine Learning service instance.
   * ``access_key`` - for SPSS Modeler streams only.
   * ``instance_id`` - Machine Learning instance unique identifier.
   * ``username``, ``password`` - basic authorization needed to generate an access token to pass in all requests to this service instance. For example, you can generate an access token using a curl like this:

Request example:

```
       curl --basic --user username:password https://ibm-watson-ml.mybluemix.net/v3/identity/token

       Output example:

       {"token":"**********"}
```
{: codeblock}

   The following Node.js code is an example of how to obtain the
   username and password from the VCAP_SERVICES environment
   variable:

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
