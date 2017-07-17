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

# Excluindo um modelo preditivo implementado


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
