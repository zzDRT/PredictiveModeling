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

# Implementando modelos on-line


**Nome do cenário**: predição de linha de produto.

**Descrição do cenário**: uma empresa que vende equipamento de outdoor
quer criar um modelo que prediga o interesse do cliente em sua
linha de produto. Um
cientista de dados preparou um modelo preditivo e o compartilha com você (o
desenvolvedor). Sua tarefa é implementar o modelo e gerar previsões de interesse do
cliente, fazendo solicitações de escore com relação ao modelo implementado.

## Usando o modelo de amostra

1. Acesse a guia Amostras do Painel IBM® Watson™ Machine
Learning.

2. Na seção Modelos de amostra, localize o quadro Predição de linha de produto
e clique no botão Incluir modelo (+).

Agora você verá o modelo de amostra Predição de linha de produto na
lista de modelos disponíveis na guia Modelos.

## Gerando o token de acesso

Gere um token de acesso usando o usuário e a senha disponíveis
na guia Credenciais de serviço da instância de serviço do IBM Watson Machine
Learning.

Exemplo de solicitação:

```
curl --basic --user username:password https://ibm-watson-ml.mybluemix.net/v3/identity/token
```
{: codeblock}

Exemplo de saída:

```
{"token":"**********"}
```
{: codeblock}

## Criando a implementação on-line

Use a chamada API a seguir para criar uma implementação
on-line do seu modelo preditivo. Isso é disponibilizado para dados de armazenamento em seus aplicativos. O terminal que é necessário para criar uma implementação on-line está disponível em seu Painel do WML-> Detalhes do modelo -> URL. Observe também que é possível localizar o valor "published_model_id"
na página Detalhes do modelo do Painel do IBM Watson Machine Learning
no campo ID e será possível obter seu "instance_id" nas credenciais VCAP da instância do Watson Machine Learning.

Exemplo de solicitação:

```
curl -X POST --header "Content-Type: application/json" --header "Accept: application/json" --header "Authorization: token" -d '{"name": "Product Line Prediction", "description": "Product Line Prediction Deployment", "type": "online"}' 'https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/{published_model_id}/deployments'
```
{: codeblock}

Exemplo de saída:

```
{  
   "metadata":{  
      "guid":"b97072ec-3ef8-4705-a1e7-c264e270e49a",
      "url":"https://ibm-watson-ml.mybluemix.net/v3/wml_instances/21208fa4-f5b8-4fb7-b162-878e455e4be2/published_models/1ab15cdb-4f9e-4d35-8077-c0f6fff10196/deployments/b97072ec-3ef8-4705-a1e7-c264e270e49a",
      "created_at":"2017-06-27T13:47:49.534Z",
      "modified_at":"2017-06-27T13:47:58.347Z"
   },
   "entity":{  
      "runtime_environment":"spark-2.0",
      "name":"Product Line Prediction TMNL",
      "instance_href":"/v2/scoring/online/jobs/b97072ec-3ef8-4705-a1e7-c264e270e49a",
      "scoring_url":"https://ibm-watson-ml.mybluemix.net/v3/wml_instances/21208fa4-f5b8-4fb7-b162-878e455e4be2/published_models/1ab15cdb-4f9e-4d35-8077-c0f6fff10196/deployments/b97072ec-3ef8-4705-a1e7-c264e270e49a/online",
      "description":"Product Line Prediction Deployment",
      "published_model":{  
         "author":{  
            "name":"IBM",
            "email":""
         },
         "name":"Product Line Prediction",
         "url":"https://ibm-watson-ml.mybluemix.net/v3/wml_instances/21208fa4-f5b8-4fb7-b162-878e455e4be2/published_models/1ab15cdb-4f9e-4d35-8077-c0f6fff10196",
         "guid":"1ab15cdb-4f9e-4d35-8077-c0f6fff10196",
         "description":"Predicts clients' interests in terms of sport product lines for chain stores in Europe.",
         "created_at":"2017-06-27T11:54:24.170Z"
      },
      "model_type":"sparkml-model-2.0",
      "status":"INITIALIZING",
      "type":"online",
      "deployed_version":{  
         "url":"https://ibm-watson-ml.mybluemix.net/v2/artifacts/models/1ab15cdb-4f9e-4d35-8077-c0f6fff10196/versions/3ab34346-f195-4ee4-b378-1204f674a725",
         "guid":"3ab34346-f195-4ee4-b378-1204f674a725",
         "created_at":"2017-06-27T11:54:07.507Z"
      }
   }
}
```
{: codeblock}

## Obtendo detalhes da implementação

É possível verificar o status, o endereço do terminal de pontuação (scoringHref)
e os parâmetros relacionados ao modelo implementado.

Exemplo de solicitação:

```
curl -X GET --header "Content-Type: application/json" --header "Accept: application/json" --header "Authorization: $token" 'https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}/published_models/{published_model_id}/deployments/{deployment_id}'
```
{: codeblock}

Exemplo de saída:

```
{  
   "metadata":{  
      "guid":"b97072ec-3ef8-4705-a1e7-c264e270e49a",
      "url":"https://ibm-watson-ml.mybluemix.net/v3/wml_instances/21208fa4-f5b8-4fb7-b162-878e455e4be2/published_models/1ab15cdb-4f9e-4d35-8077-c0f6fff10196/deployments/b97072ec-3ef8-4705-a1e7-c264e270e49a",
      "created_at":"2017-06-27T13:47:49.534Z",
      "modified_at":"2017-06-27T13:47:58.347Z"
   },
   "entity":{  
      "runtime_environment":"spark-2.0",
      "name":"Product Line Prediction TMNL",
      "instance_href":"/v2/scoring/online/jobs/b97072ec-3ef8-4705-a1e7-c264e270e49a",
      "scoring_url":"https://ibm-watson-ml.mybluemix.net/v3/wml_instances/21208fa4-f5b8-4fb7-b162-878e455e4be2/published_models/1ab15cdb-4f9e-4d35-8077-c0f6fff10196/deployments/b97072ec-3ef8-4705-a1e7-c264e270e49a/online",
      "description":"Product Line Prediction Deployment",
      "published_model":{  
         "author":{  
            "name":"IBM",
            "email":""
         },
         "name":"Product Line Prediction",
         "url":"https://ibm-watson-ml.mybluemix.net/v3/wml_instances/21208fa4-f5b8-4fb7-b162-878e455e4be2/published_models/1ab15cdb-4f9e-4d35-8077-c0f6fff10196",
         "guid":"1ab15cdb-4f9e-4d35-8077-c0f6fff10196",
         "description":"Predicts clients' interests in terms of sport product lines for chain stores in Europe.",
         "created_at":"2017-06-27T11:54:24.170Z"
      },
      "model_type":"sparkml-model-2.0",
      "status":"ACTIVE",
      "type":"online",
      "deployed_version":{  
         "url":"https://ibm-watson-ml.mybluemix.net/v2/artifacts/models/1ab15cdb-4f9e-4d35-8077-c0f6fff10196/versions/3ab34346-f195-4ee4-b378-1204f674a725",
         "guid":"3ab34346-f195-4ee4-b378-1204f674a725",
         "created_at":"2017-06-27T11:54:07.507Z"
      }
   }
}
```
{: codeblock}

## Fazendo solicitações de escore

Desde a criação do terminal de pontuação (scoringHref),
é possível agora gerar predições fazendo solicitações de escore. Nesse cenário, os registros do cliente são passados
para o modelo preditivo e uma previsão de produto de esporte é retornada.

Cabeçalho de registro de amostra:

```
GENDER,AGE,MARITAL_STATUS,PROFESSION
```
{: codeblock}

Registro de amostra:

```
M,27,Single,Professional
F,56,Unspecified,Hospitality
M,45,Married,Retired
```
{: codeblock}

Exemplo de solicitação:

```
curl -X POST --header 'Content-Type: application/json' --header 'Accept: application/json' --header 'Authorization: token' -d '{"fields": ["GENDER","AGE","MARITAL_STATUS","PROFESSION"],"values": [["M",23,"Single","Student"],["M",55,"Single","Executive"]]}' 'https://ibm-watson-ml.mybluemix.net/v3/wml_instances/{instance_id}//published_models/{published_model_id}/deployments/{deployment_id}/online'
```
{: codeblock}

Exemplo de saída:

```
{    "fields": ["GENDER",
      "AGE",
      "MARITAL_STATUS",
      "PROFESSION",
      "PROFESSION_IX",
      "GENDER_IX",
      "MARITAL_STATUS_IX",
      "features",
      "rawPrediction",
      "probability",
      "prediction",
      "predictedLabel"
   ],
   "records":[
      [
         "M",
         23,
         "Single",
         "Student",
         6.0,
         0.0,
         1.0,
         [
            0.0,
            23.0,
            1.0,
            6.0
         ],
         [
            5.600716147152702,
            6.482458372136143,
            6.048004170730676,
            0.20929155492307386,
            1.6595297550574055
         ],
         [
            0.2800358073576351,
            0.32412291860680714,
            0.3024002085365338,
            0.010464577746153694,
            0.08297648775287028
         ],
         1.0,
         "Personal Accessories"
      ],
      [
         "M",
         55,
         "Single",
         "Executive",
         3.0,
         0.0,
         1.0,
         [
            0.0,
            55.0,
            1.0,
            3.0
         ],
         [
            6.227653744886563,
            4.325198862164969,
            8.031953760146816,
            1.2319759269281225,
            0.1832177058735289
         ],
         [
            0.3113826872443282,
            0.2162599431082485,
            0.40159768800734086,
            0.06159879634640614,
            0.009160885293676447
         ],
         2.0,
         "Mountaineering Equipment"
      ]
   ]
}
```
{: codeblock}

Podemos ver, por exemplo, que um executivo de 55 anos está
interessado em equipamento de montanhismo, enquanto um estudante de
23 anos está interessado em acessórios pessoais.
