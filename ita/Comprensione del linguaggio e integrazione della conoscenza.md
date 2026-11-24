# Comprensione del linguaggio e integrazione della conoscenza

Capitolo: [[Comprensione del linguaggio, chatbot e integrazione della conoscenza]]

Utilizzare il linguaggio richiede collegare forme testuali, contesto e informazioni sul mondo. Un sistema può riconoscere regolarità linguistiche senza disporre di una descrizione esplicita e verificabile di tutti i fatti che esprime. La qualità delle risposte dipende quindi sia dalle rappresentazioni apprese sia dalle conoscenze effettivamente disponibili nel compito.

## Ambiguità e contesto

La stessa parola può assumere significati diversi in frasi differenti. Embedding contestuali e attenzione rendono possibile modificare la rappresentazione di un token in funzione della sequenza. Questa capacità aiuta a trattare l'ambiguità, ma non equivale a dimostrare una comprensione identica a quella umana.

Una descrizione del mondo richiede inoltre relazioni fra oggetti e situazioni. In una rappresentazione strutturata, entità e relazioni diventano esplicite, mentre nelle rappresentazioni neurali molte regolarità sono distribuite fra parametri e stati numerici.

## Conoscenza nei parametri e informazioni recuperate

Un modello preaddestrato utilizza ciò che ha appreso nei parametri e il contesto fornito al momento dell'inferenza. Un sistema [[RAG]] aggiunge documenti recuperati appositamente. Aggiornare l'archivio esterno e aggiornare i pesi del modello sono operazioni distinte: una nuova informazione può essere consultabile senza essere stata incorporata nei parametri.

La distinzione aiuta a scegliere come rendere disponibili informazioni specifiche. Non garantisce però che il modello le utilizzi bene: un passaggio pertinente può essere ignorato o interpretato in modo scorretto.

## Memorie e agenti linguistici

[[CoALA]] distingue memoria di lavoro, semantica, episodica e conoscenza procedurale. La memoria di lavoro contiene il contesto attivo; quella episodica conserva esperienze; quella semantica organizza conoscenze sul mondo e sull'agente. Le azioni interne possono recuperare, elaborare o memorizzare informazioni.

Il retrieval legge dalla memoria a lungo termine; il ragionamento modifica il contenuto attivo; l'apprendimento scrive informazione persistente. L'uso di una parola comune come «memoria» non rende identiche queste operazioni.

## Relazioni esplicite e verifica

I [[Knowledge Graphs]] permettono di collegare entità e percorsi, utili per domande che richiedono più passaggi. Un embedding può recuperare contenuti affini, mentre un grafo può rendere disponibili relazioni dichiarate. Sono risorse complementari, con errori e limiti differenti.

Nell'[[Integrazione simbolica e subsimbolica|integrazione fra conoscenza esplicita e modelli neurali]], occorre distinguere i dati disponibili, la loro interpretazione e la conclusione generata. Recuperare una conoscenza corretta non impedisce da solo le [[Allucinazioni dei modelli linguistici|allucinazioni]]: il modello può utilizzarla in modo scorretto o aggiungere affermazioni non supportate.
