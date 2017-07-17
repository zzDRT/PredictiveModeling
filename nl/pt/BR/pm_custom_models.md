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

# Desenvolvimento e persistência do modelo customizado

## Trabalhando com modelos customizados

### Nome do cenário: Predição da linha de produto.

### Descrição do cenário:

Nosso cliente está cuidando de uma das cadeias de
lojas mais famosas da Europa. Eles
gostariam que nós descobríssemos os interesses dos clientes em
sua linha de produtos, como acessórios pessoais,
equipamento de acampamento e proteção para atividades externas.
Um Cientista de dados desenvolve um modelo preditivo e o
compartilha com você (o desenvolvedor). Sua tarefa é implementar
o modelo e gerar análise preditiva fazendo solicitações de
pontuação em relação ao modelo implementado.

### Desenvolvimento e persistência do modelo customizado

1. Use o Data Science Experience para criar modelos customizados. Depois de se inscrever, assine e conclua as etapas a seguir.

2. Na primeira vez em que fizer login, será solicitado que você crie uma organização e um espaço. Clique em **Continuar** e aceite os valores
padrão.

3. Após a organização ser criada, acesse **Meus
Projetos** e clique em **criar
projeto**.

4. Especifique um nome e uma descrição para o seu projeto e
clique em **Criar**. O nome do projeto que
você especificou também será usado como seu nome do Contêiner de
destino.

5. Após o projeto ser criado, é possível incluir notas
e começar a desenvolver seus próprios modelos ou
fazer upload de uma das seguintes notas de amostra:

   *  [Desenvolvendo modelos Spark MLlib com o Python](https://apsportal.ibm.com/analytics/notebooks/89492fd6-a641-4819-9176-3d9381561df9/view?access_token=d80bef1a172d1d83d3721b101886337158457281774186f181a2e6a5b57f5ec7)

   *  [Desenvolvendo modelos Spark MLlib com o Scala](https://apsportal.ibm.com/analytics/notebooks/c8652d2c-bfc9-4354-8168-f1c9f7f8dfc2/view?access_token=02a83fea8450a452c8de76af98dae078459d0f56810ddef4f4c62d5bc4fc72cf)

   *  [Desenvolvendo modelos scikit-learn com o Python](https://apsportal.ibm.com/analytics/notebooks/5215a61a-16d7-4fa2-b060-e3e243ceebe3/view?access_token=70f48c95c5571a614ce97484d3f168b1d9b6aeebce015187d3d77ce6038f025e)



### Implementação e pontuação do modelo customizado

Consulte as seguintes seções para obter instruções sobre como
implementar e pontuar modelos ou consulte a seção de pontuação
das notas vinculada anteriormente.

*  [Implementando modelos on-line](pm_service_api_spark_online.html)

*  [Implementando modelos em lote](pm_service_api_spark_batch.html)

*  [Implementando modelos de fluxo](pm_service_api_spark_streaming.html)
