# Qwen3.5 4B architettura e funzionamento

Capitolo: [[Architettura Transformer e Foundation Models]]

Qwen3.5-4B è un modello della famiglia Qwen, sviluppata da Alibaba, che genera testo prevedendo progressivamente i token successivi. Il suo componente linguistico combina un numero relativamente contenuto di parametri con un'architettura ibrida: alcuni blocchi utilizzano la self-attention, altri elaborano la sequenza attraverso una memoria ricorrente. Questa combinazione rende il modello interessante per applicazioni locali come VERA, nelle quali capacità, memoria disponibile e tempo di risposta devono essere bilanciati.

## Che cosa significa 4B

La lettera B indica miliardi di parametri. Un parametro è un valore numerico appreso durante l'addestramento, per esempio un coefficiente di una matrice che trasforma le rappresentazioni interne. Non è una parola, un documento o una regola memorizzata separatamente. Il nome 4B identifica una classe dimensionale arrotondata: il componente linguistico presente nel GGUF locale contiene circa 4,206 miliardi di parametri.

Il numero dei parametri è distinto dalla loro precisione numerica. Lo stesso modello può essere conservato a 16 bit oppure in una variante quantizzata, senza diventare per questo un modello con meno parametri. La sigla Q4_K_M riguarda la rappresentazione dei pesi, mentre 4B riguarda la loro quantità. [[Quantizzazione LoRA e QLoRA]] approfondisce questa distinzione e l'adattamento al compito.

## Come è composto il componente linguistico

Il modello riceve identificatori di token, recupera i relativi embedding e trasforma i vettori attraverso 32 blocchi. La rappresentazione di ogni posizione ha 2.560 componenti. Ogni blocco contiene un meccanismo che combina informazioni della sequenza e una rete feed-forward che applica ulteriori trasformazioni non lineari. Normalizzazioni e connessioni residuali regolano la scala dei valori e permettono di aggiungere nuove trasformazioni conservando un percorso per l'informazione precedente.

| Componente | Caratteristica del 4B |
| :--- | :--- |
| Matrice di embedding | 248.320 righe, ciascuna con 2.560 valori |
| Blocchi linguistici | 32 |
| Blocchi Gated DeltaNet | 24 |
| Blocchi di full attention | 8 |
| Rete feed-forward | Dimensione intermedia di 9.216 |
| Teste nei blocchi di full attention | 16 query e 4 key/value |
| Uscita linguistica | Punteggi sul vocabolario, con pesi condivisi con l'embedding |

L'ordine segue otto gruppi identici: tre blocchi Gated DeltaNet e un blocco di full attention. La full attention si trova quindi nei blocchi 4, 8, 12 e così via fino al 32. Tutte le posizioni attraversano i 32 blocchi: il modello non sceglie di saltarne alcuni in base alla parola ricevuta.

La feed-forward espande temporaneamente il vettore, combina due trasformazioni attraverso una modulazione e lo riporta a 2.560 componenti. Le sue matrici contengono complessivamente circa 2,265 miliardi di parametri. Una parte molto rilevante dei pesi appartiene quindi alle feed-forward, non soltanto all'attenzione.

## Self-attention e recupero dal contesto

La [[Architettura Transformer e meccanismo di attenzione|self-attention]] permette a una posizione di utilizzare informazioni presenti nella stessa sequenza. Dalla rappresentazione corrente vengono calcolati tre tipi di vettori: query, key e value. La query viene confrontata con le key delle posizioni consentite; i punteggi diventano coefficienti con cui combinare i value.

Nel modello causale, una posizione può utilizzare se stessa e il prefisso, ma non il futuro. Quando compare un riferimento a PowerShell dopo la descrizione di un evento, la rappresentazione corrente può integrare informazioni dell'evento precedente. Il recupero è numerico: le query non sono domande scritte e le key non sono parole chiave scelte manualmente.

Le 16 teste query condividono quattro gruppi di key e value. Questa organizzazione, chiamata Grouped-Query Attention, riduce la memoria necessaria rispetto a conservare un gruppo KV indipendente per ciascuna query head. Le diverse teste possono costruire combinazioni differenti del contesto. Il gate dell'uscita regola poi quanto contribuiscono le componenti prodotte dall'attenzione.

## DeltaNet e memoria associativa

La full attention conserva rappresentazioni delle posizioni precedenti che possono essere consultate direttamente. DeltaNet mantiene invece uno stato ricorrente: una matrice che riassume associazioni fra key e value mentre vengono letti i token. Una query interroga questo stato per recuperare informazioni utili.

La caratteristica della delta rule è aggiornare un'associazione tenendo conto di ciò che la memoria contiene già. Il blocco legge il valore associato alla key corrente, lo confronta con il nuovo value e usa la differenza per correggere lo stato. L'aggiornamento non consiste quindi soltanto nell'accumulare tutte le informazioni ricevute.

Un esempio didattico aiuta a capire la correzione: se una certa associazione restituisce il valore 1 e l'informazione corrente richiede 3, un aggiornamento parziale può spostarla verso 2. L'esempio descrive il principio numerico; nella rete reale key e value sono vettori distribuiti, non caselle con significati scritti e separati.

Le **Gated Delta Networks** aggiungono un controllo sulla conservazione dello stato precedente. Un segnale regola quanto trattenere della memoria, mentre un altro regola quanto correggere l'associazione corrente. Il modello può così attenuare informazione precedente e aggiornare selettivamente ciò che conserva. Il termine gate indica una modulazione appresa dei valori, non una decisione simbolica esplicita.

Ogni blocco mantiene il proprio stato. Si tratta di memoria temporanea dell'inferenza: non coincide con un database, con la cronologia persistente di VERA o con un aggiornamento dei pesi. La dimensione dello stato non cresce aggiungendo una coppia KV per ogni token, ma la compressione può causare interferenze e perdita di dettagli. Gli otto blocchi di full attention completano questo meccanismo con accesso diretto al contesto. L'intero modello conserva quindi anche costi legati alla full attention.

## Dense e Mixture of Experts

Il 4B è un modello **dense**: nei suoi blocchi viene applicata la feed-forward prevista, senza scegliere soltanto alcuni esperti da un insieme. Dense non significa che ogni riga dell'embedding sia recuperata per ogni token, né che il modello sia privo di gate.

In una **Mixture of Experts**, alcuni blocchi contengono più feed-forward e un router seleziona quali utilizzare per ciascun token. Gli esperti sono componenti della stessa rete, non necessariamente assistenti completi e indipendenti. La loro specializzazione emerge dall'addestramento e non deve coincidere con etichette umane come medicina o matematica.

Un MoE può avere molti parametri totali e attivarne soltanto una parte per token. Questo riduce il calcolo rispetto ad attivarli tutti, ma gli altri esperti devono comunque essere conservati e resi disponibili. Un modello con pochi parametri attivi non richiede automaticamente la stessa memoria di un modello dense altrettanto piccolo.

Queste classificazioni si combinano: causale descrive l'accesso al contesto, autoregressivo la generazione progressiva, ibrido l'alternanza dei meccanismi, dense l'assenza di routing fra esperti e multimodale i tipi di dati supportati. Quantizzato descrive invece la precisione dei pesi. Qwen3.5-4B può appartenere contemporaneamente a tutte queste categorie compatibili.

Per struttura generale si distinguono anche encoder bidirezionali, che costruiscono rappresentazioni usando il contesto da entrambi i lati; decoder causali, che generano continuazioni a partire dal prefisso; ed encoder-decoder, nei quali un componente elabora l'input e un altro genera l'output utilizzando quelle rappresentazioni. Un modello base è inoltre distinto da uno adattato a seguire istruzioni, spesso indicato come Instruct o Chat. Queste differenze riguardano struttura e addestramento, non quantizzazione o scelta degli esperti.

## Capacità visive e modalità di generazione

Il checkpoint originale comprende un vision encoder che trasforma immagini e fotogrammi in rappresentazioni utilizzabili dal componente linguistico. Nel pacchetto GGUF i pesi visivi sono distribuiti separatamente attraverso un file mmproj. Il profilo locale di VERA qui considerato utilizza il componente testuale e non carica quel file: la capacità visiva del checkpoint non è quindi una funzione attiva automaticamente nell'applicazione.

La modalità thinking consente una generazione con passaggi intermedi prima della risposta finale. Richiede più token e più tempo, senza aggiungere parametri. Nel profilo locale di VERA è disattivata per contenere il costo delle risposte strutturate. La Multi-Token Prediction riguarda invece componenti addestrate a prevedere token futuri e utilizzabili, con runtime e configurazioni adatti, per accelerare la generazione. È una funzione distinta dal thinking.

## Perché Qwen è adatto a VERA

VERA recupera evidenze da corpus, fonti e [[Knowledge Graphs]], poi richiede al modello di produrre una risposta coerente con esse. Il modello deve comprendere il materiale, distinguere fatti supportati e inferenze e rispettare il formato richiesto. Non deve custodire nei propri pesi l'intera base di conoscenza del sistema. Questa divisione del lavoro rende ragionevole l'impiego di un modello relativamente piccolo.

Il PC esaminato dispone di un Intel Core Ultra 7 255H, circa 32 GB di RAM e una NVIDIA RTX PRO 500 Blackwell Laptop con circa 6 GB di VRAM. Il GGUF Q4_K_M occupa circa 2,71 GB sul disco; una registrazione locale riporta un picco GPU di 3.102 MiB, circa 3,03 GiB. Il profilo di avvio configura 8.192 token e una sola sequenza. Pesi, cache e buffer devono condividere la memoria disponibile: la dimensione del file non rappresenta l'intero fabbisogno.

Llama 3.1 8B Instruct è un'alternativa dense più grande, con full attention in tutti i 32 blocchi. Un GGUF Q4_K_M rappresentativo occupa circa 4,92 GB sul disco e lascia meno margine per cache e memoria di lavoro. Sul PC è plausibile provarlo con contesto contenuto; mantenerlo interamente sulla GPU con 8.192 token è molto più stretto e può richiedere compromessi. La RAM consente l'offload, ma con prestazioni diverse dall'esecuzione interamente accelerata.

La preferenza per Qwen è quindi motivata dal bilancio fra capacità e risorse, non dalla regola che un 4B sia sempre migliore di un 8B. Il confronto decisivo riguarda correttezza, citazioni, formato e latenza sugli stessi casi di VERA. Una prima alternativa contenuta è Qwen Q6_K: aumenta la precisione dei pesi e occupa circa 3,46 GB, mantenendo la stessa architettura.

Il contesto nativo di Qwen arriva a 262.144 token, ma non va confuso con il contesto conveniente su questa macchina. Fonti selezionate e controlli esterni restano necessari: né molti parametri né una lunga finestra di contesto garantiscono la verità delle affermazioni. [[Da hello alla risposta in Qwen3.5]] segue concretamente il percorso che trasforma l'input in nuovi token.
