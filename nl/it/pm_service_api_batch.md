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

# API del lavoro batch del servizio Machine Learning per i modelli IBM SPSS Modeler


*  [Eliminazione lavori](#deleting-jobs)

*  [Controllo dello stato di un lavoro](#checking-the-status-of-a-job)

*  [Reinvia un lavoro](#resubmit-a-job)

*  [Invia un lavoro in un file del flusso Modeler caricato](#submit-a-job-against-an-uploaded-modeler-stream-file)

*  [Carica un file del flusso da utilizzare nei tuoi lavori](#upload-a-stream-file-to-use-in-your-jobs)

*  [Tipi di lavoro](#job-types)

*  [Stato del lavoro](#job-status)

*  [Dettagli API lavoro](#job-api-details)

*  [JSON della definizione del lavoro](#job-definition-json)

*  [Dettagli API lavoro batch](#batch-job-api-details)

L'API del lavoro batch per il servizio Machine Learning supporta le
attività a esecuzione prolungata correlate alla formazione del modello, alla valutazione del modello e
al calcolo del punteggio batch. Questa API gestisce due tipi di asset: i file del flusso SPSS Modeler utilizzati nei lavori batch e
le definizioni del lavoro inviate. Per ogni file del flusso Modeler caricato, esistono più lavori e
più tipi inviati. Se un lavoro ha il potenziale per aggiornare i contenuti del file del flusso Modeler, il file modificato
sarà incluso negli allegati del risultato del lavoro. In un tipo di lavoro di valutazione del modello, tutti i file valutati
generati saranno presenti negli allegati del risultato del lavoro.

Per un esempio di adozione di lavoro batch, fai riferimento al
seguente blocco appunti: [From SPSS stream to batch scoring with
Python](https://apsportal.ibm.com/analytics/notebooks/9d7ce38e-9417-4c76-a6b9-5bc8cf40938a/view?access_token=5ca87e3007804e5b2bbbce77c20e99ac3c164d66f2d28dfffb54aa365caaef37).

## Eliminazione lavori

Puoi eliminare i lavori, che elimina il lavoro se è al momento in esecuzione. Richiama DELETE nell'ID lavoro (e puoi eliminarne più
di uno alla volta):

DELETE https://{PA Bluemix load balancer URL}/pm/v1/jobs/{job ID
1, job ID 2,...}?accesskey=xxxxxxxxxx

Il valore restituito indica quanti lavori a cui fa riferimento l'ID nella richiesta sono stati eliminati. Se
questo numero non corrisponde all'elenco trasmesso nella richiesta, devi controllare lo stato
dei singoli lavori.

## Controllo dello stato di un lavoro

Puoi utilizzare GET per ottenere lo stato del tuo ID lavoro in qualsiasi momento:

GET https://{PA Bluemix load balancer URL}/pm/v1/jobs/{job
ID}/status?accesskey=xxxxxxxxxx

Il JSON restituito indica jobstatus e, se il lavoro è terminato con esito
positivo, un dataUrl che puoi utilizzare per ottenere tutto il contenuto del file
generato.

## Reinvia un lavoro

Richiama PUT per <job ID>. Non deve essere in uno stato di esecuzione:

PUT https://{PA Bluemix load balancer URL}/pm/v1/jobs/{job
ID}?accesskey=xxxxxxxxxx

con content_type "application/json" che include il JSON della definizione del lavoro
nuovo o aggiornato nel corpo della richiesta.

Questa chiamata crea effettivamente un nuovo lavoro se l'ID di riferimento non esiste, con 201 su 200
restituzioni che indica cosa è accaduto. Devi trasmettere il JSON della definizione del lavoro da utilizzare in questa esecuzione.

## Invia un lavoro in un file del flusso Modeler caricato

Richiama PUT per la tua definizione del lavoro da inserire in una coda di
esecuzione:

PUT https://{PA Bluemix load balancer URL}/pm/v1/jobs/{job
ID}?accesskey=xxxxxxxxxx

Con content_type "application/json" che include il JSON della definizione del lavoro
nel corpo della richiesta.

Questa richiesta viene restituita immediatamente, il che significa un esito positivo se la definizione del lavoro è stata posizionata nella coda di
esecuzione. Non esiste una verifica della capacità di eseguire correttamente il flusso Modeler
come configurato nella tua definizione del lavoro. Il lavoro sarà eseguito da
uno dei server del lavoro Machine Learning nel cloud e puoi monitorarne
lo stato. Se esegui la formazione del modello e indichi che desideri eseguire l'aggiornamento automatico, il lavoro
sostituirà il file del flusso Modeler originale nel momento dell'esecuzione positiva del lavoro.

Per ulteriori informazioni sul JSON della definizione del lavoro, consulta [JSON della
definizione del lavoro](#job-definition-json).

## Carica un file del flusso da utilizzare nei tuoi lavori

Nota: il dashboard Machine Learning è solo per il calcolo in tempo
reale. Non puoi utilizzarlo per eseguire i lavori (calcolo batch).

Richiama PUT per rendere un file del flusso Modeler accessibile ai lavori:

PUT https://{PA Bluemix load balancer URL}/pm/v1/file/{file
ID}?accesskey=xxxxxxxxxx

con content_type "multipart/form-data" che passa il file nella
richiesta.

Dovrai fare riferimento al nome univoco utilizzato (ID file nella chiamata PUT) in una chiamata
DELETE all'API del file così come al riferimento del modello
nelle tue definizioni del lavoro. Nota: lo spazio dei nomi dei file utilizzati
nei tuoi lavori è completamente sotto il tuo controllo - in questo modo il comando PUT di un
file con lo stesso <ID file> sostituisce implicitamente la copia
corrente.

Puoi utilizzare GET per ottenere un elenco di tutti i file caricati per i tuoi lavori
omettendo l'<ID file> o richiamare uno specifico file con GET e il
relativo ID. Puoi anche utilizzare DELETE per eliminare un file caricato (ciò, ovviamente, causerà degli errori
in ogni esecuzione del lavoro in attesa che fa riferimento
al file).

Esempio di
richiesta:

```
    Content-Type: multipart/form-data
    Parametri:
        Parametri modulo:
            model_file: il file del modello da caricare
        Parametri percorso:
            contextId: l'identificativo univoco da assegnare al tuo modello o un riferimento al modello distribuito da aggiornare
        Parametri query:
            accesskey: access_key da env.VCAP_SERVICES
```
{: codeblock}

Risposta quando la distribuzione riesce:

```
    Content-Type: application/json
    Codice di stato: 200
    corpo:
        {
           "flag":true,
           "message":"informazioni dettagliate"
         }
```
{: codeblock}

Risposta quando la distribuzione non riesce:

```
    Content-Type: application/json
    Codice di stato: 200
    corpo:
        {
           "flag":false,
           "message":"motivo"
        }
```
{: codeblock}

## Tipi di lavoro

Formazione: questo tipo di lavoro indica che tutti i nodi del terminale "builder del modello"
devono essere eseguiti nel flusso Modeler. Dopo che il lavoro è stato completato correttamente, verrà creato un flusso Modeler aggiornato
con i nuovi nugget del modello eseguito nei risultati del lavoro che possono essere richiamati. Se il file del flusso Modeler
dispone di collegamenti dai nodi di build del modello ai nugget del modello eseguito nei rami di valutazione e calcolo,
sarà eseguito un aggiornamento di questi nodi.

Valutazione: questo tipo di lavoro attiva l'esecuzione di tutti i nodi del terminale "builder del
documento" (principalmente dalle schede Grafici e Output nel
client Modeler) che generano contenuti del file di report statico che possono essere
trasmessi al chiamante. Il ramo di calcolo non è considerato come parte di questo tipo di lavoro.

Aggiornamento automatico: una versione del tipo di lavoro TRAINING dove,
al completamento con esito positivo del lavoro, il file del flusso Modeler originale
nell'elenco di file batch sarà aggiornato. La valutazione e la decisione esplicita riguardante un evento di aggiornamento dei flussi Modeler
distribuiti per il calcolo in tempo reale si suppone non siano coperti dall'aggiornamento automatico in questo momento.

Punteggio batch: l'esecuzione del nodo del terminale in cui hai applicato l'opzione
Utilizza come ramo di calcolo, che indica che questo è il ramo di calcolo
in questa progettazione del flusso Modeler. La definizione del lavoro deve specificare
i dettagli dell'esportazione così come i dettagli dell'origine.

Esegui flusso: l'esecuzione è simile al fare clic sul pulsante di "esecuzione" verde
in Modeler con l'opzione Esegui questo script selezionata nella scheda
Esecuzione delle proprietà del flusso. L'utilizzo copre i bisogni dell'esecuzione con script
della formazione del modello o di altri tipi di lavoro. Tutto il controllo dinamico dello script deve essere gestito dai parametri del flusso,
con i valori del parametro trasmessi nella definizione del lavoro.

## Stato del lavoro

In sospeso: la definizione del lavoro è stata inviata ma non è stata ancora
richiesta da un server del lavoro per l'esecuzione.

In esecuzione: la definizione del lavoro è stata richiesta da un server del lavoro ed
è in esecuzione.

In annullamento: il lavoro è in fase di annullamento.

Annullato: il lavoro è stato annullato.

Non riuscito: il lavoro non è riuscito. I dettagli sulla causa dell'errore vengono
restituiti con GET nello stato del lavoro.

Riuscito: il lavoro è stato eseguito correttamente. Qualsiasi risultato comunicato per
questo evento viene restituito nel JSON di GET nello stato del lavoro.

## Dettagli API lavoro

POST /v1/jobs/{id}

Descrizione: invia una definizione del lavoro per l'esecuzione. Si verificherà un errore se
<job ID> già esiste.

Tipi di contenuto:

```
@Consumes({“application/json”})
@Produces({“application/json”})
```
{: codeblock}

Parametri:

La chiave di accesso restituita come credenziale nel provisioning o nel
bind:

```
@QueryParam("accesskey")
```
{: codeblock}

ID lavoro specificato dall'utente. Deve essere univoco per un'istanza del servizio Machine
Learning:

```
@PathParam("id")
```
{: codeblock}

Definizione lavoro JSON (consulta JSON della definizione del lavoro):

```
@BodyParam
```
{: codeblock}

Risposte:

Esito positivo. Il lavoro è stato inviato per l'esecuzione e restituisce il JSON che descrive il lavoro inviato.

```
@ApiResponse(code = 200)
```
{: codeblock}

Errore, il lavoro già esiste. Un lavoro con questo ID è già stato inviato. Il messaggio restituito descrive questo errore.

```
@ApiResponse(code = 409)
```
{: codeblock}

Altro errore. Restituito JSON dell'eccezione

```
@ApiResponse(code = 500)
```
{: codeblock}

PUT /v1/jobs/{id}

Descrizione: crea o aggiorna un lavoro. Se un lavoro con questo ID non esiste,
lo crea; altrimenti, lo aggiorna (che, di fatto, lo reinvia per l'esecuzione).

Tipi di contenuto:

```
@Consumes({“application/json”})
@Produces({“application/json”})
```
{: codeblock}

Parametri:

La chiave di accesso restituita come credenziale nel provisioning o nel
bind:

```
@QueryParam("accesskey")
```
{: codeblock}

ID lavoro specificato dall'utente:

```
@PathParam("id")
```
{: codeblock}

Definizione lavoro JSON (consulta JSON della definizione del lavoro):

```
@BodyParam
```
{: codeblock}

Risposte:

Esito positivo. Il lavoro è aggiornato ed è stato inviato per l'esecuzione. Restituisce il JSON che descrive il lavoro inviato.

```
@ApiResponse(code = 200)
```
{: codeblock}

Esito positivo. Il lavoro è stato creato ed è stato inviato per l'esecuzione. Restituisce il JSON che descrive il lavoro inviato.

```
@ApiResponse(code = 201)
```
{: codeblock}

Altro errore. Restituito JSON dell'eccezione.

```
@ApiResponse(code = 500)
```
{: codeblock}

GET /v1/jobs

Descrizione: restituisce un elenco di tutti i lavori definiti su questa istanza del servizio Machine
Learning.

Tipi di contenuto:

```
@Produces({“application/json”})
```
{: codeblock}

Parametri:

La chiave di accesso restituita come credenziale nel provisioning o nel
bind:

```
@QueryParam("accesskey")
```
{: codeblock}

Risposte:

Esito positivo. Restituisce l'array JSON di tutte le definizioni del lavoro:

```
@ApiResponse(code = 200)
```
{: codeblock}

Altro errore. Restituito JSON dell'eccezione:

```
@ApiResponse(code = 500)
```
{: codeblock}

GET /v1/jobs/{id}

Descrizione: richiede la restituzione di una specifica definizione del lavoro.

Tipi di contenuto:

```
@Produces({“application/json”})
```
{: codeblock}

Parametri:

La chiave di accesso restituita come credenziale nel provisioning o nel
bind:

```
@QueryParam("accesskey")
```
{: codeblock}

ID lavoro inviato:

```
@PathParam("id")
```
{: codeblock}

Risposte:

Esito positivo. Restituisce il JSON che descrive il lavoro di riferimento:

```
@ApiResponse(code = 200)
```
{: codeblock}

Errore, lavoro non trovato. Dettagli nel messaggio restituito:

```
@ApiResponse(code = 404)
```
{: codeblock}

Altro errore. Restituito JSON dell'eccezione:

```
@ApiResponse(code = 500)
```
{: codeblock}

GET /v1/jobs/{id}/status

Descrizione: richiama lo stato di uno specifico lavoro.

Tipi di contenuto:

```
@Produces({“application/json”})
```
{: codeblock}

Parametri:

La chiave di accesso restituita come credenziale nel provisioning o nel
bind:

```
@QueryParam("accesskey")
```
{: codeblock}

ID lavoro inviato:

```
@PathParam("id")
```
{: codeblock}

Risposte:

Esito positivo. Restituisce il JSON che descrive lo stato del lavoro:

```
@ApiResponse(code = 200)
```
{: codeblock}

Errore di lavoro non trovato, dettagli nel messaggio restituito:

```
@ApiResponse(code = 404)
```
{: codeblock}

Altro errore. Restituito JSON dell'eccezione:

```
@ApiResponse(code = 500)
```
{: codeblock}

DELETE /v1/jobs/{ids}

Descrizione: elimina uno o più lavori. Eliminerà il lavoro se è al momento in esecuzione.

Tipi di contenuto:

```
@Produces({“application/json”})
```
{: codeblock}

Parametri:

La chiave di accesso restituita come credenziale nel provisioning o nel
bind:

```
@QueryParam("accesskey")
```
{: codeblock}

Elenco ID lavoro o ID lavoro inviato con i valori ID suddivisi da
“,”

```
@PathParam("id" or “id,id,id”)
```
{: codeblock}

Risposte:

Esito positivo. Restituisce il numero di lavori eliminati:

```
@ApiResponse(code = 200)
```
{: codeblock}

Altro errore. Restituito JSON dell'eccezione:

```
@ApiResponse(code = 500)
```
{: codeblock}

## JSON della definizione del lavoro

Il JSON della definizione del lavoro contiene le seguenti sezioni generali:

Riferimento al modello predittivo e tipo di lavoro

Tipi di azione:

*  TRAINING – esegue il nodo di builder del modello mediante la formazione o
   l'aggiornamento dei nugget del modello. Il flusso Modeler aggiornato viene richiamato nei risultati del lavoro.

*  EVALUATION – esegue i nodi di analisi e builder del documento
   valutando il modello con formazione.

*  AUTO_REFRESH – esegue TRAINING e aggiorna i contenuti del file
   nello spazio di caricamento del file batch.

*  BATCH_SCORE – esegue il ramo di calcolo ed esporta i
   dati risultanti come indicato dalla definizione del lavoro.

*  RUN_STREAM – esegue il flusso Modeler come indicato nelle
   proprietà del flusso; più spesso utilizzato quando è richiesta l'esecuzione
   con script.

Modello: l'ID come specificato nell'azione di caricamento del file per il
riferimento del lavoro batch. Per ulteriori informazioni, vedi Caricamento di un file del flusso
da utilizzare nei tuoi lavori. Nota che viene utilizzato /pm/v1/file.

```
"action": "TRAINING",
"model": {
     "id": "str2" 
     "name":"stream1.str" 
},
```
{: codeblock}

Nota che l'id deve essere lo stesso ID del file utilizzato
nell'API PUT. Il nome non è obbligatorio, ma per la formazione e l'aggiornamento automatico del modello
il risultato del lavoro verrà salvato utilizzando il nome definito
qui. Se il nome non viene definito, il servizio Machine Learning
genererà il risultato in base alle regole di denominazione predefinite.

Impostazioni lavoro

Tutte le impostazioni richiedono di eseguire questo lavoro.

```
"setting": {
     … Export settings
     … Import settings
     … Parameter value settings
     … Document control settings
}
```
{: codeblock}

Definizioni di connettività del database

Tipo di database: DB2, DashDB, Informix, Oracle, Sybase, SQLServer, MySQL.

La connettività come specificata nell'istanza del servizio del DB, molti tipi di DB richiedono la trasmissione di
configurazioni specifiche nelle
‘Opzioni’

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

Impostazioni nodo di origine

La connettività del DB di riferimento e la tabella utilizzata per originare un ramo fornito nel flusso Modeler
come identificato dal nome del nodo di origine.

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

Il valore table è il nome della tabella del database. Questa tabella è utilizzata per sovrascrivere il nodo
di origine del flusso. Il valore node è il nome dell'origine dati
per il flusso. Il valore dbRef fa riferimento alla connettività del DB definita
dall'oggetto dbDefinitions e il valore table è la tabella del database utilizzata
come input del ramo fornito nel flusso Modeler. Il valore node
identifica il nodo di origine iniziale nel flusso Modeler da sostituire
con un nodo di origine del DB creato con questi parametri
forniti.

Impostazioni del nodo di esportazione

La connettività del DB di riferimento e la tabella utilizzata per conservare i dati per un ramo fornito nel flusso Modeler
come identificato dal nome del nodo del terminale.

Il controllo del metodo utilizzato durante la conservazione dei dati viene comunicato tramite l'attributo
insertMode:

*  Append – la tabella deve esistere ed essere compatibile per l'inserimento

*  Create – la tabella viene creata (se esiste già, viene restituito un
   errore)

*  Drop – se la tabella di riferimento esiste già, viene eliminata e
   ricreata

*  Refresh – le righe esistenti della tabella vengono eliminate prima di inserire
   nuove righe

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

Il valore table è il nome della tabella del database in cui scrivere i risultati del lavoro.
Il valore node è il nome del nodo del terminale per il flusso. Analogamente alle impostazioni
del nodo di origine, node identifica il nodo di output originale
nel flusso Modeler da sostituire con un nodo di esportazione del DB creato
con questi parametri forniti.

Nota: le impostazioni del nodo di esportazione e di origine
sono importanti per la corretta esecuzione del lavoro.

Sovrascritture del valore del parametro

Per sovrascrivere i valori predefiniti per i parametri al livello del flusso prima dell'esecuzione di un lavoro, puoi specificare
la coppia nome/valore da utilizzare nella definizione del lavoro.

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

Formati del report desiderati per il documento di valutazione restituito durante l'esecuzione di un tipo di lavoro di valutazione

Tutti i documenti generati dall'esecuzione del lavoro di valutazione vengono
compressi in un file .zip. Solo i nodi di builder del documento in grado di
supportare il reportFormat indicato vengono eseguiti e hanno i rispettivi
contenuti restituiti dopo un'esecuzione corretta del lavoro.

I formati supportati sono HTML, JPG, PNG, RTF, SAV, TAB e
XML.

```
"reportFormat": "HTML"
```
{: codeblock}

Esempio JSON completo

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

## Dettagli API del lavoro batch

Le seguenti sezioni forniscono i dettagli dell'API di gestione del file
Modeler SPSS del lavoro batch.

PUT /v1/file/{id}

Descrizione: carica un file del flusso Modeler SPSS da utilizzare nei lavori
batch.

Nota: se c'è già un file memorizzato nell'ID specificato,
sarà sovrascritto.

Tipi di contenuto:

```
@Consumes({ "multipart/form-data"  })
@Produces({“application/json”})
```
{: codeblock}

Parametri:

La chiave di accesso restituita come credenziale nel provisioning o nel
bind:

```
@QueryParam("accesskey") 
```
{: codeblock}

ID file specificato dall'utente. Deve essere univoco nel repository dell'istanza del servizio:

```
@PathParam("id") 
```
{: codeblock}

Risposte:

Esito positivo. Restituisce l'URL che può essere utilizzato per scaricare il file dal repository dell'istanza del servizio:

```
@ApiResponse(code = 200)
```
{: codeblock}

Altro errore. Restituito JSON dell'eccezione:

```
@ApiResponse(code = 500)
```
{: codeblock}

DELETE /v1/file/{id}

Descrizione: elimina il file memorizzato nell'ID specificato dal
repository dell'istanza del servizio.

Tipi di contenuto:

```
@Produces({“application/json”})
```
{: codeblock}

Parametri:

La chiave di accesso restituita come credenziale nel provisioning o nel
bind:

```
@QueryParam("accesskey")
```
{: codeblock}

ID specificato dall'utente per il file quando caricato:

```
@PathParam("id") 
```
{: codeblock}

Risposte:

Esito positivo. Il file specificato è stato eliminato:

```
@ApiResponse(code = 200)
```
{: codeblock}

Errore. Un file archiviato in questo ID non esiste nel repository dell'istanza del servizio:

```
@ApiResponse(code = 404)
```
{: codeblock}

Altro errore. Restituito JSON dell'eccezione:

```
@ApiResponse(code = 500)
```
{: codeblock}

GET /v1/file/

Descrizione: restituisce un elenco di ID di tutti i file gestiti
nel repository dell'istanza del servizio per l'elaborazione del lavoro batch.

Tipi di contenuto:

```
@Produces({“application/json”})
```
{: codeblock}

Parametri:

La chiave di accesso restituita come credenziale nel provisioning o nel
bind:

```
@QueryParam("accesskey")  
```
{: codeblock}

Risposte:

Esito positivo. Restituisce un array JSON di valori ID file:

```
@ApiResponse(code = 200)
```
{: codeblock}

Altro errore. Restituito JSON dell'eccezione:

```
@ApiResponse(code = 500)
```
{: codeblock}

GET /v1/file/{id}

Descrizione: richiama il file del flusso Modeler SPSS memorizzato da
utilizzare nell'elaborazione del lavoro batch nell'ID specificato.

Tipi di contenuto:

```
@Produces({ "application/octet-stream" })
```
{: codeblock}

Parametri:

La chiave di accesso restituita come credenziale nel provisioning o nel
bind:

```
@QueryParam("accesskey")  
```
{: codeblock}

ID specificato dall'utente per il file quando caricato:

```
@PathParam("id") 
```
{: codeblock}

Risposte:

Esito positivo. Restituisce il file Modeler SPSS:

```
@ApiResponse(code = 200)
```
{: codeblock}

Errore. Un file archiviato in questo ID non esiste nel repository dell'istanza del servizio:

```
@ApiResponse(code = 404)
```
{: codeblock}

Altro errore. Restituito JSON dell'eccezione:

```
@ApiResponse(code = 500)
```
{: codeblock}
