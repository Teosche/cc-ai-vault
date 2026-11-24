# Obiettivi di pre-training linguistico

Capitolo: [[Architettura Transformer e Foundation Models]]

Il pre-training costruisce parametri riutilizzabili facendo risolvere al modello un compito su grandi quantità di dati. Nel linguaggio, il segnale può essere ricavato dal testo stesso: alcune parti vengono nascoste o usate come obiettivo di previsione. Questa impostazione collega il preaddestramento al [[Self-supervised learning]].

## Masked language modeling

Nel masked language modeling si selezionano posizioni della sequenza e si chiede di ricostruire i token originali. Il contesto disponibile può comprendere elementi sia precedenti sia successivi alla posizione selezionata. Il compito favorisce così la costruzione di rappresentazioni contestuali bidirezionali.

Nello schema BERT considerato, il 15% delle posizioni è selezionato: di queste, l'80% viene sostituito da un token di maschera, il 10% da token casuali e il 10% resta invariato. «Posizione scelta per la previsione» non equivale quindi sempre a «posizione contenente [MASK]». La distinzione impedisce di interpretare erroneamente le percentuali come quote aggiuntive dell'intera sequenza.

Il next sentence prediction è un obiettivo ulteriore nello schema BERT originario: il sistema deve distinguere coppie di frasi successive da coppie non successive. Non è una componente obbligatoria di ogni modello encoder o di ogni preaddestramento linguistico.

## Modellazione autoregressiva

La modellazione autoregressiva fattorizza la probabilità di una sequenza come prodotto di probabilità condizionate ai token precedenti:

$$
p(x_1,\ldots,x_T)=\prod_{t=1}^{T}p(x_t\mid x_1,\ldots,x_{t-1}).
$$

Questa è una formalizzazione didattica del compito di previsione successiva. Il modello impara assegnando probabilità al token corretto dato il prefisso. Durante la generazione il prefisso viene ampliato con i token prodotti; durante l'addestramento una maschera impedisce di consultare il futuro.

Il termine «causale» indica qui il vincolo di accesso alle posizioni della sequenza. Non identifica il modello con un sistema di [[Inferenza causale]] sugli effetti di interventi nel mondo.

## Ricostruzione di sequenze corrotte

Un encoder-decoder può ricevere una sequenza alterata e generare quella originale. Le corruzioni possono includere mascheramento, eliminazione di token, sostituzione di intervalli o permutazione di frasi. L'obiettivo richiede di ricostruire contenuto e struttura utilizzando la rappresentazione dell'input modificato.

La ricostruzione di una sequenza non identifica il modello con un [[Variational Autoencoders|VAE]]: condividere componenti chiamati encoder e decoder non rende identiche le architetture o gli obiettivi.

## Pre-training e comportamento finale

Predire token non significa automaticamente seguire istruzioni o verificare fatti. Il compito di apprendimento rende disponibili regolarità utili; l'uso in un'applicazione può richiedere adattamento e valutazione ulteriori. La distinzione è sviluppata in [[Prompting e in-context learning]] e [[Metriche di valutazione dei Foundation Models]].
