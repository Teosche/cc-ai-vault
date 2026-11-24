# Quantizzazione LoRA e QLoRA

Capitolo: [[Adattamento, valutazione e applicazioni dei Foundation Models]]

Un modello può essere reso più piccolo da conservare oppure più economico da adattare, ma sono obiettivi diversi. La quantizzazione riduce la precisione con cui vengono rappresentati i valori. LoRA riduce quanti parametri devono essere appresi durante l'adattamento. QLoRA combina adattatori LoRA con una base quantizzata durante il training. Nessuna di queste espressioni indica, da sola, un'architettura alternativa a Transformer o DeltaNet.

## Parametri e precisione numerica

In [[Qwen3.5 4B architettura e funzionamento]], 4B indica la scala del numero di parametri. Un modello con quattro miliardi di pesi richiede idealmente 16 GB a 32 bit, 8 GB a 16 bit, 4 GB a 8 bit e 2 GB a 4 bit. Sono GB decimali dei soli pesi, calcolati su quattro miliardi esatti: non comprendono scale, metadati, cache e memoria di lavoro.

La quantizzazione associa valori originariamente più precisi a un insieme più ristretto di rappresentazioni. Quattro bit permettono 16 codici. Per ricostruire il valore approssimato servono informazioni come una scala e, in alcuni schemi, un riferimento dello zero. Un valore 0,123 potrebbe essere ricostruito come 0,10: è un esempio didattico dell'errore introdotto, non una regola specifica del file Qwen.

Distribuzioni differenti dei pesi richiedono scale adeguate. La quantizzazione a blocchi raggruppa i valori e conserva dati ausiliari per ricostruirli. Questo migliora l'adattamento locale, ma aggiunge memoria: quattro bit nominali non significano sempre quattro bit effettivi contando l'intero contenitore.

## Che cosa contiene un GGUF Q4 K M

GGUF è un formato che conserva tensori e metadati necessari a interpretarli. Q4_K_M identifica invece una ricetta di quantizzazione mista. Alcune matrici utilizzano meno bit, altre vengono mantenute a precisione superiore per limitare gli errori. Due file con la stessa sigla possono differire per conversione, componenti incluse e scelte del quantizzatore.

Il GGUF Qwen3.5-4B locale occupa circa 2,71 GB e contiene circa 4,206 miliardi di parametri. Fra i 426 tensori sono presenti codifiche Q4_K, Q5_K, Q6_K e FP32. I numerosi tensori FP32 comprendono anche vettori piccoli: contare i tensori non equivale a misurare la loro quota di memoria. La matrice di embedding è conservata in Q6_K.

La quantizzazione può migliorare velocità e possibilità di caricamento perché riduce i dati da trasferire e permette di conservare più pesi nella VRAM. Il risultato dipende però dai kernel del runtime. Pesi a quattro bit non implicano che attivazioni, accumulazioni e stati siano tutti a quattro bit. Una quantizzazione più aggressiva può cambiare token scelti, citazioni e numeri: la perdita di qualità va misurata sul compito.

## LoRA e adattamento a basso rango

LoRA significa Low-Rank Adaptation ed è un metodo di [[Fine-tuning e adattamento efficiente|adattamento efficiente]]. Invece di aggiornare un'intera grande matrice di pesi, mantiene la matrice originale congelata e apprende una correzione rappresentata attraverso due matrici più piccole. Il loro prodotto ricostruisce una variazione con la stessa forma della matrice base, ma con libertà limitata dal rango.

Il rango indica la dimensione intermedia della correzione, non il numero di blocchi modificati e neppure la precisione numerica. Con rango 8, una trasformazione fra 2.560 e 9.216 componenti richiede 94.208 coefficienti aggiuntivi, contro i 23.592.960 della matrice completa. È la correzione a essere a basso rango: la matrice base può mantenere la propria struttura.

Durante l'addestramento si confrontano output previsti e desiderati e si aggiornano i parametri dell'adattatore. Il modello base viene comunque eseguito, perché serve a costruire le rappresentazioni. Congelare i suoi pesi evita di aggiornarli, ma non elimina tutto il costo delle attivazioni e della retropropagazione.

Le matrici da adattare sono una scelta progettuale: possono riguardare proiezioni dell'attenzione e feed-forward. Un adattatore può essere mantenuto separato e caricato con la base oppure incorporato nei pesi quando la procedura lo consente. L'effetto dipende dai dati: LoRA può insegnare un formato o un comportamento, ma non garantisce automaticamente nuove capacità di ragionamento.

## QLoRA e base quantizzata durante il training

QLoRA riduce ulteriormente la memoria mantenendo la base congelata in una rappresentazione quantizzata, mentre addestra gli adattatori LoRA. Il modello base non viene aggiornato direttamente attraverso i suoi codici a quattro bit. I kernel utilizzano valori ricostruiti nelle operazioni previste, e il segnale di apprendimento raggiunge gli adattatori.

La ricetta originale introduce NF4, una codifica a quattro bit con livelli non uniformi adatti a pesi con distribuzioni approssimativamente normali; la doppia quantizzazione, che riduce anche la memoria delle costanti di quantizzazione; e ottimizzatori paginati, che aiutano a gestire picchi di memoria. Questi accorgimenti non rendono ogni operazione a quattro bit e non eliminano i requisiti di memoria del training.

| Procedura | Base | Che cosa viene appreso |
| :--- | :--- | :--- |
| Fine-tuning completo | Aggiornabile | Tutti i pesi previsti dalla ricetta |
| LoRA | Congelata | Le matrici dell'adattatore |
| QLoRA | Congelata e quantizzata | Le matrici dell'adattatore |
| Quantizzazione per inferenza | Convertita a minore precisione | Nessun nuovo adattamento implicito |

## Il caso di VERA

L'adattatore sperimentale di VERA utilizza rango 8 e alpha 16. L'obiettivo è migliorare il contratto di risposta: JSON valido, citazioni letterali, decisioni di ambito e distinzione fra evidenze e inferenze. La ricetta documentata ha eseguito LoRA con base FP32 distribuita su due T4. Non è quindi corretto chiamarla QLoRA soltanto perché l'adattatore può essere successivamente utilizzato con un GGUF quantizzato.

Il miglioramento su uno schema di output misura la conformità alle regole valutate, non dimostra che ogni analisi sia fattualmente corretta. Applicare un adattatore a una base già quantizzata può inoltre produrre risultati numericamente differenti dall'incorporarlo prima della quantizzazione. Citazioni e correttezza devono essere valutate sulla configurazione effettivamente eseguita.
