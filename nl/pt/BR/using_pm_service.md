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

# Usando o serviço Machine Learning com modelos IBM SPSS Modeler

Os métodos de modelagem disponíveis na paleta de modelagem SPSS Modeler permitem derivar novas informações de seus dados e desenvolver modelos preditivos. Cada método possui certas forças e é mais
adequado para certos tipos de problemas. 
{: .shortdesc}

Para obter detalhes sobre o SPSS Modeler e a modelagem de algoritmos que ele fornece, consulte o [IBM Knowledge Center](https://www.ibm.com/support/knowledgecenter/SS3RA7).

Depois que os requisitos de entrada e saída de seu aplicativo Bluemix e do design de ramificação de pontuação SPSS Modeler
são implementados, seu Data Analyst pode mudar qualquer aspecto interno da ramificação de pontuação. O
Data Analyst pode até alterar o(s) algoritmo(s) de modelo usado(s) em uma operação de atualização,
assegurando sua capacidade de realizar ajuste fino de suas análises preditivas
sem precisar regravar seus aplicativos.

Observe as informações importantes a seguir sobre o serviço
Machine Learning quando usado com modelos criados no SPSS Modeler:

*  Quando uma ramificação de escoragem é preparada para uso em escoragem em tempo
real, os dados de entrada vindos da solicitação de escore devem substituir o nó de origem
projetado na ramificação de escoragem e a saída da análise preditiva resultante deve
fluir de volta para o fluxo de resposta (substituindo efetivamente o nó terminal no
design de ramificação de escoragem).

*  Conforme a ramificação de escoragem é preparada para execução em tempo real no
Bluemix, ela não pode requerer uma conexão com um serviço externo. Por exemplo, um design
de ramificação de escoragem do IBM Analytical Decision Management não pode conter
referências a regras ou modelos armazenados em um repositório do IBM SPSS Collaboration
and Deployment Services.

*  A execução de uma ramificação de escoragem para escoragem em tempo real no Bluemix
não pode requerer um serviço externo. Por exemplo, não é possível implementar e escorar
com relação a algoritmos de modelo que exijam um armazenamento de dados IBM SPSS
Analytic Server e Apache Hadoop em tempo real.

*  O Machine Learning suporta o script Python integrado do Modeler.
Há algumas restrições em razão do método usado para
processar fluxos antes que eles sejam executados no Machine Learning.
Normalmente, se
um usuário optar pelo controle da execução do fluxo, eles referenciarão o nó terminal da
ramificação.
   Para o Machine Learning, quando processamos o fluxo, identificamos
os nós do JSON que serão substituídos e, em seguida, executamos a
substituição antes que o fluxo seja executado. Isso faz com que o
fluxo falhe no script porque os nós de entrada e exportação referenciados não existem mais. A
solução é usar o ID de outro nó para identificar exclusivamente a ramificação durante a
execução. Isso assegura que o fluxo seja executado conforme definido no script Python
integrado.

Para obter mais detalhes sobre o suporte atual para modelos preditivos
treinados pelo IBM SPSS Analytic Server, veja a seção Analytic Server
do IBM Knowledge Center.

1. É possível fazer download do código de amostra Node.js para tentar o serviço
Machine Learning. Conclua as etapas a seguir para criar seu
aplicativo Bluemix e ligar o serviço Machine Learning.
Esses
exemplos usam o Node.js porque é um tempo de execução popular.
   Outros podem ser usados como
iOS, Ruby, Perl ou Java.

2. Use o comando cf create-service para criar uma instância de
serviço:

   ```
   cf create-service pm-20 Free {local naming}
   ```

   Por exemplo:

   ```
   cf create-service pm-20 Free my_pm_free
   ```

   Esse comando cria uma instância de serviço de Machine Learning
com o plano grátis denominado my_pm_free no espaço do Bluemix.

3. Use o comando `cf create-service-key` para criar credenciais de serviço:

   ```cf create-service-key "{service instance name}" {vcap key name}```

   Por exemplo:

   ```cf create-service-key "IBM Watson Machine Learning - my instance" Credentials-1```

Esse comando cria credenciais do serviço Machine Learning.

4. Use o comando cf bind-service para ligar a instância de serviço
my_pm_free ao aplicativo.

   ```cf bind-service AppName my_pm_service```

   Por exemplo:

   ```cf bind-service my_app1 my_pm_free```

Esse comando liga a instância de serviço de Machine Learning
`my_pm_free` ao aplicativo Bluemix my_app1.

5. Credenciais do Machine Learning:

   Depois que você liga a instância de serviço de Machine Learning ao
aplicativo Bluemix, as credenciais do Machine Learning são
incluídas na variável de ambiente `VCAP_SERVICES`:

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

   A variável de ambiente `VCAP_SERVICES` inclui as informações a seguir:

   <dl>
   
   <dt>plano</dt>
   <dd>O plano do Machine Learning usado no fornecimento de serviço.</dd>

   <dt>url</dt>
   <dd>O endereço da instância de serviço de Machine Learning.</dd>

   <dt>access_key</dt>
   <dd>O parâmetro de consulta accessKey a ser passado em todas as solicitações
para essa instância de serviço.</dd>

   </dl>
            
Por exemplo:             

```
Get https://ibm-watson-ml.mybluemix.net/pm/v1/model/sales_model2?accesskey=XXXXXXXXXXXXX 
```

   O código de exemplo Node.js que mostra como obter a accessKey
da variável de ambiente `VCAP_SERVICES`:

```
   if (process.env.VCAP_SERVICES) {
        var env = JSON.parse(process.env.VCAP_SERVICES);
        var credentials = env['pm-20'][0].credentials;
        var accessKey = credentials.access_key;
    }
```
