# Applicazioni dei modelli linguistici nel NLP

Capitolo: [[Adattamento, valutazione e applicazioni dei Foundation Models]]

I modelli linguistici possono produrre testo, trasformare una sequenza in un'altra o costruire rappresentazioni utilizzate da classificatori e sistemi di ricerca. La stessa architettura di base può sostenere compiti diversi, ma il formato dell'input e l'obiettivo devono rendere chiara l'operazione richiesta.

## Generazione e continuazione

Un generatore autoregressivo assegna probabilità al token successivo e amplia progressivamente la sequenza. Il contesto può includere una richiesta, esempi o informazioni da utilizzare. La continuazione non è una consultazione garantita di un archivio di fatti: produrre testo plausibile e verificare una proposizione sono operazioni differenti.

L'adattamento alle istruzioni permette di orientare la generazione verso una risposta al compito. Non elimina la necessità di controllare quanto prodotto, soprattutto quando il testo contiene affermazioni sul mondo.

## Traduzione e trasformazioni testo-testo

La traduzione collega una sequenza di origine a una di destinazione. In un encoder-decoder, il decoder usa sia il contesto già generato sia le rappresentazioni dell'encoder. L'attenzione consente di combinare parti pertinenti dell'input durante la produzione dell'output.

La formulazione testo-testo estende il principio a più compiti: l'input descrive un'operazione e il modello produce la sequenza richiesta. Un riassunto richiede di mantenere informazioni centrali senza aggiungere contenuti non sostenuti dal testo di origine. Questa è una condizione di qualità del compito, non un effetto automaticamente garantito dall'architettura.

Un riassunto estrattivo seleziona parti rilevanti del testo di origine, mentre una sintesi generativa produce una nuova sequenza. Le rappresentazioni contestuali possono sostenere la selezione dei passaggi senza richiedere che il modello generi ogni frase del riassunto. I due procedimenti devono essere valutati rispetto alla fedeltà e alle informazioni mantenute.

## Classificazione e rappresentazioni

Nella classificazione, un modello di rappresentazione può produrre vettori utilizzati da una testa specifica o da un classificatore separato. Un modello generativo può invece restituire l'etichetta come testo, per esempio «positivo» o «negativo». Prima di valutare il risultato occorre definire come interpretare output non conformi al formato.

Una classificazione basata su embedding può confrontare il testo con descrizioni delle classi, senza addestrare un nuovo classificatore supervisionato su quel dataset. L'efficacia dipende dalla qualità delle rappresentazioni e dalla formulazione delle categorie.

## Question answering e conoscenza esterna

Una risposta può essere prodotta usando il solo contesto disponibile oppure documenti recuperati appositamente. Nel secondo caso, il sistema deve identificare l'informazione pertinente e poi utilizzarla coerentemente. Il recupero non basta se il generatore trascura i passaggi utili o aggiunge dettagli inventati.

Nel question answering, un sistema [[RAG]] può fornire al generatore i documenti pertinenti alla domanda, sui quali fondare la risposta. Traduzione, riassunto e question answering condividono l'elaborazione di sequenze, ma richiedono verifiche differenti su correttezza, completezza e fedeltà.
