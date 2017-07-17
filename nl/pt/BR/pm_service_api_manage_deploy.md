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

# Implementando ou atualizando um modelo preditivo


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
