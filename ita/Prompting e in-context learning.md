# Prompting e in-context learning

Capitolo: [[Adattamento, valutazione e applicazioni dei Foundation Models]]

Il prompting definisce il contesto attraverso cui si richiede a un modello un comportamento: istruzioni, informazioni, esempi e formato della risposta. Un modello generativo continua una sequenza condizionandosi su questo input. Formulare una richiesta non modifica necessariamente i parametri del modello.

## Istruzioni e dimostrazioni

Una richiesta deve rendere riconoscibili compito e output atteso. Fornire una recensione senza specificare l'operazione desiderata non equivale a chiedere di classificarne il sentiment. L'istruzione può indicare le categorie e il formato, distinguendo il testo da analizzare dalla risposta da produrre.

Nello zero-shot il contesto non include dimostrazioni del compito; nel one-shot ne include una e nel few-shot più di una. Le dimostrazioni condizionano la risposta mostrando relazioni fra input e output. Il modello rimane preaddestrato: «zero-shot» non significa che non abbia mai ricevuto dati o che sia privo di conoscenze pregresse.

L'in-context learning va distinto dal [[Fine-tuning e adattamento efficiente]]. Gli esempi nel contesto influenzano l'inferenza senza aggiornare i pesi; il fine-tuning utilizza dati per modificare parametri. Non basta usare il termine «apprendimento» per assimilare i due processi.

## Decomposizione e controllo della risposta

Un compito complesso può essere suddiviso in sottoproblemi. Pianificare i passaggi, fornire contesto pertinente e rivedere l'output sono modi per organizzare il lavoro del sistema. Vincoli sul formato possono rendere la risposta più utilizzabile, ma non garantiscono che sia vera.

La richiesta di passaggi intermedi è collegata alle tecniche Chain-of-Thought. Una sequenza di spiegazioni linguistiche può aiutare a organizzare una risposta, ma non costituisce automaticamente una dimostrazione né una descrizione fedele dei meccanismi interni. Errori e scorciatoie possono permanere anche in risposte lunghe.

Nel ciclo ReAct, l'elaborazione si alterna con azioni e osservazioni: il sistema può consultare uno strumento e utilizzare il risultato nel passo successivo. La correzione dipende allora anche dai dati restituiti e dall'interpretazione del feedback, non soltanto dal prompt iniziale.

## Contesto e valutazione

Un prompt può includere documenti recuperati, come in [[Chatbot con LLM, RAG e Knowledge Graphs]]. Il contesto esterno fornisce evidenze disponibili per la generazione, ma va verificato se la risposta le utilizzi correttamente.

La qualità di una formulazione si valuta sul compito e su esempi rappresentativi. Migliorare un singolo risultato mediante tentativi successivi non dimostra che la stessa richiesta funzioni su tutti i casi. Accuratezza, rispetto dei vincoli e supporto delle affermazioni restano criteri distinti.
