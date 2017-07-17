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

# Desenvolvendo aplicativos que alavancam modelos SPSS implementados


*  Pontuação com um modelo preditivo implementado

*  Recuperando metadados para um modelo preditivo implementado

*  Recuperando o resumo Web Application Description Language (WADL) deste serviço

PONTUANDO COM UM MODELO PREDITIVO IMPLEMENTADO

POST http://{PA Bluemix load balancer
URL}/pm/v1/score/{contextId}?accesskey={access_key for this bound
application}

Use esta chamada API para postar os dados de entrada a serem usados pelo modelo implementado
para gerar e retornar as análises preditivas nos resultados da pontuação.

Exemplo de solicitação:

```
    Content-Type: application/json;charset=UTF-8
    Parameters:
        Path parameters:
            contextId: the identifier of the deployed model to use to process this score request
        Query Parameters:
            accesskey: access_key from env.VCAP_SERVICES
        Body: the input data, json string, eg.
            {
                "tablename":"DRUG1n.sav", 
                "header":["Age", "Sex", "BP", "Cholesterol", "Na", "K", "Drug"], 
                "data":[[43.0, "M", "LOW", "NORMAL", 0.526102, 0.027164, "drugY"]]
            }   
```
{: codeblock}

Exemplo de uma resposta bem-sucedida à solicitação anterior:

```
    Content-Type: application/json;charset=UTF-8
    Status code: 200
    body: the score result, a json array, eg.
        [
            {
                "header":["Age","Sex","BP","Cholesterol","Na""K","Drug","$N-Drug","$NC-Drug"], 
                "data":[[23.0,"M","NORMAL","NORMAL",0.78452,0.055959,"drugX","drugX",0.9892564426956728]]
            }
        ]
```
{: codeblock}

Resposta quando a solicitação de escore falhar:

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

RECUPERANDO METADADOS PARA UM MODELO PREDITIVO IMPLEMENTADO

GET http://{service
instance}/pm/v1/metadata/{contextId}?accesskey={access_key for
this bound application}&metadatatype=score

Use essa chamada API para recuperar os metadados para a ramificação de pontuação
de um fluxo IBM SPSS Modeler implementado. Não forneça um corpo de solicitação com esse método.

Exemplo de solicitação:

```
    Content-Type: application/json;charset=UTF-8
    Parameters:
        Path parameters:
            contextId: the identifier of the deployed model to use to process this metadata retrieval request
        Query Parameters:
            accesskey: access_key from env.VCAP_SERVICES
```
{: codeblock}

Exemplo de uma resposta bem-sucedida à solicitação anterior:

```
    Content-Type: application/json;charset=UTF-8
    Status code: 200
    body: the metadata (formatted for readability in this document) on the scoring branch eg.
    [
      {
        flag: true
        message: 
          "Model Score Input Metadata: 
          <?xml version="1.0" encoding="UTF-8" standalone="yes"?>
          <metadata xmlns="http://spss.ibm.com/meta/internal">
          <table xsi:type="nodeImpl" tag="id574QKQ8NL6E" name="baskrule_input.csv" 
              xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
            <field measurementLevel="CATEGORICAL" storageType="STRING" name="gender"/>
            <field measurementLevel="CONTINUOUS" storageType="LONG" name="income"/>
          </table>
          </metadata> 
          Model Score Output Metadata:
          <?xml version="1.0" encoding="UTF-8" standalone="yes"?>
          <metadata xmlns="http://spss.ibm.com/meta/internal">
          <table xsi:type="nodeImpl" tag="id32CPAJBGJFG" name="Flat File" 
              xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
            <field measurementLevel="CATEGORICAL" storageType="STRING" name="gender"/>
            <field measurementLevel="CONTINUOUS" storageType="LONG" name="income"/>
            <field measurementLevel="FLAG" storageType="STRING" name="$C-beer_beans_pizza"/>
            <field measurementLevel="CONTINUOUS" storageType="DOUBLE" name="$CC-beer_beans_pizza"/>
          </table>
          </metadata>"
      }
    ]
```
{: codeblock}

Resposta quando uma solicitação de escore falhar:

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

RECUPERANDO O RESUMO DA WEB APPLICATION DESCRIPTION LANGUAGE (WADL)
DESTE SERVIÇO

OPTIONS http://{PA Bluemix load balancer URL}/pm/v1/wadl

Use essa chamada API para recuperar o WADL para esse serviço.

Exemplo de solicitação:

```
    Content-Type: */*
```
{: codeblock}

Resposta quando a solicitação de WADL for bem-sucedida:

```
    Content-Type: application/vnd.sun.wadl+xml
    Status code: 200
    body: the WADL XML ,eg.
        <?xml version="1.0" encoding="UTF-8" standalone="yes" ?>
            <application>
                <resources base="http://{PA Bluemix load balancer URL}/pm/v1/">
                    <resource path="score">
                        <doc title="Score using the model with given context ID" />
                        <resource path="{id}">
                            <param style="template" name="id" />
                            <method name="POST">
                                <request>
                                    <param style="query" name="accesskey" />
                                    <representation mediaType="application/json;charset=UTF-8" />
                                </request>
                                <response>
                                    <representation mediaType="application/json;charset=UTF-8" />
                                </response>
                            </method>
                        </resource>
                     </resource>
                     ...

             </resources>
        </application>
```
{: codeblock}

Resposta quando a solicitação de WADL falhar:

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
