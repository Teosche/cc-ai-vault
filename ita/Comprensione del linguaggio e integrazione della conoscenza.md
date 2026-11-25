# Comprensione del linguaggio e integrazione della conoscenza

Capitolo: [[Comprensione del linguaggio, chatbot e integrazione della conoscenza]]

Comprendere il linguaggio significa costruire una rappresentazione utile di ciò che viene comunicato: entità, azioni, relazioni, condizioni e intenzione della richiesta. Il significato dipende anche dal contesto. In «Anna ha parlato con Lucia del suo progetto», il possessivo richiede un'interpretazione; riconoscere le singole parole non risolve da solo l'ambiguità.

## Rappresentare significati e relazioni

Le rappresentazioni contestuali di un modello neurale cambiano con la frase. Questo aiuta a distinguere usi diversi di una parola e a collegare espressioni linguisticamente differenti. Un embedding può per esempio avvicinare «responsabile del laboratorio» a «chi dirige il laboratorio», favorendo la ricerca.

La similarità non specifica però ogni relazione. «Anna supervisiona Lucia» e «Lucia supervisiona Anna» condividono quasi tutte le parole, ma attribuiscono ruoli opposti. Una rappresentazione strutturata conserva soggetto, relazione e oggetto, rendendo esplicito ciò che una query deve verificare. Per questo la comprensione va collegata al problema della rappresentazione della conoscenza.

## Dove può trovarsi un'informazione

La conoscenza parametrica è incorporata nei pesi attraverso l'addestramento. Permette risposte senza consultazione di un archivio, ma non offre automaticamente un elenco controllabile dei fatti memorizzati. La conoscenza contestuale è il materiale presente nell'input corrente, come messaggi precedenti e documenti forniti. La conoscenza esterna rimane invece in un archivio e deve essere recuperata per diventare disponibile alla generazione.

Supponiamo che cambi il responsabile di un laboratorio. Aggiornare la relativa scheda nell'archivio permette a un [[RAG]] di recuperare il nuovo nome senza modificare i pesi. Il sistema deve però selezionare la scheda aggiornata e non confonderla con una versione precedente. L'aggiornabilità deriva dall'organizzazione dell'accesso, non dalla sola capacità linguistica.

## Memorie con funzioni differenti

In [[CoALA]], la memoria di lavoro contiene ciò che è attivo nel ciclo decisionale: richiesta, osservazioni, fatti recuperati e risultati intermedi. La memoria episodica conserva esperienze, come una precedente azione e il suo esito. Quella semantica contiene conoscenze sul mondo e sull'agente. La memoria procedurale riguarda come operare, attraverso conoscenza nei pesi e procedure esplicite nel codice.

Per chiedere nuovamente il responsabile del laboratorio, l'agente può recuperare il fatto dalla memoria semantica. Per evitare una ricerca già fallita, può usare un episodio. Per scegliere e chiamare lo strumento corretto, usa una procedura. Il retrieval legge dalla memoria persistente; il ragionamento elabora la memoria di lavoro; l'apprendimento scrive informazione che potrà essere riutilizzata.

## Dalla conoscenza alla risposta

Un [[Knowledge Graphs|Knowledge Graph]] può rappresentare «Anna appartiene al laboratorio L» e «Marco dirige L». La domanda «chi dirige il laboratorio di Anna?» richiede di identificare L e poi il suo responsabile. Una risposta corretta deve rispettare le due relazioni, non soltanto recuperare nomi affini alla domanda.

Ogni interfaccia può introdurre errori: un nome ambiguo può essere associato all'entità sbagliata, un fatto può essere obsoleto o una risposta può aggiungere una motivazione assente. Integrare conoscenza significa quindi coordinare acquisizione, rappresentazione, recupero e inferenza, mantenendo controllabile come una conclusione dipenda dai dati.
