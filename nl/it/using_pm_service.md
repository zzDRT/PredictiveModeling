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

# Utilizzo del servizio Machine Learning con i modelli IBM SPSS Modeler

I metodi di modellazione disponibili nella tavolozza dei modelli SPSS Modeler ti consentono di ricavare nuove
informazioni dai tuoi dati e di sviluppare modelli predittivi. Ogni metodo ha determinati punti di forza e si presta meglio per particolari tipi di problemi. 
{: .shortdesc}

Per i dettagli su SPSS Modeler e sugli algoritmi di modellazione che fornisce, consulta [IBM
Knowledge Center](https://www.ibm.com/support/knowledgecenter/SS3RA7).

Dopo che sono stati implementati i requisiti di input e di output della progettazione della tua applicazione Bluemix e
del ramo di calcolo del punteggio SPSS Modeler, il tuo analista dei dati può modificare qualsiasi aspetto interno del ramo di calcolo del
punteggio. L'analista dei dati può anche modificare uno o più algoritmi di modello utilizzati in un'operazione di aggiornamento, assicurandoti di poter ottimizzare la tua analisi predittiva senza dover riscrivere le tue applicazioni.

Prendi nota delle seguenti informazioni importanti riguardo il servizio Machine
Learning quando utilizzato con i modelli creati in SPSS Modeler:

*  Quando un ramo di calcolo viene preparato per l'utilizzo per il calcolo in tempo reale, i dati di input ricevuti nella richiesta di calcolo
devono sostituire il nodo di origine creato nel ramo di calcolo e l'output di analisi predittiva risultante
deve ritornare al flusso della risposta (sostituendo di fatto il nodo del terminale
nella progettazione del ramo di calcolo).

*  Quando il ramo di calcolo è stato preparato per l'esecuzione in tempo reale in Bluemix, non può richiedere una connessione
a un servizio esterno. Ad esempio, una progettazione del ramo di calcolo di IBM Analytical Decision Management
non può contenere i riferimenti alle regole o ai modelli archiviati in un repository IBM SPSS Collaboration e
Deployment Services.

*  L'esecuzione di un ramo di calcolo per il calcolo in tempo reale in Bluemix non può richiedere un servizio
esterno. Ad esempio, non puoi distribuire e calcolare gli algoritmi del modello che richiedono un server IBM SPSS
Analytic e un archivio dati Apache Hadoop in tempo reale.

*  Machine Learning supporta lo script Modeler integrato con Python.
   Esistono alcune restrizioni dovute al metodo utilizzato per
   l'elaborazione dei flussi prima di essere eseguiti in Machine Learning.
   Normalmente, se un utente sceglie
di controllare l'esecuzione del flusso, fa riferimento al nodo del terminale del ramo.
   Per Machine Learning, quando elaboriamo il flusso, identifichiamo
   i nodi da JSON che saranno sovrascritti e quindi eseguiamo
   la sostituzione prima dell'esecuzione del flusso. Questo causa il malfunzionamento del flusso nello script perché i nodi di esportazione e l'input di riferimento
non esistono più. La soluzione è di utilizzare l'ID di un altro nodo per identificare univocamente il ramo
durante l'esecuzione. Ciò assicura che il flusso eseguito sia definito nello script Python integrato.

Per ulteriori dettagli sul supporto corrente per i modelli predittivi eseguiti dal server
IBM SPSS Analytic, consulta la sezione Analytic Server in
IBM Knowledge Center.

1. Puoi scaricare il codice di esempio Node.js per provare il servizio Machine
   Learning. Completa la seguente procedura per creare la tua
   applicazione Bluemix ed eseguire il bind del servizio Machine Learning.
   Questi esempi utilizzano Node.js perché è un runtime di ampio utilizzo.
   È possibile utilizzarne degli altri come iOS, Ruby, Perl o Java.

2. Utilizza il comando cf create-service per creare un'istanza del
   servizio:

   ```
   cf create-service pm-20 Free {local naming}
   ```

   Ad esempio:

   ```
   cf create-service pm-20 Free my_pm_free
   ```

   Questo comando crea un'istanza del servizio Machine Learning
   con il piano Gratuito denominato my_pm_free nel tuo spazio Bluemix.

3. Utilizza il comando `cf create-service-key` per creare le credenziali del servizio:

   ```cf create-service-key "{service instance name}" {vcap key name}```

   Ad esempio:

   ```cf create-service-key "IBM Watson Machine Learning - my instance" Credentials-1```

   Questo comando crea le credenziali del servizio Machine Learning.

4. Utilizza il comando cf bind-service per eseguire il bind dell'istanza del servizio
   my_pm_free alla tua applicazione.

   ```cf bind-service AppName my_pm_service```

   Ad esempio:

   ```cf bind-service my_app1 my_pm_free```

   Questo comando esegue il bind dell'istanza del servizio Machine Learning
   `my_pm_free` all'applicazione Bluemix my_app1.

5. Credenziali di Machine Learning:

   Dopo che hai eseguito il bind dell'istanza del servizio Machine Learning alla tua
   applicazione Bluemix, le credenziali di Machine Learning vengono
   aggiunte alla variabile di ambiente `VCAP_SERVICES`:

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

   La variabile di ambiente `VCAP_SERVICES` include le seguenti informazioni:

   <dl>
   
   <dt>piano</dt>
   <dd>Il piano Machine Learning utilizzato nel provisioning del servizio.</dd>

   <dt>url</dt>
   <dd>L'indirizzo dell'istanza del servizio Machine Learning.</dd>

   <dt>chiave_accesso</dt>
   <dd>Il parametro di query accessKey da passare in tutte le richieste
            a questa istanza del servizio.</dd>

   </dl>
            
Ad esempio:             

```
Get https://ibm-watson-ml.mybluemix.net/pm/v1/model/sales_model2?accesskey=XXXXXXXXXXXXX 
```

   Codice Node.js di esempio che mostra come ottenere l'accessKey dalla
   variabile di ambiente `VCAP_SERVICES`:

```
   if (process.env.VCAP_SERVICES) {
        var env = JSON.parse(process.env.VCAP_SERVICES);
        var credentials = env['pm-20'][0].credentials;
        var accessKey = credentials.access_key;
    }
```
