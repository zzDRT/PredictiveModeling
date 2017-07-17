---

copyright:
  years: 2016, 2017
lastupdated: "2017-06-21"

---
<!-- Copyright info and last updated date at top of file: REQUIRED
    The copyright and lastupdated info is YAML content that must occur at the top of the MD file, before attributes are listed.
    It must be --- surrounded by 3 dashes ---
    The value "years" can contain just one year or a two years separated by a comma. (years: 2014, 2016)
    The value "lastupdated" must be followed by a machine date in quotes in the following format: "YYYY-MM-DD"
    The value for "years" must be indented 2 spaces under "copyright", followed by "lastupdated" which should start on its own non-indented line.

-->

<!-- Common attributes used in the template are defined as follows: -->
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# API de tarefa em lote do serviço Machine Learning para modelos IBM SPSS Modeler


*  [Excluindo tarefas](#deleting-jobs)

*  [Verificando o status de uma tarefa](#checking-the-status-of-a-job)

*  [Reenviar uma tarefa](#resubmit-a-job)

*  [Enviar uma tarefa em um arquivo de fluxo do Modeler transferido por upload](#submit-a-job-against-an-uploaded-modeler-stream-file)

*  [Fazer upload de um arquivo de fluxo para usar em suas tarefas](#upload-a-stream-file-to-use-in-your-jobs)

*  [Tipos de Job](#job-types)

*  [Status do Trabalho](#job-status)

*  [Detalhes da API de tarefa](#job-api-details)

*  [JSON de definição de tarefa](#job-definition-json)

*  [Detalhes da API de tarefa em lote](#batch-job-api-details)

A API de tarefa em lote do serviço Machine Learning suporta as
tarefas de longa execução relacionadas a treinamento de modelo, avaliação de modelo
e escoragem de lote. Essa API gerencia dois tipos de ativos: os arquivos de fluxo do SPSS
Modeler usados nas tarefas em lote e as definições de tarefa submetidas. Para cada
arquivo de fluxo do Modeler transferido por upload, pode haver muitas tarefas de
muitos tipos submetidas. Se uma tarefa tiver o potencial para atualizar o conteúdo do
arquivo de fluxo do Modeler, o arquivo modificado será incluído nos anexos do resultado
da tarefa. Em um tipo de tarefa de avaliação de modelo, todos os arquivos de avaliação
gerados estarão nos anexos do resultado da tarefa.

Para obter um exemplo de adoção de uma tarefa em lote, consulte as
anotações a seguir: [Do fluxo de SPSS à escoragem de lote com o
Python](https://apsportal.ibm.com/analytics/notebooks/9d7ce38e-9417-4c76-a6b9-5bc8cf40938a/view?access_token=5ca87e3007804e5b2bbbce77c20e99ac3c164d66f2d28dfffb54aa365caaef37).

## Excluindo tarefas

É possível excluir tarefas, o que a cancelará se ela estiver em
execução no momento. Chame DELETE no ID da tarefa (e é possível cancelar mais de
uma tarefa por vez):

DELETE https://{PA Bluemix load balancer URL}/pm/v1/jobs/{job ID
1, job ID 2,...}?accesskey=xxxxxxxxxx

O retorno indicará quantas tarefas referenciadas por ID na solicitação foram
excluídas. Se esse número não corresponder à lista passada na solicitação, deve-se
inspecionar o status das tarefas individuais.

## Verificando o status de uma tarefa

É possível OBTER o status de seu ID da tarefa a qualquer momento:

GET https://{PA Bluemix load balancer URL}/pm/v1/jobs/{job
ID}/status?accesskey=xxxxxxxxxx

O JSON retornado indica jobstatus e, se a tarefa tiver sido concluída
com sucesso, um dataUrl que poderá ser usado para obter todo o conteúdo de
arquivo gerado.

## Reenviar uma tarefa

Chame PUT para o <ID da tarefa>. Não deve
estar em um estado de execução:

PUT https://{PA Bluemix load balancer URL}/pm/v1/jobs/{job
ID}?accesskey=xxxxxxxxxx

com content_type "application/json" incluindo o JSON da definição de
tarefa nova ou atualizada no corpo da solicitação.

Essa chamada de fato criará uma nova tarefa se o ID referenciado não existir, com o
retorno de 201 em vez de 200 indicando qual desses ocorreu. Deve-se passar o JSON de
definição de tarefa a ser usado nessa execução.

## Enviar uma tarefa em um arquivo de fluxo do modelador transferido por upload

Chame PUT para sua definição de tarefa para colocá-la na fila de
execução:

PUT https://{PA Bluemix load balancer URL}/pm/v1/jobs/{job
ID}?accesskey=xxxxxxxxxx

com content_type "application/json" incluindo o JSON de definição de
tarefa no corpo da solicitação.

Essa solicitação retorna imediatamente, indicando sucesso se a definição de tarefa foi
colocada na fila de execução. Não há teste da capacidade de executar com sucesso o fluxo
do Modeler conforme configurado em sua definição de tarefa. A tarefa será executada por
um dos servidores de tarefa de Machine Learning na nuvem e será possível
monitorar seu status. Se estiver executando treinamento de modelo e indicando
que deseja uma atualização automática, a tarefa substituirá o arquivo de fluxo do Modeler
original em caso de execução bem-sucedida da tarefa.

Para obter mais informações sobre o JSON de definição de tarefa, veja [JSON
de definição de tarefa](#job-definition-json).

## Fazer upload de um arquivo de fluxo para usar em suas tarefas

Nota: o painel de Machine Learning é somente para escoragem em
tempo real. Não é possível usá-lo para executar tarefas (escoragem de
lote).

Chame PUT para tornar um arquivo de fluxo do Modeler acessível para tarefas:

PUT https://{PA Bluemix load balancer URL}/pm/v1/file/{file
ID}?accesskey=xxxxxxxxxx

com content_type "multipart/form-data" passando o arquivo na
solicitação.

O nome exclusivo usado (ID do arquivo na chamada PUT) é o que você
referenciará em uma chamada DELETE para a API de arquivo, bem como a
referência de modelo em suas definições de tarefa. Observe que o namespace de arquivos
usados em suas tarefas está totalmente sob seu controle – portanto, o PUT de um
arquivo com o mesmo <ID de arquivo> substitui implicitamente a cópia
atual retida.

É possível OBTER uma lista de todos os arquivos transferidos por upload para suas tarefas
omitindo o <ID do arquivo> ou recuperar um arquivo específico por GET com
seu ID. Também é possível EXCLUIR um arquivo transferido por upload (isso, naturalmente,
causará erros em qualquer execução de tarefa pendente que referencie
o arquivo).

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

## Tipos de Job

Treinamento: esse tipo de tarefa indica que todos os nós terminais
"construtores de modelo" devem ser executados no fluxo do Modeler. Após a
conclusão bem-sucedida da tarefa, um fluxo atualizado do Modeler com nuggets do modelo
recém-treinado estará nos resultados da tarefa que podem ser recuperados. Se o arquivo de
fluxo do Modeler tiver links dos nós de construção de modelo para os nuggets do modelo
treinado nas ramificações de avaliação e escoragem, isso causará uma atualização desses
nós.

Avaliação: esse tipo de tarefa aciona a execução de todos os nós
terminais "construtores de documento" (principalmente nas guias Gráficos e Saída
no cliente do Modeler) que geram conteúdo de arquivo de relatório estático que
pode ser novamente passado ao responsável pela chamada. A ramificação de escoragem não é considerada como parte desse
tipo de tarefa.

Atualização automática: uma versão do tipo de tarefa TRAINING em que, na
conclusão bem-sucedida da tarefa, o arquivo de fluxo original do Modeler
na lista de arquivos em lote será atualizado. A decisão de
avaliação e explícita sobre um evento de atualização dos fluxos do Modeler implementados
para escoragem em tempo real é assumida e não coberta na atualização automática neste
momento.

Escoragem de lote: a execução do nó terminal que você aplicou à
opção Usar como ramificação de escoragem, indicando que essa é a ramificação
de escoragem neste design de fluxo do Modeler. A definição de tarefa deve especificar
os detalhes de Exportação, bem como os de Origem.

Fluxo de execução: a execução é semelhante a clicar no botão verde
"executar" no Modeler com a opção Executar este script selecionada na
guia Execução das propriedades do fluxo. São
necessárias coberturas de uso para execução do script do treinamento de modelo ou outros
tipos de tarefa. Todo o controle dinâmico do script deve ser manipulado pelos parâmetros
do fluxo, com os valores de parâmetro passados na definição de tarefa.

## Status do Trabalho

Pendente: a definição de tarefa foi enviada, mas ainda não foi
reivindicada por um servidor de tarefa para execução.

Em execução: a definição de tarefa foi reivindicada por um servidor de tarefa e
está em execução.

Cancelando: a tarefa está no processo de ser cancelada.

Cancelada: a tarefa foi cancelada.

Com falha: a tarefa falhou. Os detalhes sobre a causa da falha são
retornados em GET no status da tarefa.

Sucesso: a tarefa foi executada com sucesso. Qualquer resultado comunicado para
esse evento é retornado no JSON do GET no status da tarefa.

## Detalhes da API de tarefa

POST /v1/jobs/{id}

Descrição: envia uma definição de tarefa para execução. Um
erro resultará se o <job ID> já existir.

Tipos de conteúdo:

```
@Consumes({“application/json”})
@Produces({“application/json”})
```
{: codeblock}

Parameters:

Chave de acesso retornada como credenciais na provisão ou ligação:

```
@QueryParam("accesskey")
```
{: codeblock}

ID da tarefa especificado pelo usuário. Deve ser exclusivo para uma instância de serviço de
Machine Learning:

```
@PathParam("id")
```
{: codeblock}

Definição de tarefa JSON (veja JSON de definição de tarefa):

```
@BodyParam
```
{: codeblock}

Respostas:

Sucesso. A tarefa é enviada para execução e retorna o JSON que descreve a tarefa
enviada.

```
@ApiResponse(code = 200)
```
{: codeblock}

Erro de tarefa já existente. Uma tarefa com esse ID já foi enviada. A mensagem
retornada descreve esse erro.

```
@ApiResponse(code = 409)
```
{: codeblock}

Outro erro. JSON de exceção retornada

```
@ApiResponse(code = 500)
```
{: codeblock}

PUT /v1/jobs/{id}

Descrição: criar ou atualizar uma tarefa. Se uma tarefa
com esse ID não existir, será criada; caso contrário, será atualizada (o que, efetivamente,
a reenvia para execução).

Tipos de conteúdo:

```
@Consumes({“application/json”})
@Produces({“application/json”})
```
{: codeblock}

Parameters:

Chave de acesso retornada como credenciais na provisão ou ligação:

```
@QueryParam("accesskey")
```
{: codeblock}

ID da tarefa especificado pelo usuário:

```
@PathParam("id")
```
{: codeblock}

Definição de tarefa JSON (veja JSON de definição de tarefa):

```
@BodyParam
```
{: codeblock}

Respostas:

Sucesso. A tarefa foi atualizada e enviada para execução. Retorna o JSON que descreve
a tarefa enviada.

```
@ApiResponse(code = 200)
```
{: codeblock}

Sucesso. A tarefa foi criada e enviada para execução. Retorna o JSON que descreve
a tarefa enviada.

```
@ApiResponse(code = 201)
```
{: codeblock}

Outro erro. JSON de exceção retornada.

```
@ApiResponse(code = 500)
```
{: codeblock}

GET /v1/jobs

Descrição: retorna uma lista de todas as tarefas definidas nesta instância de
serviço de Machine Learning.

Tipos de conteúdo:

```
@Produces({“application/json”})
```
{: codeblock}

Parameters:

Chave de acesso retornada como credenciais na provisão ou ligação:

```
@QueryParam("accesskey")
```
{: codeblock}

Respostas:

Sucesso. Retorna a matriz JSON de todas as definições de tarefa:

```
@ApiResponse(code = 200)
```
{: codeblock}

Outro erro. JSON de exceção retornada:

```
@ApiResponse(code = 500)
```
{: codeblock}

GET /v1/jobs/{id}

Descrição: solicitar o retorno de uma definição de tarefa específica.

Tipos de conteúdo:

```
@Produces({“application/json”})
```
{: codeblock}

Parameters:

Chave de acesso retornada como credenciais na provisão ou ligação:

```
@QueryParam("accesskey")
```
{: codeblock}

ID da tarefa enviada:

```
@PathParam("id")
```
{: codeblock}

Respostas:

Sucesso. Retorna o JSON que descreve a tarefa
referenciada:

```
@ApiResponse(code = 200)
```
{: codeblock}

Erro de tarefa não localizada. Detalhes na mensagem
retornada:

```
@ApiResponse(code = 404)
```
{: codeblock}

Outro erro. JSON de exceção retornada:

```
@ApiResponse(code = 500)
```
{: codeblock}

GET /v1/jobs/{id}/status

Descrição: recuperar o status de uma tarefa específica.

Tipos de conteúdo:

```
@Produces({“application/json”})
```
{: codeblock}

Parameters:

Chave de acesso retornada como credenciais na provisão ou ligação:

```
@QueryParam("accesskey")
```
{: codeblock}

ID da tarefa enviada:

```
@PathParam("id")
```
{: codeblock}

Respostas:

Sucesso. Retorna o JSON que descreve o status da
tarefa:

```
@ApiResponse(code = 200)
```
{: codeblock}

Erro de tarefa não localizada, detalhes na mensagem
retornada:

```
@ApiResponse(code = 404)
```
{: codeblock}

Outro erro. JSON de exceção retornada:

```
@ApiResponse(code = 500)
```
{: codeblock}

DELETE /v1/jobs/{ids}

Descrição: excluir uma ou mais tarefas. Cancelará a
tarefa se ela estiver em execução no momento.

Tipos de conteúdo:

```
@Produces({“application/json”})
```
{: codeblock}

Parameters:

Chave de acesso retornada como credenciais na provisão ou ligação:

```
@QueryParam("accesskey")
```
{: codeblock}

ID da tarefa enviada ou lista de IDs de tarefa com valores de ID divididos por
“,”

```
@PathParam("id" or “id,id,id”)
```
{: codeblock}

Respostas:

Sucesso. Retorna o número de tarefas excluídas:

```
@ApiResponse(code = 200)
```
{: codeblock}

Outro erro. JSON de exceção retornada:

```
@ApiResponse(code = 500)
```
{: codeblock}

## JSON de definição de tarefa

O JSON de definição de tarefa contém as seções gerais a seguir:

Referência de tipo de tarefa e modelo preditivo

Tipos de ação:

*  TRAINING – Executa o nó de construção de modelo treinando ou
atualizando os nuggets do modelo. O fluxo do Modeler atualizado é recuperado nos
resultados da tarefa.

*  EVALUATION – Executa os nós de construtor e análise de documento
avaliando o modelo treinado.

*  AUTO_REFRESH – Executa TRAINING e atualiza o conteúdo do arquivo no
espaço de upload do arquivo em lote.

*  BATCH_SCORE – Executa a ramificação de escoragem e exporta os
dados resultantes conforme direcionado pela definição de tarefa.

*  RUN_STREAM – Executa o fluxo do Modeler conforme indicado nas
propriedades do fluxo; muitas vezes usado quando a execução do script é
necessária.

Modelo: ID conforme especificado na ação de upload de arquivo para a referência de
tarefa em lote. Para obter mais informações, veja Fazendo upload de um arquivo de fluxo
para usar em suas tarefas. Observe que /pm/v1/file é usado.

```
"action": "TRAINING",
"model": {
     "id": "str2" 
     "name":"stream1.str" 
},
```
{: codeblock}

Observe que o ID deve ser o mesmo ID do arquivo usado na API
de PUT. O nome não é necessário, mas para treinamento e atualização automática
de modelo, o resultado da tarefa será salvo usando o nome definido
aqui. Se o nome não for definido, o serviço Machine Learning
gerará o resultado de acordo com as regras de nomenclatura predefinidas.

Definições do job

Todas as configurações requeridas para executar esta
tarefa.

```
"setting": {
     … Export settings
     … Import settings
     … Parameter value settings
     … Document control settings
}
```
{: codeblock}

Definições de conectividade do banco de dados

Tipo de banco de dados: DB2, DashDB, Informix, Oracle, Sybase, SQLServer, MySQL.

Conectividade conforme especificada na instância de serviço do BD, muitos tipos de BD
requerem que configurações específicas sejam passadas em
‘Options’

```
"dbDefinitions":{
     "db1":{
          "type":"DashDB",
          "host":"dashdb-enterprise-yp-dal09-11.services.dal.bluemix.net",
          "port":50001,
          "db":"BLUDB",
          "username":"bluadmin",
          "password":"NmI4MDViMzBkNDUz",
          "options":"Security=SSL"
     },
     "db2": {
          "type": "SQLServer",
          "db": "qatest",
          "host": "9.20.27.37",
          "port": 1433,
          "username": "sa",
          "password": "Pass1234",
          "options": "EnableQuotedIdentifiers=1"
     }
},
```
{: codeblock}

Configurações do nó de origem

Conectividade e tabela de BD de referência usadas para dar origem a uma
determinada ramificação no fluxo do Modeler, conforme identificado pelo nome do nó de
origem.

```
"inputs": [
     {
          "odbc": {
               "dbRef”; “db2”,
               "table": "DRUG1N",
          },
          "node": "ScoreInput",
          "attributes": []
     }
],
```
{: codeblock}

A tabela é o nome da tabela de banco de dados. Essa tabela é usada para
substituir o nó de origem do fluxo. O nó é o nome da origem de dados
para o fluxo. O dbRef referencia a conectividade do DB definida
pelo objeto dbDefinitions e a tabela é a tabela de banco de dados usada
como a entrada da ramificação específica no fluxo do Modeler. O nó
identifica o nó de origem original no fluxo do Modeler a ser
substituído por um nó de origem do DB construído com esses parâmetros
fornecidos.

Exportar configurações de nó

Conectividade e tabela de BD de referência usadas para persistir dados
de uma determinada ramificação no fluxo do Modeler, conforme identificado pelo nome do nó
do terminal.

O método de controle usado ao persistir dados é comunicado por meio do
atributo insertMode:

*  Anexar – a tabela deve existir e ser compatível para inserção

*  Criar – a tabela é criada (um erro será retornado se ela já
existir)

*  Eliminar – se a tabela referenciada existir, ela será eliminada e
recriada

*  Atualizar – as linhas existentes da tabela são excluídas antes de
inserir novas linhas

```
"exports": [
     {
          "odbc": {
               "dbRef”; “db1”,
               "table": "DRUGSCORES",
               “insertMode”:”Append”
          },
          "node": "ExportScores",
          "attributes": [],
     }
],
```
{: codeblock}

A tabela é o nome da tabela de banco de dados na qual gravar os resultados da tarefa.
O nó é o nome do nó terminal para o fluxo. Semelhante às
configurações do nó de origem, nó identifica o nó de saída original no
fluxo do Modeler a ser substituído por um nó de exportação do DB construído
com esses parâmetros fornecidos.

Nota: as configurações do nó de Exportação e do nó de Origem são
importantes para fazer com que sua tarefa seja executada com sucesso.

Substituições de valor de parâmetro

Para substituir os valores padrão dos parâmetros de nível de fluxo antes da
execução da tarefa, é possível especificar os pares de nome/valor a serem usados na
definição de tarefa.

```
"parameterOverride": [
     {
          "name": "p1",
          "value": "v1"
     },
     {
          "name": "p2",
          "value": "v2"
     }
],
```
{:codeblock}

Formatos de relatório desejados para documento de avaliação retornado ao
executar um tipo de tarefa de avaliação

Todos os documentos gerados da execução da tarefa de Avaliação são
empacotados em um arquivo .zip. Somente nós do construtor de documento capazes
de suportar o reportFormat indicado são executados e têm
seu conteúdo retornado após a execução bem-sucedida da tarefa.

Os formatos suportados são HTML, JPG, PNG, RTF, SAV, TAB e XML.

```
"reportFormat": "HTML"
```
{: codeblock}

Exemplo completo de JSON

```
{ 
     "action": "TRAINING", 
     "model": { 
          "id": "str2" 
          "name": "stream1.str" 
     },
     "dbDefinitions":{
          "db":{        
                    "type":"DashDB",
                    "host":"dashdb-enterprise-yp-dal09-11.services.dal.bluemix.net",        
                    "port":50001,        
                    "db":"BLUDB", 
                    "username":"bluadmin",  
                    "password":"NmI4MDViMzBkNDUz", 
                    "options":"Security=SSL"      
               }
     },
     "setting": {          
          "inputs": [
                    {
                         "odbc": {
                                   "dbRef”; “db”,
                                   "table": "DRUG1N",
                         },
                         "node": "ScoreInput",
                         "attributes": []
                    }
          ],
          "parameterOverride": [
                    {
                        "name": "p1",
                        "value": "v1"
                    },
                    {
                        "name": "p2",
                        "value": "v2"
                    }
          ],
     }
}
```
{: codeblock}

## Detalhes da API de tarefa em lote

As seções a seguir fornecem
detalhes da API de gerenciamento de arquivos do SPSS Modeler em
lote da tarefa.

PUT /v1/file/{id}

Descrição: faz upload de um arquivo de fluxo do SPSS Modeler para uso nas tarefas
em lote.

Nota: se houver um arquivo já armazenado com o ID especificado,
ele será sobrescrito.

Tipos de conteúdo:

```
@Consumes({ "multipart/form-data"  })
@Produces({“application/json”})
```
{: codeblock}

Parameters:

Chave de acesso retornada como credenciais na provisão ou ligação:

```
@QueryParam("accesskey") 
```
{: codeblock}

ID do arquivo especificado pelo usuário. Deve ser exclusivo no repositório de
instância de serviço:

```
@PathParam("id") 
```
{: codeblock}

Respostas:

Sucesso. Retorna a URL que pode ser usada para fazer download do arquivo do
repositório da instância de serviço:

```
@ApiResponse(code = 200)
```
{: codeblock}

Outro erro. JSON de exceção retornada:

```
@ApiResponse(code = 500)
```
{: codeblock}

DELETE /v1/file/{id}

Descrição: exclui o arquivo armazenado com o ID especificado do
repositório de instância de serviço.

Tipos de conteúdo:

```
@Produces({“application/json”})
```
{: codeblock}

Parameters:

Chave de acesso retornada como credenciais na provisão ou ligação:

```
@QueryParam("accesskey")
```
{: codeblock}

ID especificado pelo usuário para o arquivo quando transferido por upload:

```
@PathParam("id") 
```
{: codeblock}

Respostas:

Sucesso. O arquivo especificado foi excluído:

```
@ApiResponse(code = 200)
```
{: codeblock}

Erro. Um arquivo armazenado sob este ID não existia no repositório de instância de
serviço:

```
@ApiResponse(code = 404)
```
{: codeblock}

Outro erro. JSON de exceção retornada:

```
@ApiResponse(code = 500)
```
{: codeblock}

GET /v1/file/

Descrição: retorna uma lista dos IDs de todos os arquivos que estão sendo gerenciados
no repositório de instância de serviço para processamento de tarefa em lote.

Tipos de conteúdo:

```
@Produces({“application/json”})
```
{: codeblock}

Parameters:

Chave de acesso retornada como credenciais na provisão ou ligação:

```
@QueryParam("accesskey")  
```
{: codeblock}

Respostas:

Sucesso. Retorna uma matriz JSON de valores de ID de
arquivo:

```
@ApiResponse(code = 200)
```
{: codeblock}

Outro erro. JSON de exceção retornada:

```
@ApiResponse(code = 500)
```
{: codeblock}

GET /v1/file/{id}

Descrição: recupera o arquivo de fluxo do SPSS Modeler armazenado para
uso no processamento de tarefa em lote com o ID especificado.

Tipos de conteúdo:

```
@Produces({ "application/octet-stream" })
```
{: codeblock}

Parameters:

Chave de acesso retornada como credenciais na provisão ou ligação:

```
@QueryParam("accesskey")  
```
{: codeblock}

ID especificado pelo usuário para o arquivo quando transferido por upload:

```
@PathParam("id") 
```
{: codeblock}

Respostas:

Sucesso. Retorna o arquivo do SPSS Modeler:

```
@ApiResponse(code = 200)
```
{: codeblock}

Erro. Um arquivo armazenado sob este ID não existia no repositório de instância de
serviço:

```
@ApiResponse(code = 404)
```
{: codeblock}

Outro erro. JSON de exceção retornada:

```
@ApiResponse(code = 500)
```
{: codeblock}
