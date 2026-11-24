# Variational Autoencoders

Capitolo: [[Modelli neurali profondi]]

Un **Variational Autoencoder**, abbreviato **VAE**, è un modello generativo che apprende a rappresentare i dati tramite *variabili latenti* e a produrre nuovi esempi a partire da esse. Mantiene una struttura encoder-decoder, ma la inserisce in un modello probabilistico: l'encoder associa a un input una **distribuzione di possibili codici**, mentre il decoder descrive quali dati siano plausibili dato un codice.

Il punto di partenza è un limite degli [[Autoencoders]] ordinari. Un decoder addestrato soltanto a ricostruire gli esempi non sa necessariamente che cosa produrre per un codice casuale. Per generare nuovi dati serve una regola su come scegliere i codici e un addestramento che renda utile quella scelta.

## Un modello generativo con variabili latenti

Una **variabile latente** rappresenta fattori non osservati direttamente, ma utili a spiegare i dati. Nel caso di immagini di cifre potrebbe contribuire a descrivere differenze di forma e stile, senza che queste vengano etichettate manualmente. Il modello suppone che si possa generare un dato scegliendo prima un codice $z$ da una distribuzione $p(z)$ e poi un dato $x$ da una distribuzione condizionata $p_\theta(x\mid z)$. 

La distribuzione $p(z)$ è detta **prior**: stabilisce *come vengono scelti i codici prima di osservare una specifica immagine*. Una scelta comune è una distribuzione normale standard, centrata in zero e con varianza uno per componente. Il decoder, con parametri $\theta$, definisce invece $p_\theta(x\mid z)$, cioè la distribuzione dei dati associata a ciascun codice.

Generare e codificare risolvono due problemi diversi. Per generare si procede da $z$ a $x$; quando abbiamo già un'immagine, vorremmo risalire ai codici che potrebbero averla prodotta. La distribuzione che risponde a questa seconda domanda è la **posterior** $p_\theta(z\mid x)$. Calcolarla esattamente è in genere difficile, perché richiede di considerare tutti i possibili codici del modello.

## Che cosa produce l'encoder

Il VAE usa una rete encoder $q_\phi(z\mid x)$ per approssimare la posterior. Il simbolo $q$ distingue l'approssimazione appresa dalla distribuzione esatta $p_\theta(z\mid x)$. «Variazionale» indica proprio l'uso di una famiglia di distribuzioni approssimanti, i cui parametri vengono ottimizzati.

Nella versione più comune l'encoder produce due vettori: la **media** $\mu(x)$ e le **deviazioni standard** $\sigma(x)$ di una distribuzione normale con componenti indipendenti:

$$
q_\phi(z\mid x)=\mathcal{N}\!\left(\mu(x),\operatorname{diag}(\sigma^2(x))\right).
$$

La media indica ***dove** si concentra la distribuzione del codice*; la deviazione standard indica ***quanto** essa sia dispersa*. La notazione $\operatorname{diag}$ specifica una matrice di covarianza diagonale: in questa approssimazione non vengono modellate correlazioni fra le componenti del codice, una volta fissato l'input.

Per una stessa immagine si possono quindi campionare codici differenti. Non si tratta però di aggiungere rumore arbitrariamente: la distribuzione e il suo rapporto con il decoder vengono appresi attraverso un obiettivo preciso.

## Campionare senza interrompere l'apprendimento

Per addestrare entrambe le reti occorre far propagare il gradiente attraverso il codice campionato. Si usa il **reparameterization trick**: invece di descrivere $z$ soltanto come un campione estratto dalla distribuzione dell'encoder, lo si costruisce da un rumore indipendente dai parametri:

$$
\varepsilon\sim\mathcal{N}(0,I),\qquad
z=\mu(x)+\sigma(x)\odot\varepsilon.
$$

$\odot$ indica il prodotto componente per componente e $I$ la matrice identità. Il rumore $\varepsilon$ viene campionato dalla normale standard; media e scala sono prodotte dall'encoder. Per un rumore fissato, il codice dipende così in modo differenziabile dai parametri della rete.

In un esempio a due dimensioni, se $\mu=(1,-1)$, $\sigma=(0{,}2,0{,}5)$ e $\varepsilon=(0{,}5,-1)$, il codice è $z=(1{,}1,-1{,}5)$. Questi numeri illustrano il calcolo, non corrispondono a un'immagine o a un modello già addestrato.

## L'obiettivo: ricostruzione e regolarizzazione

L'addestramento deve rendere i dati ricostruibili e mantenere le distribuzioni dei codici compatibili con il prior. Una forma della perdita da minimizzare per un esempio è:

$$
L_{VAE}(x)=
-\mathbb{E}_{z\sim q_\phi(z\mid x)}[\log p_\theta(x\mid z)]
+D_{KL}\!\left(q_\phi(z\mid x)\,\|\,p(z)\right).
$$

Il primo termine misura quanto il decoder riesca ad attribuire alta probabilità al dato originale usando codici campionati dall'encoder. Il simbolo $\mathbb{E}$ indica una media sulla distribuzione dei codici; nella pratica la si stima con campioni. Secondo la distribuzione scelta per i dati, questo termine può tradursi in un errore di ricostruzione simile all'errore quadratico o alla cross-entropy.

Il secondo termine è una **divergenza di Kullback–Leibler**, o **KL**, che confronta la distribuzione prodotta dall'encoder con il prior. È non negativa e vale zero quando le due distribuzioni coincidono. Penalizza quindi codifiche troppo lontane dalla distribuzione da cui vorremo campionare durante la generazione. Il risultato dipende dal compromesso: distribuire tutti i codici nello stesso modo renderebbe difficile distinguere gli input, mentre trascurare il prior ostacolerebbe il campionamento di codici utili.

Il negativo di questa perdita è detto **ELBO**, *Evidence Lower Bound*: è un limite inferiore al logaritmo della probabilità del dato secondo il modello. Massimizzarlo permette di apprendere il modello generativo e l'encoder approssimante senza dover calcolare esattamente la posterior. Non è quindi soltanto un errore di ricostruzione con rumore aggiunto.

## Ricostruire e generare

Per **ricostruire** un'immagine si passa dall'encoder, si ottiene un codice dalla distribuzione associata all'immagine e si usa il decoder. Per **generare**, invece, non serve un'immagine iniziale: si campiona $z$ dal prior e lo si passa al decoder. L'uscita della rete parametrizza una distribuzione sui dati, dalla quale si può campionare; in alcune visualizzazioni se ne mostra direttamente la media.

Nel caso di cifre manoscritte, codici diversi possono produrre nuove forme e stili simili a quelli appresi. La regolarizzazione favorisce uno spazio latente utilizzabile anche fra i codici degli esempi, ma non garantisce che ogni punto produca un'immagine plausibile o che una componente rappresenti esattamente una caratteristica interpretabile.

## Limiti e confronto con gli altri modelli

Un VAE può imparare rappresentazioni utili e generare dati, ma il risultato dipende dalla capacità delle reti, dal prior e dalla distribuzione scelta per l'uscita. Con certe perdite sui pixel le immagini possono apparire sfocate, perché il modello tende a mediare dettagli incerti. Un altro rischio è il **posterior collapse**: l'encoder si avvicina al prior e il decoder usa poco o nulla l'informazione contenuta nel codice.

Rispetto a un autoencoder ordinario, il VAE aggiunge un modello probabilistico e un obiettivo che regola la distribuzione latente. Le [[Generative Adversarial Networks]] apprendono invece attraverso il confronto fra un generatore e un discriminatore: è un diverso meccanismo di addestramento, che non richiede la stessa struttura di inferenza variazionale.
