# Metodi di embedding

Capitolo: [[Architettura Transformer e Foundation Models]]

Un embedding è una rappresentazione vettoriale di dati, costruita in modo che le relazioni numeriche siano utili per un compito. Nel linguaggio può rappresentare un token, una parola, una frase o un documento. La scelta dell'unità rappresentata è importante: un vettore per parola e un vettore per un intero testo non rispondono alle stesse esigenze.

## Rappresentazioni distribuite

Una rappresentazione basata soltanto sulla presenza o frequenza delle parole distingue i testi mediante il lessico osservato. Metodi come word2vec apprendono invece vettori a partire dai contesti in cui le parole compaiono. **Parole utilizzate in contesti simili possono ottenere rappresentazioni vicine.**

Le coordinate non devono essere interpretate come un elenco di concetti umani chiaramente nominabili. L'informazione è distribuita fra molte dimensioni: la rappresentazione complessiva può risultare utile anche quando una singola coordinata non ha un significato intuitivo.

## Embedding statici e contestuali

Un embedding statico assegna alla stessa parola lo stesso vettore anche quando cambia il contesto. Il termine inglese «bank», per esempio, può indicare una banca oppure la riva di un fiume. Una rappresentazione contestuale utilizza la sequenza per differenziare i due usi.

Nel Transformer occorre distinguere l'embedding iniziale del token dagli stati nascosti successivi. L'embedding iniziale viene consultato attraverso il suo identificativo; i blocchi di attenzione costruiscono poi rappresentazioni che dipendono dalle altre posizioni. Non si tratta di ricalcolare da zero il vocabolario a ogni frase, ma di trasformare i vettori in funzione del contesto.

L'informazione posizionale aggiunge una distinzione ulteriore: identità del token e posizione nella sequenza non sono la stessa cosa. Un contenuto uguale in una posizione diversa può avere un ruolo diverso nell'elaborazione.

## Dalle parole ai documenti

Un modello per embedding di frasi o documenti produce rappresentazioni confrontabili per classificazione, raggruppamento e ricerca semantica. Non ogni stato nascosto di un generatore è automaticamente un buon embedding per la ricerca: il confronto richiede che modello e criterio di similarità siano adatti al compito.

Per testi lunghi, un solo vettore può comprimere eccessivamente le informazioni. Suddividere il documento in passaggi consente di recuperare parti più specifiche, ma impone di conservare contesto sufficiente. Un frammento isolato può risultare ambiguo; titolo, passaggi adiacenti o sovrapposizioni possono aiutare.

## Similarità e multimodalità

Le [[Metriche di similarità]] permettono di confrontare vettori, per esempio attraverso il coseno. Vicinanza vettoriale non significa identità logica, verità o equivalenza in ogni contesto. Indica una relazione appresa rispetto ai dati e all'obiettivo del modello.

In [[LLM e Foundation Models multimodali|CLIP]], encoder di immagini e testi vengono allineati in uno spazio condiviso. Questo permette di confrontare una fotografia con descrizioni testuali: la rappresentazione riguarda così dati di modalità diverse.
