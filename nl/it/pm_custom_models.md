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

# Sviluppo e persistenza del modello personalizzato

## Utilizzo di modelli personalizzati

### Nome scenario: Previsione linea di prodotti.

### Descrizione scenario:

Il nostro cliente sta realizzando una delle più famose catene di negozi
in Europa. Ci richiede di scoprire gli interessi dei propri clienti in merito alla propria linea di prodotti,
come Accessori personali, Attrezzature da campeggio e Protezione all'aperto.
Un data scientist sviluppa un modello predittivo e lo condivide con te (lo sviluppatore). Il tuo compito è quello di distribuire
il modello e di generare l'analisi predittiva effettuando richieste di punteggio sul modello distribuito.

### Sviluppo e persistenza del modello personalizzato

1. Utilizza Data Science Experience per creare modelli personalizzati. Dopo la registrazione, esegui l'accesso e completa la seguente
procedura.

2. La prima volta che accedi, ti verrà richiesto di creare un'organizzazione e uno spazio. Fai clic su
**Continua** e accetta i valori predefiniti.

3. Dopo aver creato l'organizzazione, vai a **I miei progetti** e fai clic
su **Crea progetto**.

4. Specifica un nome e una descrizione per il progetto e fai clic su **Crea**. Il nome del
progetto specificato verrà utilizzato anche come nome del contenitore di destinazione.

5. Una volta creato il progetto, puoi aggiungere dei blocchi appunti e iniziare a sviluppare i tuoi propri
modelli o puoi caricare uno dei seguenti blocchi appunti di esempio:

   *  [Sviluppo di modelli Spark MLlib con Python](https://apsportal.ibm.com/analytics/notebooks/89492fd6-a641-4819-9176-3d9381561df9/view?access_token=d80bef1a172d1d83d3721b101886337158457281774186f181a2e6a5b57f5ec7)

   *  [Sviluppo di modelli Spark MLlib con Scala](https://apsportal.ibm.com/analytics/notebooks/c8652d2c-bfc9-4354-8168-f1c9f7f8dfc2/view?access_token=02a83fea8450a452c8de76af98dae078459d0f56810ddef4f4c62d5bc4fc72cf)

   *  [Sviluppo di modelli scikit-learn con Python](https://apsportal.ibm.com/analytics/notebooks/5215a61a-16d7-4fa2-b060-e3e243ceebe3/view?access_token=70f48c95c5571a614ce97484d3f168b1d9b6aeebce015187d3d77ce6038f025e)



### Distribuzione e punteggio del modello personalizzato

Vedi le seguenti sezioni per istruzioni sulla distribuzione e sul calcolo del punteggio dei modelli o consulta
la sezione relativa al calcolo del punteggio dei blocchi appunti collegati precedentemente.

*  [Distribuzione di modelli online](pm_service_api_spark_online.html)

*  [Distribuzione di modelli batch](pm_service_api_spark_batch.html)

*  [Distribuzione di modelli streaming](pm_service_api_spark_streaming.html)
