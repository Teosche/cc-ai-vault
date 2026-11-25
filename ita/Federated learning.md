# Federated learning

Capitolo: [[Paradigmi di apprendimento]]

Il federated learning permette a più partecipanti di addestrare un modello condiviso mantenendo i dati di training presso i rispettivi dispositivi o istituzioni. Il coordinamento riguarda gli aggiornamenti del modello, anziché il trasferimento di tutti gli esempi in un archivio centrale. Ogni partecipante può comunque usare un obiettivo supervisionato o self-supervised: «federated» descrive dove e come si svolge l'apprendimento.

## Un ciclo di addestramento condiviso

Nel metodo FedAvg un server invia il modello corrente a un insieme di client. Ciascuno esegue alcuni aggiornamenti sui propri dati e restituisce i parametri risultanti. Il server li combina e distribuisce il nuovo modello per il round successivo. Si alternano così apprendimento locale e aggregazione.

Se $w_k$ sono i parametri dopo l'aggiornamento del client $k$ e $n_k$ il suo numero di esempi, una media pesata fra i client partecipanti è:

$$
w_{\mathrm{nuovo}}=\sum_k\frac{n_k}{\sum_j n_j}w_k.
$$

Con due partecipanti che possiedono 100 e 300 esempi, il secondo pesa tre volte il primo. Come esempio su un solo coefficiente, se i valori locali sono 2 e 6, il nuovo valore è $0,25\cdot2+0,75\cdot6=5$.
Questo illustra l'aggregazione; non significa che mediare modelli arbitrari addestrati indipendentemente sia equivalente a FedAvg, che parte da uno stato comune.

## Dati diversi e comunicazione limitata

I client possono avere quantità e tipi di dati differenti. Un dispositivo può contenere soprattutto una lingua e un altro un'altra lingua; un ospedale può osservare casi diversi da quelli degli altri. Si parla di distribuzioni non identiche, o non-IID. Gli aggiornamenti locali possono quindi orientarsi in direzioni diverse.

Eseguire più lavoro locale riduce il numero di comunicazioni, ma può aumentare la distanza fra i modelli dei client. Inoltre, alcuni dispositivi possono essere lenti o indisponibili. La procedura deve bilanciare qualità, costo della trasmissione e rappresentatività dei partecipanti effettivamente coinvolti.

## Conservare i dati localmente e proteggerli

Non trasmettere gli esempi grezzi riduce una forma di esposizione, ma gli aggiornamenti possono comunque contenere informazioni sui dati. Il federated learning non garantisce quindi da solo la riservatezza. La secure aggregation permette al coordinatore di ottenere un aggregato senza vedere direttamente i contributi individuali, entro le assunzioni del protocollo. La protezione dipende dal sistema completo e da quali soggetti si considerano affidabili.

La valutazione deve esaminare sia il modello complessivo sia le prestazioni sui diversi partecipanti. Un miglioramento medio può nascondere un peggioramento su client poco rappresentati; la sola accuratezza centrale non descrive tale differenza.
