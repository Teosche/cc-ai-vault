Capitolo: [[Comprensione del linguaggio, chatbot e integrazione della conoscenza]]

Un chatbot basato su LLM è un sistema che organizza la conversazione e le risorse disponibili a un generatore. Deve decidere quali messaggi conservare, se cercare informazioni e come trasformare i risultati in una risposta. Il modello linguistico è una componente: una conversazione affidabile dipende anche dal modo in cui gli vengono presentati contesto, strumenti ed evidenze.

## Ricostruire la domanda corrente

Dopo una domanda sui risultati di un'azienda nel 2024, «e l'anno dopo?» richiede di recuperare il riferimento precedente. Cercare soltanto quelle tre parole sarebbe poco utile. La query rewriting può produrre una richiesta autosufficiente che mantenga azienda, indicatore e nuovo anno. Deve esplicitare ciò che era implicito, evitando di inventare un vincolo non espresso.

La memoria conversazionale deve conservare le informazioni utili entro il contesto disponibile. Mantenere ogni messaggio può aumentare costi e rumore; riassumerli può perdere dettagli. In una struttura come [[CoALA]], la memoria di lavoro contiene la richiesta attiva e quanto recuperato, mentre esperienze e conoscenze persistenti vengono conservate separatamente.

## Dal recupero alla risposta

In una pipeline [[RAG]], i documenti sono segmentati e indicizzati prima delle domande. Alla richiesta corrente si possono associare candidati tramite ricerca lessicale e semantica. La prima è utile per identificativi e termini precisi; la seconda può trovare formulazioni diverse dello stesso concetto. Un cross-encoder usato come reranker esamina insieme query e candidato, affinando l'ordine prima di consumare il contesto del generatore.

I testi selezionati vengono inseriti nel prompt con la domanda e le istruzioni. Il generatore deve ricostruire una risposta mantenendo soggetti, numeri e condizioni. Se si chiede la procedura di riavvio di un dispositivo, un passaggio relativo a un altro modello non basta, anche se è semanticamente vicino.

Nel [[CAG]], un insieme delimitato di documenti viene invece precaricato e il suo stato di elaborazione viene riutilizzato tramite KV cache. Non è una raccolta di risposte pronte e non modifica i pesi. Evita il recupero per ogni domanda, ma richiede che i documenti rientrino nel contesto e che la cache rimanga coerente con la loro versione.

## Ricerche multiple e relazioni strutturate

Una domanda che confronta due anni può usare due query indipendenti: entrambe sono definite prima di leggere i risultati. Una domanda sul responsabile del laboratorio di Anna richiede invece di identificare prima il laboratorio e poi cercarne il responsabile. La seconda ricerca dipende dalla prima: è una catena multi-hop.

Un Knowledge Graph rende espliciti tali legami. Se contiene «Anna appartiene a L» e «Marco dirige L», il sistema può attraversare entrambe le relazioni e ottenere Marco. Filtri, conteggi e ordinamenti possono essere eseguiti sulla struttura, anziché affidati a una lettura approssimativa di molti frammenti. Rimangono da controllare identificazione delle entità, verso delle relazioni e completezza del grafo.

## Adattare il comportamento attraverso il feedback

Il reinforcement learning può modificare il comportamento del chatbot in base a una valutazione delle risposte. Nell’RLHF si raccolgono preferenze fra risposte alternative e si addestra un modello di reward a riprodurle. Il generatore viene poi trattato come una policy da ottimizzare per aumentare quel punteggio, per esempio mediante [[Proximal Policy Optimization]]. Il segnale valuta quindi il risultato della generazione, invece di fornire soltanto una risposta da imitare.

Per un assistente documentale si possono preferire risposte pertinenti, aderenti alle evidenze e capaci di riconoscere quando manca un’informazione. Il reward deve però rappresentare effettivamente questi criteri: premiare una risposta convincente senza verificarne il supporto può favorire una sicurezza ingiustificata. Il recupero rende disponibili documenti, il grafo rende interrogabili le relazioni e il rinforzo orienta il comportamento del generatore. Sono componenti complementari e vanno valutate sia separatamente sia nel sistema complessivo.

## Quando il chatbot agisce

Un chatbot con strumenti può decidere di cercare un dato, eseguire una query e leggere l'esito prima di rispondere. Il ciclo azione-osservazione permette di correggere una ricerca fallita e raccogliere informazioni ulteriori. La produzione di una chiamata valida non dimostra però che lo strumento abbia restituito ciò che serviva.

La valutazione segue dunque ogni passaggio: comprensione della richiesta, pertinenza dei dati, correttezza dell'operazione e fedeltà della risposta. Migliorare lo stile del generatore non ripara un archivio incompleto, così come un grafo corretto non impedisce una spiegazione finale inventata.
