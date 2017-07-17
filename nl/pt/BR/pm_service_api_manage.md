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

# Gerenciando os modelos SPSS implementados


*  [Implementando ou atualizando um modelo preditivo](#deploying-or-refreshing-a-predictive-model)

*  [Recuperando uma lista de todos os modelos atualmente implementados](#retrieving-a-list-of-all-currently-deployed-models)

*  [Fazendo download de uma cópia de um arquivo de modelo implementado específico](#downloading-a-copy-of-a-specific-deployed-model-file)

*  [Excluindo um modelo preditivo implementado](#deleting-a-deployed-predictive-model)

## Implementando ou atualizando um modelo preditivo

PUT http://{PA Bluemix load balancer
URL}/pm/v1/model/{contextId}?accesskey={access_key for this bound
application}

Use esta chamada API para fazer upload de um arquivo que contenha a ramificação de pontuação
desenvolvida pelo IBM SPSS Modeler que você gostaria de implementar.
Isso é disponibilizado para dados de armazenamento em seus aplicativos. Cada
arquivo de modelo recebe um ID de contexto como um alias conveniente a ser usado para
fazer referência ao modelo implementado em chamadas de serviço subsequentes. Se existir
um modelo para um ID de contexto, ele será substituído por essa chamada PUT como
um meio de atualizar as análises preditivas em uso por seus
aplicativos.

Exemplo de solicitação:

```
    Content-Type: multipart/form-data
    Parameters:
        Form parameters:
            model_file: the model file to upload
        Path parameters:
            contextId: the unique identifier to assign to your model or a reference to the deployed model to refresh
        Query Parameters:
            accesskey: access_key from env.VCAP_SERVICES
```
{: codeblock}

Resposta quando a implementação for bem-sucedida:

```
    Content-Type: application/json
    Status code: 200
    body:
        {
           "flag":true, 
           "message":"detailed information"  
         }
```
{: codeblock}

Resposta quando a implementação falhar:

```
    Content-Type: application/json
    Status code: 200
    body:
        {
           "flag":false, 
           "message":"reason"
        }
```
{: codeblock}

## Recuperando uma lista de todos os modelos atualmente implementados

GET http://{PA Bluemix load balancer
URL}/pm/v1/model?accesskey={access_key for this bound
application}

Recupere um resumo de todos os modelos que estão atualmente implementados nessa instância de serviço.

Exemplo de solicitação:

```
    Content-Type: */*
    Parameters:
        Query Parameters:
            accesskey: access_key from env.VCAP_SERVICES
```
{: codeblock}

Resposta quando a solicitação para um resumo de modelo implementado for bem-sucedida:

```
    Content-Type: application/json
    Status code: 200
    body: an empty array if no models have been deployed or a summary of the deployed models...
        [
            {
                "stream":"neural_net1.str",
                "id":"1"
            },
            {
                "stream":"neural_net2.str",
                "id":"2"
            },
            ...
        ]
```
{: codeblock}

Resposta quando a solicitação para um resumo de modelo implementado falhar:

```
    Content-Type: application/json
    Status code: 200
    body:
        {
           "flag":false, 
           "message":"reason"  
         }
```
{: codeblock}

## Fazendo download de uma cópia de um arquivo de modelo implementado específico

GET http://{PA Bluemix load balancer
URL}/pm/v1/model/{contextId}?accesskey={access_key for this bound
application}

Use esta chamada API para fazer download de uma cópia de um arquivo de modelo implementado específico.

Exemplo de solicitação:

```
    Content-Type: */*
    Parameters:
        Path parameters:
            contextId: the identifier of deployed predictive model you want to download
        Query Parameters:
            accesskey: access_key from env.VCAP_SERVICES
```
{: codeblock}

Resposta quando a solicitação de download for bem-sucedida:

```
    Content-Type: application/octet-stream
    Status code: 200
    Header:
        "Content-Disposition":"attachment; filename=model_file_name.str"
```
{: codeblock}

Resposta quando a solicitação de download falhar:

```
    Content-Type: application/json
    Status code: 200
    body:
        {
           "flag":false, 
           "message":String // failure reason 
        }
```
{: codeblock}

## Excluindo um modelo preditivo implementado

DELETE http://{service
instance}/pm/v1/model/{contextId}?accesskey={access_key for this
bound application}

Use essa chamada API para excluir o modelo preditivo da instância de serviço de
Machine Learning. Após essa
chamada, o modelo preditivo não ficará mais disponível para download ou dados de pontuação em seus aplicativos.

Exemplo de solicitação:

```
    Content-Type: */*
    Parameters:
        Path parameters:
            contextId: the identifier of deployed model you want to un-deploy and delete
        Query Parameters:
            accesskey: access_key from env.VCAP_SERVICES
```
{: codeblock}

Resposta quando a remoção de implementação for bem-sucedida:

```
    Content-Type: application/json
    Status code: 200
    body:
        {
           "flag":true, 
           "message":"detailed information"  
         }
```
{: codeblock}

Resposta quando a remoção de implementação falhar:

```
    Content-Type: application/json
    Status code: 200
    body:
        {
           "flag":false, 
           "message":"reason"
        }
```
{: codeblock}
