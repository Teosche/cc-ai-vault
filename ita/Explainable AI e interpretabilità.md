# Explainable AI e interpretabilità

Capitolo: [[Explainable AI, etica e bias]]

La Explainable AI cerca di rendere comprensibile il comportamento dei sistemi artificiali. Una previsione indica quale risultato è stato prodotto; una spiegazione cerca informazioni utili a comprenderne le ragioni, individuare errori o decidere se fidarsi del sistema nel caso considerato. La domanda da porre viene prima del metodo: spiegare una singola decisione e descrivere una regolarità generale richiedono analisi diverse.

## Spiegazioni locali e globali

Una spiegazione locale riguarda un input o una piccola regione dello spazio delle previsioni. Una mappa può mostrare quali pixel risultano rilevanti per la classe «ape» in una certa immagine. Una spiegazione globale cerca invece regolarità del modello: per esempio, se i fiori sostengano spesso quella classe in molti casi.

Passare dal locale al globale richiede campioni e aggregazioni. Una singola immagine non rappresenta l'intera distribuzione e una media può nascondere sottogruppi con comportamenti diversi. La domanda «perché questa previsione?» non si risolve automaticamente con una classifica media delle caratteristiche.

## Struttura leggibile e analisi post-hoc

Un modello intrinsecamente interpretabile ha una struttura che rende relativamente diretto esaminare le relazioni usate. In una regressione lineare si possono leggere coefficienti e contributi, tenendo però conto della scala e delle correlazioni delle variabili. In una rete profonda, più trasformazioni combinano l'informazione e una lettura equivalente diventa difficile.

Un metodo post-hoc analizza il modello dopo l'addestramento. Può misurare cosa accade cambiando un input, esaminare gradienti o costruire esempi sintetici. Alcuni metodi sono specifici dell'architettura; altri usano soltanto input e output e possono essere applicati a famiglie differenti. La scelta dipende quindi anche dall'accesso disponibile: un'API che restituisce punteggi non offre necessariamente gradienti o pesi.

## A quale «perché» si sta rispondendo

Dire che una regione ha un gradiente alto significa che piccole variazioni dei suoi valori influenzano localmente il punteggio. Dire che oscurarla riduce la confidenza descrive invece l'effetto di una perturbazione più grande. Dire che un concetto compare spesso in immagini sintetiche descrive un'altra analisi ancora. Le risposte possono essere complementari senza misurare la stessa proprietà.

Una spiegazione utile deve quindi essere fedele all'oggetto analizzato, comprensibile al destinatario e verificabile. Un testo convincente generato per giustificare una decisione non basta se descrive ragioni scollegate dal comportamento. Anche una spiegazione fedele di un modello può rivelare un criterio indesiderato: comprensibilità e correttezza sono qualità distinte.

## Spiegare un agente

Per un agente, il risultato dipende da più componenti. Una richiesta può essere interpretata bene ma tradotta in un piano sbagliato; il piano può essere adeguato ma uno strumento può fallire. Occorre ricostruire il percorso fra obiettivo, decisioni, chiamate e osservazioni, oltre ad analizzare il modello neurale.

Se l'agente dichiara che un documento è stato aggiornato, il controllo dovrebbe trovare sia la chiamata eseguita sia il suo esito. Questo rende localizzabile l'errore. I [[Metodi di interpretabilità dei modelli]] approfondiscono invece aspetti della previsione; l'[[Etica, fairness e accountability nell’IA|accountability]] collega la ricostruzione del comportamento alla responsabilità delle decisioni.
