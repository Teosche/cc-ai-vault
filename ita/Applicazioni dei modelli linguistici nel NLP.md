Capitolo: [[Adattamento, valutazione e applicazioni dei Foundation Models]]

Un modello linguistico può essere usato come produttore di testo oppure come costruttore di rappresentazioni. Nel primo caso genera direttamente una risposta; nel secondo trasforma parole o documenti in vettori utilizzati da classificatori e sistemi di ricerca. La scelta dipende dal risultato richiesto, dai dati disponibili e dal costo dell'elaborazione.

## Classificare un testo

Consideriamo una recensione da assegnare a «positiva» o «negativa». Un encoder produce rappresentazioni contestuali: il significato di «buono» in «non è buono» dipende dalle parole circostanti. Una testa di classificazione trasforma una rappresentazione dell'intero testo in punteggi per le categorie. Con fine-tuning si possono aggiornare anche le rappresentazioni; con un encoder congelato si apprende soltanto il classificatore.

Un'alternativa usa embedding di frasi già adatti al confronto semantico. Si possono fornire a un classificatore separato oppure confrontare con rappresentazioni delle descrizioni delle classi. In quest'ultimo caso una descrizione come «recensione che esprime insoddisfazione» funge da riferimento semantico. La formulazione delle categorie diventa parte del metodo e può influenzare il risultato.

Un modello generativo può invece ricevere la recensione e produrre l'etichetta come testo, in zero-shot o few-shot. Questo rende flessibile la definizione del compito, ma introduce anche output non validi: «abbastanza negativa» non coincide necessariamente con una delle etichette ammesse. Prima della valutazione bisogna decidere come trattare questi casi.

## Assegnare un ruolo ai singoli token

Altri compiti richiedono una previsione per ciascuna posizione anziché una sola categoria per l'intero testo. Nel riconoscimento di entità, il sistema individua persone, luoghi e organizzazioni e deve ricostruirne i confini anche quando un nome occupa più token. Nell'analisi delle parti del discorso assegna invece ruoli grammaticali, come nome o verbo. Nel question answering estrattivo può stimare le posizioni iniziale e finale del segmento che risponde alla domanda. Le rappresentazioni contestuali condividono l'informazione della frase, mentre la testa di uscita rispecchia il compito.

## Generare e trasformare sequenze

Nella generazione autoregressiva, il modello distribuisce probabilità sui possibili token successivi e ne seleziona uno alla volta. Ogni scelta diventa contesto per le successive. La procedura di selezione può favorire continuazioni più prevedibili oppure più varie; una maggiore varietà non implica maggiore correttezza.

La traduzione richiede una relazione più vincolata fra input e output. In un encoder-decoder, l'encoder rappresenta il testo di origine, mentre il decoder genera la traduzione considerando sia ciò che ha già prodotto sia le rappresentazioni di origine tramite cross-attention. Per tradurre un pronome o una parola ambigua serve quindi conservare il contesto, non sostituire ogni parola in modo indipendente.

La formulazione testo-testo estende questo meccanismo: una richiesta può chiedere di tradurre, correggere o riassumere. Il formato esterno è simile, ma cambia ciò che rende valida la risposta. Una traduzione deve preservare il significato; una correzione deve modificare gli errori senza alterare il contenuto; un riassunto deve selezionare le informazioni centrali.

## Riassumere senza perdere il contenuto

Un riassunto estrattivo seleziona frasi del documento e le assembla. Un riassunto generativo riformula le informazioni in una nuova sequenza. Il primo può mantenere formulazioni fedeli ma risultare poco coeso; il secondo può collegare meglio le idee, introducendo però il rischio di dettagli non presenti.

Se un testo afferma che una procedura è valida soltanto per una versione del prodotto, omettere tale condizione cambia il significato. La qualità del riassunto dipende quindi anche dalle relazioni e dai vincoli conservati, non soltanto dalla brevità o dalla somiglianza delle parole.

## Cercare informazioni e rispondere

La ricerca semantica usa embedding per recuperare passaggi pertinenti anche quando domanda e documento impiegano vocaboli diversi. La vicinanza fra vettori seleziona candidati; un reranker può valutarne meglio il rapporto con la domanda. La risposta viene poi costruita dal generatore usando i testi selezionati, come nel [[RAG]].

Nel question answering estrattivo si individua un segmento del documento che contiene la risposta; in quello generativo si formula un nuovo testo, eventualmente combinando più passaggi. Una domanda su «chi collabora con chi» può richiedere relazioni esplicite in un grafo, oltre alla similarità testuale. Le [[Metriche di valutazione dei Foundation Models|verifiche]] devono seguire questa differenza: riconoscere un'etichetta, preservare una traduzione e sostenere una risposta con evidenze non sono lo stesso obiettivo.
