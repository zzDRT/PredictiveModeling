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

# Use o serviço Machine Learning com os modelos Spark e Python


Observe as informações importantes a seguir sobre o serviço
Machine Learning:

*  Estruturas suportadas do Machine Learning:

  *  Spark 2.0 MLlib
  *  Python 3 com scikit-learn 0.17

*  Modelos não supervisionados não são suportados.

*  A implementação em lote e de fluxo para modelos Python scikit-learn não é suportada neste estágio

*  O suporte em lote e de fluxo para modelos Spark está em Beta. Se você estiver interessado em participar, inclua
você mesmo na lista de espera! Para obter mais informações, veja: [https://www.ibm.biz/mlwaitlist](https://www.ibm.biz/mlwaitlist).

É possível fazer download do código de amostra Node.js para tentar o serviço
Machine Learning. Conclua as etapas a seguir para criar seu
aplicativo Bluemix e ligar o serviço Machine Learning.
Esses
exemplos usam o Node.js porque é um tempo de execução popular. Outros podem ser usados como
iOS, Ruby, Perl ou Java.

Observe que também é possível executar as etapas 1-3 usando a Interface gráfica do Bluemix em vez da ferramenta Cloud Foundry (cf).

1. Use o comando cf create-service para criar uma instância de
serviço:

   ```
   cf create-service pm-20 Free {local naming}
   ```
{: codeblock}

   Por exemplo:

   ```
   cf create-service pm-20 Free my_wml_free
   ```
{: codeblock}

   Esse comando cria uma instância de serviço de Machine Learning
com o plano grátis denominado ```my_wml_free``` no espaço do Bluemix.

2. Use o comando cf create-service-key para criar as credenciais
de serviço:

   ```
   cf create-service-key "{service instance name}" {vcap key name}
   ```
{: codeblock}

   Por exemplo:

   ```
   cf create-service-key "IBM Watson Machine Learning - my instance" Credentials-1
   ```
{: codeblock}

   Esse comando cria credenciais do serviço Machine Learning.



3. Use o comando cf bind-service para ligar a instância de serviço
   ```my_wml_free``` to your application.

   ```
   cf bind-service {AppName} my_wml_free
   ```
{: codeblock}

   Por exemplo:

   ```
   cf bind-service my_app1 my_wml_free
   ```
{: codeblock}

   Esse comando liga a instância de serviço de Machine Learning
my_wml_free ao aplicativo do Bluemix my_app1.



Credenciais do Machine Learning:

   Depois que você liga a instância de serviço de Machine Learning ao
aplicativo do Bluemix, as credenciais do Machine Learning são
incluídas na variável de ambiente VCAP_SERVICES:

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

   A variável de ambiente VCAP_SERVICES inclui as informações
a seguir:

   * plan - o plano do Machine Learning usado no fornecimento de serviço.

   * url - o endereço da instância de serviço de Machine Learning.

   * access_key - somente para fluxos do SPSS Modeler.

   * instance_id - identificador exclusivo da instância do Machine Learning.

   * username, password - autorização básica necessária para gerar um token de acesso para passar em todas as solicitações para essa instância de serviço. Por exemplo, é possível gerar um token de acesso usando um curl como este:

Exemplo de solicitação:

```
       curl --basic --user username:password https://ibm-watson-ml.mybluemix.net/v2/identity/token

       Exemplo de saída:

       {"token":"**********"}
```
{: codeblock}

   O código Node.js a seguir é um exemplo de como obter o
nome do usuário e a senha da variável de ambiente
VCAP_SERVICES:

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
