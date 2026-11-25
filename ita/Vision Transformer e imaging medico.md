Capitolo: [[Adattamento, valutazione e applicazioni dei Foundation Models]]

Un Vision Transformer, o ViT, applica l’elaborazione di sequenze alle immagini. Una fotografia non è una sequenza di parole, ma può diventare una sequenza di regioni rappresentate da vettori. L’attenzione permette poi di collegare informazioni provenienti da regioni diverse. Nei video bisogna rappresentare anche il tempo: riconoscere gli stessi oggetti in due sequenze non basta a distinguere, per esempio, aprire una porta da chiuderla.

## ViT: dalla patch all’embedding

Un’immagine di altezza $H$, larghezza $W$ e $C$ canali viene divisa in patch quadrate non sovrapposte di lato $P$. Ciascuna patch contiene $P^2C$ valori e viene appiattita in un vettore. Una proiezione lineare appresa trasforma questo vettore nella dimensione interna $D$ del Transformer. Appiattire riordina i valori; la proiezione impara come combinarli per costruire caratteristiche utili al compito.

Per un’immagine RGB di 224 per 224 pixel e patch di lato 16 si ottengono 196 patch. Ogni patch contiene $16\cdot16\cdot3=768$ valori, ma il suo embedding può avere una dimensione diversa: la proiezione collega questi due spazi. Tutte le patch usano la stessa trasformazione appresa. Non si cercano identificativi in un vocabolario come nella tokenizzazione del testo: il token visivo deriva direttamente dai valori della regione.

Alla rappresentazione di ciascuna patch viene sommato un embedding di posizione. Questo rende distinguibili, per esempio, un dettaglio in alto a sinistra e lo stesso dettaglio in basso a destra. L’ordine con cui si appiattiscono le patch è una convenzione; la sequenza deve conservare un collegamento coerente con la griglia dell’immagine.

## Il Transformer encoder e la classificazione

Il ViT di classificazione aggiunge un class token appreso, normalmente indicato come CLS. È un vettore di parametri indipendente dalle patch: all’inizio non rappresenta una regione dell’immagine, ma durante l’elaborazione interagisce con i token visivi. Si distingue quindi il vettore iniziale, condiviso fra immagini, dal suo stato nascosto finale, che dipende dall’immagine corrente.

La sequenza attraversa blocchi encoder. In ciascun blocco, la Layer Normalization prepara le rappresentazioni per la multi-head self-attention; ogni token costruisce query, key e value e combina informazioni degli altri token secondo i pesi di attenzione. Un collegamento residuale aggiunge il risultato alla rappresentazione precedente. Seguono un’altra normalizzazione e un MLP, applicato a ciascun token, con due trasformazioni lineari e una non linearità GELU; un secondo collegamento residuale conserva anche in questo passaggio l’informazione precedente.

Attenzione e MLP hanno ruoli complementari: la prima scambia informazioni fra posizioni, il secondo trasforma le caratteristiche presenti in ciascuna rappresentazione. Dopo più blocchi, lo stato finale del CLS alimenta una testa di classificazione che produce i punteggi delle categorie. Per un problema a classe unica, la softmax li converte in una distribuzione sulle classi. Il ViT è qui un encoder con una testa di classificazione: non serve un decoder autoregressivo.

La rete può essere riutilizzata come backbone, cioè come componente che produce rappresentazioni per un sistema più ampio. Cambiare la testa non cambia da solo il modo in cui le patch vengono elaborate. Il riuso e l’eventuale modifica dei pesi dipendono dal [[Fine-tuning e adattamento efficiente|metodo di adattamento]].

## Dettaglio spaziale, costo e apprendimento

Con attenzione globale ogni patch può interagire con tutte le altre già nel primo blocco. Questa possibilità non implica che il modello abbia già imparato relazioni corrette: deve apprendere dai dati quali collegamenti siano utili. Rispetto a una CNN, il ViT classico impone meno vincoli di località e gerarchia spaziale, affidando una parte maggiore di questa struttura all’apprendimento.

Il costo cresce con il numero di token. Dimezzare il lato delle patch nell’immagine precedente porta da 196 a 784 patch, quadruplicandone il numero. I confronti fra coppie crescono approssimativamente di sedici volte. Si guadagna dettaglio, ma aumentano memoria e calcolo. Dati, preaddestramento e regolarizzazione diventano particolarmente importanti quando un modello flessibile viene adattato su raccolte piccole.

## ViViT: costruire gli embedding di una clip video

ViViT estende ViT alla classificazione di video. Una clip è una porzione temporale della registrazione, composta da frame. Prima di scegliere come calcolare l’attenzione, occorre stabilire che cosa costituisca un token. **Uniform Frame Sampling** e **Tubelet Embedding** sono due strategie di costruzione dell’input; le quattro architetture ViViT descrivono invece come elaborarlo.

### Uniform Frame Sampling

Nel campionamento uniforme si selezionano frame distribuiti a intervalli regolari nella clip. Ogni frame viene diviso in patch bidimensionali, appiattite e proiettate come in ViT. Gli embedding dei diversi frame vengono poi riuniti nella sequenza video, mantenendo informazioni su posizione e ordine temporale.

*Se si campionano $T$ frame e ciascuno contiene $S$ patch, si producono $TS$ token visivi*. In un esempio con otto frame di 32 per 32 pixel e patch di lato 16, ogni frame fornisce quattro patch e la clip produce 32 token. Un token descrive inizialmente una regione di un solo frame: le relazioni con altri istanti devono essere costruite dal Transformer.

La distanza fra frame determina quali eventi siano osservabili. Un movimento breve può cadere fra due campioni e non essere rappresentato. L’informazione temporale non è però intrinsecamente irrecuperabile: un’opportuna codifica delle posizioni temporali e l’elaborazione successiva possono collegare i frame selezionati.

### Tubelet Embedding

Una tubelet è un blocco non sovrapposto che comprende una regione spaziale su più frame consecutivi. Con dimensioni temporali e spaziali $t\times P\times P$, contiene $tP^2C$ valori, che vengono appiattiti e proiettati in un solo embedding. L’operazione equivale a un embedding tramite convoluzione tridimensionale con kernel e passo appropriati.

Se le dimensioni sono divisibili per quelle delle tubelet, il numero di token è:

$$
N=\frac{T}{t}\frac{H}{P}\frac{W}{P}.
$$

Nell’esempio precedente, una tubelet lunga due frame e larga 16 per 16 pixel produce $4\cdot2\cdot2=16$ token. Ogni token combina già variazioni entro un piccolo intervallo temporale, e la sequenza risulta più corta. La tubelet resta legata a una posizione spaziale: non segue automaticamente lo stesso oggetto mentre si sposta fra regioni diverse. Aumentarne la durata riduce i token ma rende più compressa la rappresentazione delle variazioni locali.

## Spatio-temporal Attention: elaborare insieme spazio e tempo

La prima architettura ViViT invia tutti i token spazio-temporali a un encoder con attenzione globale. Una query può confrontarsi con token di altre regioni e di altri istanti. Per riconoscere un gesto, una regione con la mano può quindi interagire direttamente con regioni che mostrano l’oggetto prima e dopo il movimento.

Un class token può raccogliere una rappresentazione complessiva della clip, poi classificata dalla testa finale. Il limite principale è il numero di confronti: con $T$ posizioni temporali e $S$ posizioni spaziali, l’attenzione globale considera un ordine di grandezza pari a $(TS)^2$ coppie. Con clip lunghe o patch piccole, questo costo può diventare dominante.

## Factorized Encoder: prima lo spazio, poi il tempo

La seconda architettura divide l’elaborazione fra due encoder. L’encoder spaziale confronta i token appartenenti allo stesso indice temporale e produce un vettore riassuntivo per ciascun frame o blocco temporale. Tale vettore può essere il CLS spaziale oppure una media delle rappresentazioni delle patch. Lo stesso encoder spaziale viene riutilizzato sui diversi istanti.

I vettori riassuntivi, ordinati nel tempo e dotati delle informazioni posizionali necessarie, formano l’input dell’encoder temporale. Quest’ultimo apprende le relazioni fra istanti e produce la rappresentazione usata per classificare il video. Con otto frame, il secondo encoder elabora otto riassunti anziché tutte le patch di tutti i frame.

Si tratta di una fusione temporale successiva alla sintesi spaziale. Il risparmio deriva anche dalla compressione: se un dettaglio locale viene perso nel riassunto del frame, l’encoder temporale non lo riceve più come token autonomo. La distinzione da un encoder-decoder è essenziale: entrambi i componenti codificano rappresentazioni; il secondo non genera una sequenza di uscita token per token.

## Factorized Self-Attention: separare le operazioni dentro il blocco

La terza architettura conserva la griglia dei token spazio-temporali e fattorizza l’attenzione all’interno dei blocchi. Un’operazione spaziale mette in relazione patch dello stesso istante; una successiva operazione temporale collega token della stessa posizione spaziale attraverso istanti diversi. Dopo questi scambi interviene la trasformazione MLP.

Per seguire un oggetto che si muove, l’informazione può passare fra posizioni nella fase spaziale e fra tempi nella fase temporale. Non si riduce ogni frame a un solo vettore prima dell’intera elaborazione temporale, come nel Factorized Encoder. Le due operazioni si ripetono invece nei blocchi, mantenendo rappresentazioni locali.

Il costo dei confronti è collegato a $TS^2+ST^2$, anziché a $(TS)^2$, considerando separatamente ogni gruppo spaziale e temporale. Questo conteggio descrive le coppie di token, non l’intero costo della rete. Nella formulazione originale di questa variante si usa il pooling delle rappresentazioni finali al posto del class token, evitando ambiguità quando i token vengono riorganizzati fra gli assi.

## Factorized Dot-Product Attention: specializzare le teste

La quarta architettura separa spazio e tempo fra le teste dello stesso modulo di attenzione. Una parte delle teste confronta token dello stesso istante in posizioni diverse; le altre confrontano token della stessa posizione in istanti diversi. Le uscite vengono concatenate e combinate da una proiezione lineare.

La differenza rispetto alla variante precedente è il punto della fattorizzazione. Nel Factorized Self-Attention si eseguono due operazioni successive, per cui la seconda riceve il risultato della prima. Nel Factorized Dot-Product Attention i gruppi di teste elaborano in parallelo due tipi di vicinato e i risultati vengono poi fusi. Si limita così l’insieme delle interazioni senza aggiungere due moduli completi di self-attention per blocco.

Le quattro architetture rappresentano compromessi fra interazioni dirette, compressione, numero di parametri e costo. La scelta dipende dai dati e dalla durata elaborabile. Il modello più efficiente può trattare una clip più lunga; quello globale può conservare collegamenti più diretti. Non esiste una superiorità generale del Factorized Encoder indipendente dalla configurazione. Il trasferimento da modelli preaddestrati sulle immagini permette inoltre di riutilizzare parte delle capacità spaziali, mentre le componenti temporali devono essere inizializzate e apprese in modo appropriato.

## Swin: costruire una gerarchia visiva

Swin Transformer affronta soprattutto il costo delle immagini ad alta risoluzione e la presenza di oggetti a scale diverse. Il ViT classico mantiene sostanzialmente la stessa griglia di patch nei livelli; Swin costruisce rappresentazioni via via meno risolute e più ricche di canali. Una gerarchia è utile anche come backbone per localizzazione e segmentazione, dove servono informazioni spaziali a scale differenti.

### Patch Partition e Patch Merging

La Patch Partition iniziale divide un’immagine RGB in patch di 4 per 4 pixel. Ogni patch contiene 48 valori, proiettati nella dimensione $C$ delle caratteristiche. Il primo stadio lavora quindi su una griglia di $H/4$ per $W/4$ token.

Il Patch Merging raggruppa quattro token adiacenti disposti in un quadrato di 2 per 2. Concatena i loro vettori lungo i canali, ottenendo temporaneamente $4C$ caratteristiche, e applica una proiezione a $2C$. Altezza e larghezza si dimezzano, il numero di token diventa un quarto e la dimensione di ciascuno raddoppia. La concatenazione e la proiezione sono passaggi diversi: concatenare soltanto non darebbe $2C$ canali.

Ripetendo l’operazione si ottengono stadi con risoluzioni $H/4$, $H/8$, $H/16$ e $H/32$ lungo l’altezza, e dimensioni $C$, $2C$, $4C$ e $8C$. La larghezza segue la stessa riduzione. La rete costruisce così caratteristiche locali dettagliate e rappresentazioni successive più compatte.

### Window-based Self-Attention

L’attenzione viene calcolata entro finestre non sovrapposte di $M\times M$ token. Ogni query si confronta con i token della propria finestra, invece che con tutta l’immagine. Con $N$ token complessivi ci sono circa $N/M^2$ finestre, ciascuna con $M^4$ coppie: il totale è proporzionale a $NM^2$. Se la dimensione della finestra rimane fissa, questo termine cresce linearmente con i token, mentre l’attenzione globale cresce quadraticamente.

Ogni blocco comprende anche normalizzazioni, MLP e collegamenti residuali. Limitare il vicinato dell’attenzione non elimina le altre trasformazioni. Con finestre sempre identiche, inoltre, i token di due finestre confinanti non potrebbero scambiarsi direttamente informazioni attraverso questi moduli.

### Shifted Windows, Cyclic Shift e maschera

Swin alterna blocchi con finestre regolari e blocchi con partizione spostata di circa metà finestra. Un token vicino al confine può così incontrare nel blocco successivo token che prima appartenevano alla finestra adiacente. L’informazione attraversa progressivamente i confini; non diventa automaticamente globale dopo due soli blocchi, indipendentemente dalle dimensioni dell’immagine.

Lo spostamento crea regioni di bordo difficili da organizzare in finestre tutte uguali. Il Cyclic Shift riporta sul lato opposto i token che escono dal bordo, permettendo una disposizione regolare per il calcolo. Questa operazione può però mettere nella stessa finestra token lontani nell’immagine originale.

Una maschera impedisce tali interazioni artificiali: i confronti non consentiti ricevono un valore molto negativo prima della softmax, ottenendo peso nullo. Dopo l’attenzione si applica lo spostamento inverso. Lo shift organizza quindi il calcolo; la maschera conserva il vicinato desiderato e lo shift inverso ripristina l’allineamento spaziale.

### Relative Position Bias

Swin utilizza anche la posizione relativa fra token. Al confronto fra query e key aggiunge un termine appreso che dipende dallo spostamento spaziale della coppia. Un token può così distinguere un vicino a destra da uno sopra, anche all’interno della stessa finestra.

$$
\operatorname{Attention}(Q,K,V)=
\operatorname{softmax}\!\left(\frac{QK^\top}{\sqrt{d_k}}+B+\mathcal{M}\right)V.
$$

$B$ è il bias di posizione relativa e $\mathcal{M}$ rappresenta la maschera dei confronti vietati. Località, gerarchia e posizione relativa introducono struttura spaziale nell’elaborazione: Swin non elimina semplicemente ogni informazione di posizione.

## Imaging medico: dalle slice alla classificazione

La classificazione delle lesioni pancreatiche IPMN utilizza un adattamento visivo distinto dalla classificazione di azioni video. Le slice di una risonanza descrivono sezioni spaziali del volume; il loro indice non deve essere confuso con il tempo di una clip. Aggregare più sezioni rende disponibili dettagli che una singola slice potrebbe non mostrare.

Nel sistema considerato, regioni d’interesse ricavate da slice consecutive vengono disposte in una griglia. Le acquisizioni T1 e T2 sono combinate come due canali nell’immagine di ingresso: questa è un’early fusion, perché il modello apprende una rappresentazione congiunta prima della decisione finale. La griglia viene divisa in patch, proiettata in embedding e completata con informazioni posizionali e class token. Il CLS finale alimenta la classificazione in Normal Grade, Low-Grade o High-Grade.

In una late fusion, le modalità vengono invece elaborate separatamente e combinate in una fase successiva. Il confronto fra le due strategie verifica se il modello tragga vantaggio dall’elaborazione congiunta precoce dei contrasti, anziché limitarsi a combinare risultati già separati.

La valutazione comprende 139 scansioni di pazienti distinti e una cross-validation a dieci fold. Le slice dello stesso paziente devono restare nella stessa suddivisione quando si misura la generalizzazione a pazienti nuovi, evitando che informazioni correlate compaiano sia nel training sia nel test. Le accuracy centrali riportate sono 0,53 con T1, 0,64 con T2, 0,60 con late fusion e 0,70 con early fusion T1+T2. Nel confronto fra architetture, i valori centrali sono 0,42 per AlexNet, 0,51 per DenseNet e 0,70 per il metodo Transformer. Sono risultati dello specifico esperimento, accompagnati da variabilità nelle prove: non dimostrano che ogni Transformer superi ogni CNN o che la combinazione T1+T2 sia sempre preferibile.

## Attention Rollout e lettura delle mappe

Per una previsione visiva, osservare soltanto l’ultima matrice di attenzione non ricostruisce come l’informazione sia stata mescolata nei livelli precedenti. L’Attention Rollout aggrega prima le teste di ciascun livello, tipicamente mediandone le matrici. Integra poi i collegamenti residuali, per esempio aggiungendo l’identità e rinormalizzando, e moltiplica le matrici dei livelli nell’ordine del flusso di informazione.

Con matrici per livello $A_1,A_2,\ldots,A_L$ già preparate in questo modo, il prodotto $R=A_L\cdots A_2A_1$ riassume i percorsi attraverso cui l’informazione può propagarsi dall’input all’ultimo livello. La riga del CLS, limitata alle patch, viene ridisposta sulla griglia per ottenere una mappa. Una patch può risultare collegata al CLS anche attraverso token intermedi, pur avendo un collegamento diretto finale debole.

Nel caso MRI, confrontare mappe e regione della cisti permette di esaminare se il sistema stia utilizzando l’area pertinente o indizi di contesto. Una previsione corretta accompagnata da una mappa concentrata altrove suggerisce un problema da indagare. Il rollout descrive però una propagazione basata sull’attenzione e non comprende integralmente MLP, interazioni e trasformazioni del modello. Per verificare l’importanza di una regione occorre confrontarlo anche con perturbazioni e altri [[Metodi di interpretabilità dei modelli]].
