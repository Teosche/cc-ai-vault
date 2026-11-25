# Prompting e in-context learning

Capitolo: [[Adattamento, valutazione e applicazioni dei Foundation Models]]

Il prompt è il contesto con cui si orienta un modello verso un compito. Un generatore autoregressivo produce il token successivo in funzione della sequenza già disponibile: istruzioni, documenti ed esempi modificano quindi le condizioni della generazione. Se gli si fornisce soltanto una recensione, può continuarla; per ottenerne il sentiment occorre rendere riconoscibile l'operazione desiderata.

## Definire il comportamento atteso

Una richiesta deve rendere riconoscibili compito ed output atteso. Per esempio: «Classifica la recensione come positiva o negativa. Restituisci soltanto l'etichetta. Recensione: il prodotto è arrivato rotto». L'istruzione definisce l'operazione, le categorie delimitano le risposte e il testo della recensione fornisce il dato da analizzare. Separare questi elementi aiuta il modello a non confondere il contenuto con una nuova istruzione.

La precisione del formato serve soprattutto quando la risposta deve essere elaborata da un programma. Se ci si aspetta un'etichetta, una spiegazione corretta ma aggiunta liberamente può rendere l'output inutilizzabile. Correttezza del contenuto e rispetto del formato sono quindi requisiti distinti, entrambi da verificare.

## Mostrare il compito attraverso gli esempi

Nello zero-shot si descrive il compito senza fornire dimostrazioni. Nel one-shot si mostra una coppia input-output; nel few-shot se ne mostrano diverse. Dopo «servizio impeccabile → positiva» e «consegna mai arrivata → negativa», una nuova recensione compare come elemento da completare secondo la regolarità illustrata.

Questo è il principio dell'in-context learning: gli esempi permettono al modello di ricostruire temporaneamente categorie, formato o relazioni utili. I pesi restano invariati. Se gli esempi vengono rimossi dal contesto, non rimane automaticamente una nuova competenza memorizzata. Nel [[Fine-tuning e adattamento efficiente|fine-tuning]], invece, il confronto con gli output desiderati produce aggiornamenti dei parametri.

La scelta degli esempi conta. Dimostrazioni tutte semplici possono non chiarire come trattare negazioni o giudizi misti; etichette incoerenti possono suggerire la regola sbagliata. Inoltre, gli esempi occupano token e riducono lo spazio disponibile per dati e risposta. «Zero-shot» indica l'assenza di dimostrazioni nel prompt, non l'assenza di un precedente addestramento.

## Scomporre una richiesta complessa

Il Chain-of-Thought introduce passaggi intermedi fra problema e risposta. Invece di tentare subito una conclusione, il sistema può individuare le informazioni necessarie, confrontarle ed eseguire un calcolo. Nel few-shot si possono mostrare anche dimostrazioni di questa scomposizione; nello zero-shot si può richiedere una soluzione per passaggi. L'utilità dipende dal compito: aggiungere testo a una classificazione semplice può soltanto aumentare il costo.

Una sequenza di passaggi plausibili può comunque contenere un errore. Il testo esplicativo non è una registrazione completa del calcolo interno e va controllato come qualsiasi altro output. Anche generare più soluzioni e aggregarle aiuta soltanto se gli errori non si ripetono sistematicamente in tutte.

ReAct estende la scomposizione con azioni e osservazioni. Per confrontare due prodotti, un agente può decidere quali dati mancano, consultare uno strumento, leggere il risultato e formulare una nuova richiesta. L'osservazione fornisce informazioni che non erano presenti nel prompt iniziale. Il ciclo richiede però che l'agente controlli il risultato dell'azione prima di usarlo.

## Scegliere come affrontare il problema

La scomposizione non richiede sempre la stessa procedura. In una risposta Direct il sistema produce direttamente il risultato; con Chain-of-Thought esplicita passaggi intermedi. Plan-Exec separa invece la costruzione di un piano dall’esecuzione dei suoi passi. Per confrontare documenti, il piano può stabilire prima quali aspetti cercare e poi organizzare il lavoro su ciascuno.

Reflection aggiunge una revisione del risultato o dei passaggi precedenti: il sistema identifica una possibile incongruenza e tenta una correzione. ReCode traduce invece una parte del problema in codice eseguibile o in una procedura simbolica. Se il compito richiede di sommare valori, eseguire il calcolo fornisce un controllo diverso dal generare un’altra spiegazione linguistica. ReAct collega l’elaborazione a strumenti e osservazioni durante la soluzione, anziché limitarsi a una sequenza di ragionamenti nel contesto.

Scegliere una di queste procedure è a sua volta una decisione. Una strategia adatta al calcolo può essere inutile per una domanda semplice, mentre un problema con dati mancanti può richiedere ricerca. Il routing seleziona la procedura in funzione del compito; l’affidabilità di questa selezione deve essere misurata separatamente. Il fatto che un modello sappia usare una strategia non implica che sappia scegliere quando convenga usarla.

## Valutare una formulazione

Un prompt va provato su un insieme di casi nuovi, includendo quelli ambigui. Modificarlo finché funziona su un solo esempio può adattarlo a quel caso senza migliorare il compito complessivo. Documenti recuperati tramite [[RAG]] possono fornire evidenze, ma bisogna verificare che la risposta le utilizzi, oltre a rispettare istruzioni e formato.
