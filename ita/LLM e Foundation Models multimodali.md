# LLM e Foundation Models multimodali

Capitolo: [[Architettura Transformer e Foundation Models]]

Un Foundation Model è un modello preaddestrato su dati ampi e riutilizzabile come base per compiti diversi. L'adattamento può avvenire attraverso ulteriori aggiornamenti dei parametri oppure mediante istruzioni ed esempi nel contesto. «Preaddestrato» descrive la storia dell'apprendimento; non garantisce che il modello segua istruzioni, conosca fatti aggiornati o sia adatto a ogni applicazione.

## Modelli di rappresentazione e modelli generativi

BERT utilizza una struttura encoder-only ed è orientato alla costruzione di rappresentazioni contestuali. Può sostenere classificazione o estrazione di caratteristiche, con una componente di output adeguata al compito. Il suo preaddestramento include la ricostruzione di token selezionati, come spiegato in [[Obiettivi di pre-training linguistico]].

GPT-3 appartiene invece alla famiglia decoder-only, che modella il token successivo in modo autoregressivo. Istruzioni e dimostrazioni possono condizionare la continuazione del testo senza aggiornare i pesi durante l'inferenza. Questo è distinto dal [[Fine-tuning e adattamento efficiente]], nel quale i parametri vengono modificati.

La distinzione fra rappresentazione e generazione è funzionale, non una gerarchia assoluta di capacità. Un generatore può produrre un'etichetta come testo; un encoder può fornire vettori utili a un classificatore. Occorre chiedersi quale output sia necessario e come venga ottenuto.

## CLIP: allineare immagini e linguaggio

CLIP combina un encoder visivo e un encoder testuale. L'addestramento utilizza coppie immagine-testo: per ogni immagine, la descrizione abbinata costituisce il positivo, mentre le altre descrizioni nel batch funzionano come negativi impliciti. Il confronto viene effettuato anche nella direzione testo-immagine.

La funzione contrastiva favorisce l'allineamento delle coppie corrette nello spazio condiviso. Non richiede che il modello generi una descrizione parola per parola: il suo output caratteristico è una rappresentazione confrontabile. CLIP non va quindi trattato come un chatbot soltanto perché usa anche il linguaggio.

Per classificare un'immagine, si possono rappresentare descrizioni delle classi candidate, come «una foto di un cane», e confrontarle con il vettore dell'immagine. La classe con il punteggio più alto diventa la previsione. Lo zero-shot riguarda l'assenza di un nuovo addestramento supervisionato su quelle classi, non l'assenza di dati nel preaddestramento.

## Multimodalità e modalità di fusione

Un sistema multimodale può utilizzare più tipi di input con ruoli differenti. Allineare rappresentazioni in uno spazio condiviso, come CLIP, è diverso dall'inserire rappresentazioni visive nel contesto di un modello linguistico o dal calcolare attenzione congiunta sui token delle modalità. L'architettura determina come l'informazione può essere scambiata.

La coerenza fra modalità non è garantita dalla sola presenza di un encoder per immagini. Un sistema può trascurare dettagli visivi o seguire scorciatoie linguistiche. Il confronto con [[Sfide dell’interazione multimodale]] riguarda la necessità di integrare informazioni pertinenti senza confonderne i ruoli.
