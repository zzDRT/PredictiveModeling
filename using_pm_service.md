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

# Using the service

The modeling methods available on the SPSS Modeler modeling
palette enable you to derive new information from your data and
to develop predictive models. Each method has certain strengths
and is best suited for particular types of problems.
{: .shortdesc}

For details
about SPSS Modeler and the modeling algorithms it provides, see
[IBM Knowledge Center](https://www.ibm.com/support/knowledgecenter/SS3RA7).

After the input and output requirements of your Bluemix
application and SPSS Modeler scoring branch design are
implemented, your Data Analyst can change any internal aspect of
the scoring branch. The Data Analyst can even change the model
algorithm(s) used in a refresh operation, ensuring your ability
to fine-tune your predictive analytics without needing to rewrite
your applications.

Note the following important information regarding the Machine
Learning service when used with models created in SPSS Modeler:

*  When a scoring branch is prepared for use in real-time
   scoring, input data coming in on the score request must
   replace the source node designed into the scoring branch and
   the resulting predictive analytic output must flow back into
   the response flow (effectively replacing the terminal node in
   the scoring branch design).

*  As the scoring branch is prepared for real-time execution in
   Bluemix, it cannot require a connection to an external
   service. For example, an IBM Analytical Decision Management
   scoring branch design cannot contain references to rules or
   models stored in an IBM SPSS Collaboration and Deployment
   Services repository.

*  The execution of a scoring branch for real-time scoring in
   Bluemix cannot require an external service. For example, you
   cannot deploy and score against model algorithms that require
   an IBM SPSS Analytic Server and Apache Hadoop data store in
   real-time.

*  Machine Learning supports Modeler embedded Python scripting.
   There are a few restrictions due to the method used for
   processing streams before they run in Machine Learning.
   Typically, if a user chooses to control the execution of the
   stream, they will reference the terminal node of the branch.
   For Machine Learning, when we process the stream, we identify
   the nodes from JSON that will be overridden, and then do the
   replacement before the stream runs. This causes the stream to
   fail in the script because the referenced input and export
   nodes no longer exist. The solution is use the ID of another
   node to uniquely identify the branch during execution. This
   ensures that the stream executes as defined in the embedded
   Python script.

For more details about current support for IBM SPSS Analytic
Server-trained predictive models, see the Analytic Server section
of IBM Knowledge Center.

1. You can download Node.js sample code to try the Machine
   Learning service. Complete the following steps to create your
   Bluemix application and bind the Machine Learning service.
   These examples use Node.js because it is a popular runtime.
   Others can be used such as iOS, Ruby, Perl, or Java.

2. Use the cf create-service command to create a service
   instance:

   ```
   cf create-service pm-20 Free {local naming}
   ```
   {: codeblock}

   For example:

   ```
   cf create-service pm-20 Free my_pm_free
   ```
   {: codeblock}

   This command creates one Machine Learning service instance
   with Free plan named my_pm_free in your Bluemix space.

3. Use the `cf create-service-key` command to create service
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

4. Use the cf bind-service command to bind the service instance
   my_pm_free to your application.

   ```
   cf bind-service AppName my_pm_service
   ```
   {: codeblock}

   For example:

   ```
   cf bind-service my_app1 my_pm_free
   ```
   {: codeblock}

   This command binds the Machine Learning service instance
   `my_pm_free` to the Bluemix application my_app1.

5. Machine Learning credentials:

   After you bind the Machine Learning service instance to your
   Bluemix application, the Machine Learning credentials are
   added to the `VCAP_SERVICES` environment variable:

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
{: codeblock}

   The `VCAP_SERVICES` environment variable includes the following
   information:

   <dl>

   <dt>plan</dt>
   <dd>The Machine Learning plan that is used in the service provisioning.</dd>

   <dt>url</dt>
   <dd>The address of the Machine Learning service instance.</dd>

   <dt>access_key</dt>
   <dd>The query parameter accessKey to pass in all requests
            to this service instance.</dd>

   </dl>

For example:             

```
Get https://ibm-watson-ml.mybluemix.net/pm/v1/model/sales_model2?accesskey=XXXXXXXXXXXXX
```
{: codeblock}

   Example Node.js code that shows how to obtain the accessKey
   from the `VCAP_SERVICES` environment variable:

```
   if (process.env.VCAP_SERVICES) {
        var env = JSON.parse(process.env.VCAP_SERVICES);
        var credentials = env['pm-20'][0].credentials;
        var accessKey = credentials.access_key;
    }
```
{: codeblock}
