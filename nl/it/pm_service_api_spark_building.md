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

# Creazione di applicazioni di analisi predittiva


Questa sezione descrive il processo per la distribuzione e l'utilizzo di un'applicazione di esempio.

**Nome scenario**: Previsione linea di prodotti.

**Descrizione scenario**: il nostro cliente sta realizzando una delle più famose catene di negozi
in Europa. Ci richiede di scoprire gli interessi dei propri clienti in merito alla propria linea di prodotti,
come Accessori personali, Attrezzature da campeggio e Protezione all'aperto.
Un data scientist sviluppa un modello predittivo e lo condivide con te (lo sviluppatore). Il tuo compito è quello di
preparare e distribuire l'applicazione Node.js che consiglia linee di prodotti sportivi effettuando
richieste di punteggio sul modello distribuito.

1. Accedi a Bluemix e crea un'istanza di Machine Learning.

2. Avvia il Dashboard Machine Learning.

3. Vai alla scheda Esempi.

4. Nella sezione Modelli di esempio, cerca il tile Previsione linea di prodotti
   e fai clic sul pulsante Aggiungi modello (+). Adesso vedrai il
   modello di esempio Previsione linea di prodotti nell'elenco di modelli disponibili sulla
   scheda Modelli.

   **Nota**: se anziché gli esempi, vuoi utilizzare i tuoi propri progetti e modelli
   di Data Science Experience, devi rendere persistente un modello personalizzato
   nel repository Machine Learning. Per farlo, puoi utilizzare
l'API REST o le librerie del client. Per i
   dettagli, vedi Sviluppo e persistenza del modello personalizzato.

5. Dal menu Azione, seleziona Crea distribuzione per distribuire
   il modello Previsione linea di prodotti come distribuzione online.

6. Specifica il nome della distribuzione (ad esempio, Previsione linea
   di prodotti) e fai clic su Salva. Dovresti ora visualizzare la distribuzione Previsione linea di
   prodotti nell'elenco Distribuzioni.

7. Dal menu Azione, seleziona Visualizza dettagli per la tua distribuzione.
   L'Endpoint di punteggio presentato nel riquadro dei dettagli sarà utilizzato per
   eseguire le richieste di calcolo di punteggio.

8. Per eseguire le richieste di calcolo di punteggio attraverso l'API REST, sono necessari un endpoint di punteggio e un token
di autorizzazione. Per ulteriori informazioni, vedi
   Distribuzione di modelli online.

9. Puoi ora sperimentare l'applicazione Node.js di esempio
   disponibile all'indirizzo
   https://github.com/pmservice/product-line-prediction.

   Per distribuire automaticamente il codice dell'applicazione di esempio al tuo
   spazio Bluemix, vai alla scheda Esempi e, nella sezione
   Applicazioni di esempio, seleziona l'applicazione Previsione linea di
   prodotti e distribuiscila facendo clic sul pulsante (+).
   Se richiesto, effettua l'autenticazione nel modulo DeployToBluemix.

   Dovresti ora vedere l'applicazione di esempio sul Dashboard Bluemix
   nella sezione Tutte le applicazioni.

10. Fai clic sull'applicazione per vederne i dettagli. Qui puoi modificare i dettagli dell'applicazione, come ad esempio il
numero di istanze, la memoria ecc.

11. Puoi ora eseguire il bind della tua applicazione con l'istanza Machine
    Learning. Nella scheda Connessioni, fai clic su Connetti esistente.
    Al termine della
ripreparazione, la tua applicazione viene connessa all'istanza del servizio.

12. Esegui l'applicazione utilizzando l'indirizzo della rotta.

13. Nell'applicazione, seleziona la tua distribuzione Previsione linea di prodotti. Sei ora pronto a utilizzare
i record di esempio e i risultati di previsione.
