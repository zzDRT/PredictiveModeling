---

copyright:
  years: 2016, 2017
lastupdated: "2017-08-03"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tsSymptoms: .tsSymptoms}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}

# Troubleshooting

Here are the answers to common troubleshooting questions about using IBM Watson Machine Learning.
{: shortdesc}

## Getting help and support for Watson Machine Learning
{: #gettinghelp}

If you have problems or questions when using Watson Machine Learning, you can get help by searching for information or by asking questions through a forum. You can also open a support ticket.

When using the forums to ask a question, tag your question so that it is seen by the machine learning development teams.

If you have technical questions about machine learning, post your question on <a href="http://stackoverflow.com/search?q=machine-learning+ibm-bluemix" target="_blank">Stack Overflow <img src="../icons/launch-glyph.svg" alt="External link icon"></a> and tag your question with "ibm-bluemix" and "machine-learning".

For questions about the service and getting started instructions, use the <a href="https://developer.ibm.com/answers/topics/machine-learning/?smartspace=bluemix" target="_blank">IBM developerWorks dW Answers <img src="../icons/launch-glyph.svg" alt="External link icon"></a> forum. Include the  "machine-learning" and "bluemix" tags.
See [Getting help](https://console.bluemix.net/docs/support/index.html#getting-help) for more details about using the forums.

For information about opening an IBM support ticket, or about support levels and ticket severities, see [Contacting support](https://console.bluemix.net/docs/support/index.html#contacting-support).

## Authorization token has not been provided.
{: #ts_missing_authorization_token}
 
The REST API cannot be invoked successfully.
{: tsSymptoms}
 
Authorization token has not been provided in the `Authorization` header.
{: tsCauses}
 
Pass authorization token in the `Authorization` header.
{: tsResolve}
       
## Invalid authorization token.
{: #ts_invalid_authorization_token}
 
The REST API cannot be invoked successfully.
{: tsSymptoms}
 
Authorization token which has been provided cannot be decoded or parsed.
{: tsCauses}
 
Pass correct authorization token in the `Authorization` header.
{: tsResolve}
       
## Authorization token and instance_id which was used in the request are not the same.
{: #ts_not_matching_authorization_token}
 
The REST API cannot be invoked successfully.
{: tsSymptoms}
 
The Authorization token which has been used is not generated for the service instance against which it was used.
{: tsCauses}
 
Pass authorization token in the `Authorization` header which corresponds to the service instance which is being used.
{: tsResolve}
       
## Authorization token is expired.
{: #ts_expired_authorization_token}
 
The REST API cannot be invoked successfully.
{: tsSymptoms}
 
Authorization token is expired.
{: tsCauses}
 
Pass not expired authorization token in the `Authorization` header.
{: tsResolve}
       
## Public key needed for authentication is not available.
{: #ts_missing_public_key}
 
The REST API cannot be invoked successfully.
{: tsSymptoms}
 
This is internal service issue.
{: tsCauses}
 
The issue needs to be fixed by support team.
{: tsResolve}
       
## Operation timed out after {{timeout}}
{: #ts_operation_timeout}
 
The REST API cannot be invoked successfully.
{: tsSymptoms}
 
The timeout occurred during performing requested operation.
{: tsCauses}
 
Try to invoke desired operation again.
{: tsResolve}
       
## Unhandled exception of type {{type}} with {{status}}
{: #ts_unhandled_exception_with_status}
 
The REST API cannot be invoked successfully.
{: tsSymptoms}
 
This is internal service issue.
{: tsCauses}
 
Try to invoke desired operation again. If it occurs more times than it needs to be fixed by support team.
{: tsResolve}
       
## Unhandled exception of type {{type}} with {{response}}
{: #ts_unhandled_exception_with_response}
 
The REST API cannot be invoked successfully.
{: tsSymptoms}
 
This is internal service issue.
{: tsCauses}
 
Try to invoke desired operation again. If it occurs more times than it needs to be fixed by support team.
{: tsResolve}
       
## Unhandled exception of type {{type}} with {{json}}
{: #ts_unhandled_exception_with_json}
 
The REST API cannot be invoked successfully.
{: tsSymptoms}
 
This is internal service issue.
{: tsCauses}
 
Try to invoke desired operation again. If it occurs more times than it needs to be fixed by support team.
{: tsResolve}
       
## Unhandled exception of type {{type}} with {{message}}
{: #ts_unhandled_exception_with_message}
 
The REST API cannot be invoked successfully.
{: tsSymptoms}
 
This is internal service issue.
{: tsCauses}
 
Try to invoke desired operation again. If it occurs more times than it needs to be fixed by support team.
{: tsResolve}
       
## Requested object could not be found.
{: #ts_not_found}
 
The REST API cannot be invoked successfully.
{: tsSymptoms}
 
The request resource could not be found.
{: tsCauses}
 
Ensure that you are referring to the existing resource.
{: tsResolve}
       
## Underlying database reported too many requests.
{: #ts_too_many_cloudant_requests}
 
The REST API cannot be invoked successfully.
{: tsSymptoms}
 
The user has sent too many requests in a given amount of time.
{: tsCauses}
 
Try to invoke desired operation again.
{: tsResolve}
       
 ## The definition of the evaluation is not defined neither in the artifactModelVersion nor in the deployment. It needs to be specified \" +\n      \"at least in one of the places.
{: #ts_missing_evaluation_definition}
 
The REST API cannot be invoked successfully.
{: tsSymptoms}
 
Learning Configuration does not contain all required information
{: tsCauses}
 
Provide `definition` in `learning configuration`
{: tsResolve}
       
## Evaluation requires learning configuration specified for the model.
{: #ts_missing_learning_configuration}
 
There is no possibility to create `learning iteration`.
{: tsSymptoms}
 
There is no `learning configuration` defined for the model.
{: tsCauses}
 
Create `learning configuration` and try to create `learning iteration` again.
{: tsResolve}
       
## Evaluation requires spark instance to be provided in `X-Spark-Service-Instance` header
{: #ts_missing_spark_definition_for_evaluation}
 
The REST API cannot be invoked successfully.
{: tsSymptoms}
 
There is no all required information in `learning configuration`
{: tsCauses}
 
Provide `spark_service` in Learning Configuration or in `X-Spark-Service-Instance` header
{: tsResolve}
       
## Model does not contain any version.
{: #ts_missing_latest_model_version}
 
There is no possibility to create neither deployment nor set learning configuration.
{: tsSymptoms}
 
There is inconsistency related to the persistence of the model.
{: tsCauses}
 
Try to persist the model again and try perform the action again.
{: tsResolve}
       
## Patch operation can only modify existing learning configuration.
{: #ts_patch_non_existing_learning_configuration}
 
There is no possibility to invoke patch REST API method to patch learning configuration.
{: tsSymptoms}
 
There is no `learning configuration` set for this model or model does not exist.
{: tsCauses}
 
Endure that model exists and has already learning configuration set.
{: tsResolve}
       
## Patch operation expects exactly one replace operation.
{: #ts_patch_multiple_ops}
 
The deployment cannot be patched.
{: tsSymptoms}
 
The patch payload contains more than one operation or the patch operation is different than `replace`.
{: tsCauses}
 
Use only one operation in the patch payload which is `replace` operation
{: tsResolve}
       
## The given payload is missing required fields: [${fields.mkString(\",\")}] or the values of the fields are corrupted.
{: #ts_invalid_request_payload}
 
There is no possibility to process action which is related to access to the underlying data set.
{: tsSymptoms}
 
The access to the data set is not properly defined.
{: tsCauses}
 
Correct the access definition for the data set.
{: tsResolve}
       
## Provided evaluation method: $method is not supported. Supported values: [${supported.mkString(\",\")}].
{: #ts_evaluation_method_not_supported}
 
There is no possibility to create learning configuration.
{: tsSymptoms}
 
The wrong evaluation method was used to create learning configuration.
{: tsCauses}
 
Use supported evaluation method which is one of: `regression`, `binary`, `multiclass`.
{: tsResolve}
       
## There can be only one active evaluation per model. Request could not be completed because of existing active evaluation: {{url}}
{: #ts_active_evaluation_conflict}
 
There is no possibility to create another learning iteration
{: tsSymptoms}
 
There can be only one running evaluation for the model.
{: tsCauses}
 
See the already running evaluation or wait till it ends and start the new one.
{: tsResolve}
       
## The deployment type {{type}} is not supported.
{: #ts_not_supported_deployment_type}
 
There is no possibility to create deployment.
{: tsSymptoms}
 
Not supported deployment type was used.
{: tsCauses}
 
Supported deployment type should be used.
{: tsResolve}
       
## Incorrect input: ({{message}})
{: #ts_deserialization_error}
 
The REST API cannot be invoked successfully.
{: tsSymptoms}
 
There is an issue with parsing json.
{: tsCauses}
 
Ensure that the correct json is passed in the request.
{: tsResolve}
       
## Insufficient data - metric {{name}} could not be calculated
{: #ts_missing_metric}
 
Learning iteration has failed
{: tsSymptoms}
 
Value for metric with defined threshold could not be calculated because of insufficient feedback data
{: tsCauses}
 
Review and improve data in data source `feedback_data_ref` in `learning configuration`
{: tsResolve}
       
## For type {{type}} spark instance must be provided in `X-Spark-Service-Instance` header
{: #ts_missing_prediction_spark_definition}
 
Deployment cannot be created
{: tsSymptoms}
 
`batch` and `streaming` deployments require spark instance to be provided
{: tsCauses}
 
Provide spark instance in `X-Spark-Service-Instance` header
{: tsResolve}
       
## Action {{action}} has failed with message {{message}}
{: #ts_http_client_error}
 
The REST API cannot be invoked successfully.
{: tsSymptoms}
 
There was an issue with invoking underlying service.
{: tsCauses}
 
If there is suggestion how to fix the issue than follow it. Contact the support team if there is no suggestion in the message or the suggestion does not solve the issue.
{: tsResolve}
       
## Path `{{path}}` is not allowed. Only allowed path for patch stream is `/status`
{: #ts_wrong_stream_patch_path}
 
There is no possibility to patch the stream deployment.
{: tsSymptoms}
 
The wrong path was used to patch the `stream` deployment.
{: tsCauses}
 
Patch the `stream` deployment with supported path option which is `/status` (it allows to start/stop stream processing.
{: tsResolve}
       
## Patch operation is not allowed for instance of type `{{$type}}`
{: #ts_patch_not_supported}
 
There is no possibility to patch the deployment.
{: tsSymptoms}
 
The wrong deployment type is being patched.
{: tsCauses}
 
Patch the `stream` deployment type.
{: tsResolve}
       
## Data connection `{{data}}` is invalid for feedback_data_ref
{: #ts_invalid_feedback_data_connection}
 
There is no possibility to create `learning configuration` for the model.
{: tsSymptoms}
 
Not supported data source was used when defining feedback_data_ref.
{: tsCauses}
 
Use only supported data source type which is `dashdb`
{: tsResolve}
       
## Path {{path}} is not allowed. Only allowed path for patch model is `/deployed_version/url` or `/deployed_version/href` for V2
{: #ts_patch_model_path_not_allowed}
 
There is no option to patch model.
{: tsSymptoms}
 
The wrong path was used during patching of the model.
{: tsCauses}
 
Patch model with supported path which allows to update the version of deployed model.
{: tsResolve}
       
## Parsing failure: {{msg}}
{: #ts_parsing_error}
 
The REST API cannot be invoked successfully.
{: tsSymptoms}
 
The requested payload could not be parsed successfully.
{: tsCauses}
 
Ensure that your request payload is correct and can be parsed correctly.
{: tsResolve}
       
## Runtime environment for selected model: {{env}} is not supported for `learning configuration`. Supported environments: [{{supported_envs}}].
{: #ts_runtime_env_not_supported}
 
There is no option to create `learning configuration`
{: tsSymptoms}
 
The model for which the `learning_configuration` was tried to be created is not supported.
{: tsCauses}
 
Create `learning configuration` for model which has supported runtime.
{: tsResolve}
       
## Current plan \'{{plan}}\' only allows {{limit}} deployments
{: #ts_deployments_plan_limit_reached}
 
There is no possibility to create deployment.
{: tsSymptoms}
 
The limit for number of deployments was reached for the current plan.
{: tsCauses}
 
Upgrade to the plan which does not have such limitation.
{: tsResolve}
       
## Database connection definition is not valid ({{code}})
{: #ts_sql_error}
 
There is no possibility utilize the `learning configuration` functionality.
{: tsSymptoms}
 
Database connection definition is not valid.
{: tsCauses}
 
Try to fix the issue which is described by `code` returned by underlying database.
{: tsResolve}
       
## There were problems while connecting underlying {{system}}
{: #ts_stream_tcp_error}
 
The REST API cannot be invoked successfully.
{: tsSymptoms}
 
There was an issue during connection to the underlying system. It might be temporary network issue.
{: tsCauses}
 
Try to invoke desired operation again. If it occurs more times than contact support team.
{: tsResolve}
       
## Error extracting X-Spark-Service-Instance header: ({{message}})
{: #ts_spark_header_deserialization_error}
 
There is no possibility to invoke REST API which requires Spark credentials
{: tsSymptoms}
 
There is an issue with base-64 decoding or parsing Spark credentials.
{: tsCauses}
 
Ensure that the correct Spark credentials were correctly base-64 encoded. For details, please look at documentation.
{: tsResolve}
       
## This functionality is forbidden for non beta users.
{: #ts_not_beta_user}
 
The desired REST API cannot be invoked successfully.
{: tsSymptoms}
 
REST API which was invoked is currently in beta.
{: tsCauses}
 
If you are interested in participating, add yourself to the wait list. The details can be found in documentation.
{: tsResolve}
       
## {{code}} {{message}}
{: #ts_underlying_api_error}
 
The REST API cannot be invoked successfully.
{: tsSymptoms}
 
There was an issue with invoking underlying service.
{: tsCauses}
 
If there is suggestion how to fix the issue then follow it. Contact the support team if there is no suggestion in the message or the suggestion does not solve the issue.
{: tsResolve}
       
## Rate limit exceeded.
{: #ts_rate_limit_exceeded}
 
Rate limit exceeded.
{: tsSymptoms}
 
Rate limit for current plan has been exceeded.
{: tsCauses}
 
To solve this problem, acquire another plan with a greater rate limit
{: tsResolve}
       
## Invalid query parameter `{{paramName}}` value: {{value}}
{: #ts_invalid_query_parameter_value}
 
Validation error as passed incorrect value for query parameter.
{: tsSymptoms}
 
Error in getting result for query.
{: tsCauses}
 
Correct query parameter value. The details can be found in documentation.
{: tsResolve}
       
## Invalid token type: {{type}}
{: #ts_invalid_token_type}
 
Error regarding token type.
{: tsSymptoms}
 
Error in authorization.
{: tsCauses}
 
Token should be started with `Bearer` prefix
{: tsResolve}
       
## Invalid token format. Bearer token format should be used.
{: #ts_invalid_token_format}
 
Error regarding token format.
{: tsSymptoms}
 
Error in authorization.
{: tsCauses}
 
Token should be bearer token and should start with `Bearer` prefix
{: tsResolve}
              
