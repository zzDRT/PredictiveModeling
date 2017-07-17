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

# Construindo aplicativos de analítica preditiva


Esta seção descreve o processo para implementar e
usar um aplicativo de amostra.

**Nome do cenário**: predição de linha de produto.

**Descrição do cenário**: nosso cliente está cuidando de uma das cadeias de
lojas mais famosas da Europa. Eles
gostariam que nós descobríssemos os interesses dos clientes em
sua linha de produtos, como acessórios pessoais,
equipamento de acampamento e proteção para atividades externas.
Um Cientista de dados desenvolve um modelo preditivo e o
compartilha com você (o desenvolvedor). Sua tarefa é preparar e
implementar o aplicativo Node.js que recomenda linhas de
produtos esportivos fazendo solicitações de pontuação em relação
ao modelo implementado.

1. Efetue logon no Bluemix e crie uma instância do Machine Learning.

2. Ative o Painel do Machine Learning.

3. Acesse a guia Amostras.

4. Na seção Modelos de amostra, localize o quadro Predição de linha de produto
e clique no botão Incluir modelo (+). Agora você verá o modelo de amostra Predição de linha de produto na
lista de modelos disponíveis na guia Modelos.

   **Nota**: se você quiser usar seu próprio projeto e modelos do
Data Science Experience, em vez das amostras, deverá persistir um
modelo customizado no repositório do Machine Learning. É possível fazer isso usando a API REST ou as bibliotecas do
cliente. Para obter
detalhes, veja Desenvolvimento e persistência do modelo customizado.

5. No menu Ação, selecione Criar implementação para implementar o
modelo Predição de linha de produto como uma implementação on-line.

6. Especifique o nome da implementação (por exemplo, Predição de linha de
produto) e clique em Salvar. Agora, você deverá ver a implementação de
Predição de linha de produto na lista Implementações.

7. No menu Ação, selecione Visualizar detalhes para a sua implementação.
   O Terminal de pontuação apresentado na área de janela de detalhes será
usado para executar solicitações de escore.

8. Para executar solicitações de pontuação por meio da API
REST, são necessários um terminal de pontuação e um token de autorização. Para obter mais
informações, veja Implementando modelos on-line.

9. Agora é possível experimentar com o aplicativo Node.js de amostra
disponível em
   https://github.com/pmservice/product-line-prediction.

   Para implementar automaticamente o código do aplicativo de amostra no
espaço do Bluemix, acesse a guia Amostras e, na seção
Aplicativos de amostra, selecione o aplicativo Product Line
Prediction e implemente-o clicando no botão (+).
Autentique-se no formulário DeployToBluemix, se solicitado.

   Agora, você deverá ver o aplicativo de amostra no
Painel do Bluemix na seção Todos os apps.

10. Clique no aplicativo para visualizar os seus detalhes. Aqui
é possível modificar detalhes do aplicativo como o número de
instâncias, memória, etc.

11. Agora, é possível ligar seu aplicativo à instância do
Machine Learning. Na guia Conexões, clique em Conectar existente.
Após o
reestabelecimento, seu aplicativo está conectado à instância de serviço.

12. Execute o aplicativo usando o endereço da rota.

13. No aplicativo, selecione sua implementação de Previsão de
Linha de Produto. Agora você está pronto para jogar com
registros de amostra e com os resultados de previsão.
