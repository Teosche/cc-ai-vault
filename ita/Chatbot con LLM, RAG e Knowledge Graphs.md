# Chatbot con LLM, RAG e Knowledge Graphs

Capitolo: [[Comprensione del linguaggio, chatbot e integrazione della conoscenza]]

Un chatbot basato su LLM combina un modello generativo con la gestione di messaggi, contesto e risorse informative. La sola capacità di continuare testo non specifica come conservare la conversazione, recuperare documenti o verificare le risposte. L'architettura del sistema determina quali informazioni siano disponibili a ogni turno.

## Dal messaggio alla ricerca

Una domanda conversazionale può dipendere da riferimenti precedenti: «e nell'altro anno?» non è una query autosufficiente. La riscrittura della query rende esplicita l'esigenza informativa utilizzando il contesto della conversazione. Non dovrebbe modificare il significato della richiesta per renderla più semplice da cercare.

Il retrieval può combinare ricerca lessicale e semantica. Un reranker riordina una selezione di candidati valutandone la rilevanza rispetto alla domanda. Questa fase è distinta dalla generazione della risposta e può fallire anche se il modello generativo è competente.

## Costruire il contesto della risposta

Il sistema presenta al generatore la domanda e i passaggi recuperati. La segmentazione dei documenti influenza ciò che può essere trovato: frammenti troppo piccoli perdono contesto, mentre documenti condensati in un solo vettore possono nascondere dettagli specifici.

La generazione deve collegare la risposta alle evidenze. Citazioni e delimitazione dell'incertezza aiutano a verificare il risultato, ma non sostituiscono il controllo della corrispondenza fra testo e fonti.

Il [[RAG]] seleziona il contesto documentale in funzione della richiesta. Nel [[CAG]], invece, un insieme delimitato di documenti viene precaricato e il suo stato di elaborazione viene riutilizzato per più domande. Questa scelta modifica il modo di rendere disponibili le informazioni, mentre resta necessario controllare la fedeltà delle risposte ai documenti.

## Domande composte e grafi

Alcune richieste richiedono più ricerche indipendenti; altre richiedono passi successivi, nei quali il primo risultato determina la query seguente. La distinzione fra multi-query e multi-hop riguarda la dipendenza fra le ricerche.

Un Knowledge Graph può fornire percorsi espliciti fra entità e sostenere operazioni come filtri, ordinamenti e aggregazioni. Non è necessario trattarlo come sostituto universale della ricerca su testi: la scelta dipende dalla struttura della domanda e dalla qualità del grafo.

## Memoria, strumenti e rinforzo

Un chatbot può accedere a memoria di conversazione e strumenti esterni. Il ciclo di azione e osservazione consente di utilizzare risultati aggiornati, mentre la conoscenza procedurale controlla come gli strumenti vengono chiamati.

L'RLHF può orientare il modello a produrre risposte preferite secondo un segnale di ricompensa. Non implementa di per sé il retrieval e non garantisce che la risposta consulti correttamente il grafo. Adattamento del modello e disponibilità di evidenze sono parti diverse del sistema.

## Valutazione delle componenti

Bisogna verificare pertinenza dei documenti, fedeltà della risposta, supporto delle citazioni e utilità per l'utente. Una risposta scorrevole non dimostra che il recupero funzioni, così come un documento corretto non dimostra che il generatore lo abbia utilizzato fedelmente. Le [[Metriche di valutazione dei Foundation Models|metriche di valutazione]] devono distinguere questi aspetti, così da localizzare i problemi nel recupero o nella generazione.
