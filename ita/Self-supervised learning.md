# Self-supervised learning

Capitolo: [[Paradigmi di apprendimento]]

Il self-supervised learning ricava dai dati il target rispetto al quale addestrare il modello. Il testo «il gatto dorme» contiene già la risposta al compito «prevedi la parola dopo il gatto»: non serve annotare manualmente una categoria. Una loss confronta comunque previsione e target e produce aggiornamenti dei pesi. L'assenza di etichette aggiunte da un annotatore non significa assenza di supervisione o di scelte progettuali.

## Predire parti mancanti o future

Nel masked language modeling si nasconde una parte dei token e si chiede di ricostruirla utilizzando il contesto disponibile. In «ho lasciato le chiavi sul [MASK]», le parole prima e dopo la posizione possono contribuire alla previsione. Per riuscirci su molti esempi, il modello deve apprendere regolarità sintattiche e semantiche.

Nel preaddestramento di BERT viene selezionato il 15% delle posizioni: fra queste, l'80% è sostituito con il token di mascheramento, il 10% con un token casuale e il 10% resta invariato. Le posizioni selezionate rimangono quelle su cui si calcola l'obiettivo. La presenza occasionale di token non mascherati evita che l'apprendimento dipenda esclusivamente da un simbolo assente nell'uso normale.

Nel causal language modeling si predice invece ciascun token usando soltanto quelli precedenti. Una maschera impedisce di vedere il futuro, che renderebbe banale il compito. Questo obiettivo è direttamente collegato alla generazione autoregressiva: durante l'inferenza, il modello riceve un prefisso e lo prolunga.

Un encoder-decoder può essere addestrato al denoising: riceve una sequenza corrotta da cancellazioni, mascheramenti o permutazioni e ricostruisce l'originale. La struttura del compito cambia ciò che conviene apprendere, come approfondito negli [[Obiettivi di pre-training linguistico]].

## Apprendere somiglianze attraverso il contrasto

Nell'apprendimento contrastivo si costruiscono coppie positive e negative. Due viste ottenute dalla stessa immagine possono formare una coppia positiva; immagini diverse forniscono negativi. L'obiettivo avvicina le rappresentazioni dei positivi e rende distinguibili quelle dei negativi. Le trasformazioni definiscono quali variazioni il modello dovrebbe ignorare: un ritaglio moderato può conservare il soggetto, uno eccessivo può eliminarlo.

In CLIP si usano coppie immagine-testo. Con un batch di tre coppie si confronta ogni immagine con tutti e tre i testi: le tre corrispondenze originarie sono positive, le sei combinazioni incrociate fungono da negativi impliciti. La loss considera entrambe le direzioni, immagine verso testo e testo verso immagine, costruendo uno spazio in cui modalità diverse siano confrontabili.

Le associazioni immagine-descrizione costituiscono già una forma di supervisione naturale e possono essere rumorose. Inoltre, due immagini diverse possono avere descrizioni semanticamente equivalenti: un negativo costruito dal batch non è necessariamente un opposto concettuale. Il metodo va descritto attraverso l'origine effettiva delle coppie, non soltanto con l'etichetta «contrastivo».

## Riutilizzare ciò che è stato appreso

Le rappresentazioni possono essere congelate per addestrare una testa oppure adattate con [[Fine-tuning e adattamento efficiente|fine-tuning]]. Il vantaggio è non dover imparare ogni regolarità da pochi dati annotati. Il trasferimento resta da verificare: un obiettivo che ricostruisce bene il testo può apprendere anche stereotipi e correlazioni poco utili per il compito finale.
