# Fine-tuning e adattamento efficiente

Capitolo: [[Adattamento, valutazione e applicazioni dei Foundation Models]]

Il fine-tuning riprende un modello preaddestrato e lo ottimizza su dati più specifici. In un classificatore, gli esempi indicano la categoria corretta; in un modello generativo, possono associare una richiesta alla risposta desiderata. La loss misura la distanza dall'obiettivo e la backpropagation aggiorna i parametri scelti. Si riutilizzano così rappresentazioni già apprese, adattandole al nuovo uso.

## Dalle continuazioni alle istruzioni

Un modello addestrato a prevedere testo può continuare una domanda senza rispondervi nel modo atteso. L'instruction fine-tuning usa coppie di istruzioni e risposte per rendere più probabile un comportamento da assistente. Per esempio, una richiesta di riassunto viene associata a una sintesi del contenuto, anziché a una continuazione del documento.

L'allineamento tramite preferenze affronta un problema diverso: più risposte possono essere linguisticamente possibili, ma alcune risultano più utili o appropriate. Nell'RLHF si raccolgono giudizi su risposte alternative, si addestra un modello di reward a riprodurre tali preferenze e si aggiorna la policy per aumentare il reward. Si passa quindi dall'imitazione di una risposta bersaglio all'ottimizzazione di una valutazione. Il reward resta un'approssimazione delle preferenze, con possibili errori e distorsioni.

## Quanta parte del modello aggiornare

Nel **full fine-tuning** tutti i pesi selezionati del modello vengono aggiornati. Questo permette di modificare anche le rappresentazioni interne, ma richiede memoria per gradienti e stati dell'ottimizzatore, oltre che per i pesi. Dati scarsi o poco rappresentativi possono inoltre provocare overfitting o peggiorare capacità precedenti.

Nel **linear probing** si congela il modello che produce le rappresentazioni e si addestra una testa lineare. Immaginiamo che un encoder trasformi una recensione in un vettore: la testa impara quali combinazioni delle sue coordinate distinguono giudizi positivi e negativi. Se l'informazione necessaria non è presente nel vettore, la testa non può ricostruirla modificando l'encoder, perché l'encoder è congelato.

Il **Parameter-Efficient Fine-Tuning** interviene con pochi parametri addestrabili. Una stessa base può così essere condivisa fra compiti, conservando per ciascuno un componente piccolo. Il risparmio riguarda soprattutto aggiornamenti e adattatori: la base deve comunque essere disponibile e il calcolo delle attivazioni continua ad avere un costo.

## Apprendere vettori di condizionamento

Il prompt tuning sostituisce la ricerca manuale di una frase efficace con l'ottimizzazione di vettori aggiunti all'input. Durante l'addestramento, gli errori vengono propagati attraverso il modello congelato fino a questi vettori. Si apprendono quindi parametri continui che orientano l'elaborazione, senza richiedere che corrispondano a parole leggibili.

Il prefix tuning rende disponibili rappresentazioni apprese nei diversi livelli del Transformer, comunemente come key e value aggiuntive per l'attenzione. Ogni livello può così essere condizionato direttamente. Una riparametrizzazione può rendere più stabile l'ottimizzazione, generando i prefissi tramite una rete durante il training. Il P-tuning usa un prompt encoder per costruire vettori che possono occupare posizioni diverse nella sequenza. Nel visual prompt tuning lo stesso principio di condizionamento viene applicato ai token visivi. Il punto comune è il modello congelato; cambia dove entrano i parametri appresi.

## Visual Prompt Tuning: adattare un encoder visivo

Nel Visual Prompt Tuning si apprendono token aggiuntivi che partecipano all’elaborazione delle patch, mantenendo congelato il backbone visivo. Il prompt è quindi un insieme di vettori, non una descrizione testuale dell’immagine e neppure una nuova etichetta. Durante il training, l’errore della classificazione aggiorna questi vettori e la testa prevista dalla configurazione.

Nella variante shallow i prompt vengono inseriti all’ingresso del Transformer; nella variante deep si introducono prompt specifici nei diversi livelli. La seconda può condizionare direttamente più stadi dell’elaborazione senza aggiornare tutti i pesi del ViT. Il risparmio di parametri va distinto dal costo di elaborare token aggiuntivi.

Per classificare l’immagine si può usare lo stato finale del CLS oppure aggregare token di prompt, token visivi o entrambi mediante pooling. Cambiare questa scelta modifica quale rappresentazione raggiunge la testa. Inserire i prompt e scegliere come leggere l’uscita sono dunque due decisioni distinte, da verificare sul compito.

## LoRA: dal delta dei pesi alla fattorizzazione

Per adattare un modello si può descrivere il risultato come $W_{\mathrm{adattato}}=W_0+\Delta W$, dove $W_0$ contiene i pesi preaddestrati e $\Delta W$ la variazione appresa. Conservare separatamente la variazione permette di riutilizzare la stessa base per più compiti. Tuttavia, una matrice $\Delta W$ libera ha tanti coefficienti quanti $W_0$: cambiare il modo di salvarli non riduce ancora i parametri da apprendere.

LoRA introduce il risparmio imponendo una struttura alla variazione. Per una matrice $W_0\in\mathbb{R}^{m\times n}$, che trasforma un vettore di $n$ componenti in uno di $m$, si apprendono due fattori:

$$
A\in\mathbb{R}^{r\times n},\qquad B\in\mathbb{R}^{m\times r},\qquad
W_{\mathrm{adattato}}=W_0+\frac{\alpha}{r}BA.
$$

Con vettori colonna, la trasformazione aggiuntiva è $B(Ax)$: $A$ ricava $r$ combinazioni dell'input e $B$ le combina per produrre la correzione dell'output. Questa viene sommata a $W_0x$. Il rango della correzione è al massimo $r$; il rango di $W_0$ non viene ridotto. L'ipotesi è che, per il compito scelto, basti modificare poche direzioni della trasformazione già appresa.

Una trasformazione da 512 a 2048 componenti contiene $512\cdot2048=1.048.576$ pesi, escludendo il bias. Con $r=8$, $A$ contiene 4.096 coefficienti e $B$ ne contiene 16.384: in totale 20.480, circa l'1,95% della matrice completa. Si risparmia perché la correzione non può assegnare liberamente un valore indipendente a ciascuna delle oltre un milione di posizioni. I pesi originali rimangono comunque memorizzati ed eseguiti.

## Scala, inizializzazione e apprendimento

Nella formulazione standard, $\alpha/r$ regola quanto il ramo LoRA contribuisce all'output. Con rango 8 e $\alpha=16$, il contributo $BAx$ viene moltiplicato per 2. Il rango stabilisce la capacità della correzione, mentre la scala ne regola l'ampiezza; il learning rate governa invece la grandezza degli aggiornamenti durante l'ottimizzazione. Sono scelte distinte, benché influenzino insieme l'apprendimento. Se si cambia il rango mantenendo fisso $\alpha$, cambia anche il rapporto $\alpha/r$: un confronto sperimentale deve tenere conto di entrambe le variazioni.

Si inizializza normalmente $A$ con piccoli valori casuali e $B$ a zero. All'inizio $BA=0$, quindi l'aggiunta dell'adattatore lascia invariata la trasformazione preaddestrata. L'errore può comunque aggiornare $B$, perché il suo ingresso $Ax$ è generalmente diverso da zero. Dopo questo primo aggiornamento, anche $A$ può ricevere un gradiente non nullo. Inizializzare entrambi i fattori a zero bloccherebbe invece questo avvio: ciascun gradiente dipende dall'altro fattore.

Durante il training la loss misura l'errore sul compito, per esempio la classificazione di una recensione. La backpropagation attraversa il modello e aggiorna gli adattatori, lasciando congelati i pesi della base. Non servono gradienti dei pesi e stati dell'ottimizzatore per $W_0$, ma occorrono ancora calcoli e memoria per le attivazioni. Una testa di classificazione può essere addestrata insieme agli adattatori: in quel caso i parametri apprendibili comprendono anche la testa.

## Dove applicare LoRA e come riutilizzarla

Nei Transformer si possono adattare le proiezioni query, key, value e output dell'attenzione, le trasformazioni dei feed-forward oppure un loro sottoinsieme. Modificare query e value cambia rispettivamente le rappresentazioni usate per cercare relazioni e quelle da aggregare. Intervenire sui feed-forward modifica invece la trasformazione delle caratteristiche di ciascun token. La scelta dei moduli e del rango determina dove e con quanta libertà il modello può cambiare.

Per un classificatore BERT con 12 livelli e rappresentazioni di 768 componenti, applicare rango 8 a query e value richiede $12\cdot2\cdot8(768+768)=294.912$ parametri LoRA. Una testa da 768 componenti a tre classi aggiunge $768\cdot3+3=2.307$ parametri, bias incluso. Il totale addestrabile diventa 297.219: la sola percentuale dei fattori della singola matrice non descrive quindi l'intero modello. Un dropout di 0,1 sul ramo LoRA azzera casualmente una parte dei suoi ingressi durante il training per ridurre la dipendenza da combinazioni particolari; in inferenza viene disattivato.

A fine addestramento si possono salvare separatamente i fattori e caricare l'adattatore del compito desiderato sulla stessa base compatibile. Si può anche calcolare $W_{\mathrm{fuso}}=W_0+(\alpha/r)BA$: essendo il ramo aggiuntivo lineare, dopo la fusione basta una sola trasformazione della stessa dimensione di quella originaria. L'assenza di operazioni aggiuntive in inferenza riguarda questa configurazione; mantenere i rami separati richiede ancora il loro calcolo.

LoRA può essere combinata con altri metodi PEFT, come il prefix tuning, che agisce su rappresentazioni di condizionamento. Il risparmio di parametri non garantisce però un dato livello di accuratezza: contano dati, moduli selezionati, rango e ottimizzazione. Un rango maggiore amplia le variazioni rappresentabili, ma aumenta memoria e parametri e non assicura una migliore generalizzazione. Anche un modello con base congelata può cambiare comportamento e peggiorare su altri compiti mentre l'adattatore è attivo.

## LoHa, LoKr e AdaLoRA

Le varianti intervengono sulla struttura o sulla distribuzione della capacità di adattamento. LoHa costruisce due prodotti a basso rango e li combina elemento per elemento: $\Delta W=(B_1A_1)\odot(B_2A_2)$. Il simbolo $\odot$ indica il prodotto di Hadamard: nella stessa posizione si moltiplicano i due valori. Il risultato può avere rango maggiore di ciascun prodotto iniziale; la correzione segue quindi un vincolo diverso da quello di una sola coppia LoRA.

LoKr utilizza un prodotto di Kronecker, $\Delta W=C\otimes D$, eventualmente fattorizzando ulteriormente le matrici. Ogni coefficiente di $C$ moltiplica un'intera copia di $D$, formando una matrice a blocchi. Due matrici $2\times2$ producono così una matrice $4\times4$ descritta da otto coefficienti anziché sedici indipendenti. Questa condivisione riduce i parametri, ma impone relazioni fra i blocchi della correzione.

AdaLoRA affronta invece il fatto che assegnare lo stesso rango a ogni matrice può sprecare capacità dove serve poco. Rappresenta gli aggiornamenti tramite direzioni e coefficienti di una fattorizzazione simile alla decomposizione ai valori singolari, ne stima l'importanza durante l'apprendimento e riduce il contributo delle componenti meno utili. Entro un budget complessivo, le matrici più importanti conservano più capacità. Non basta dunque scegliere un rango maggiore ovunque: occorre anche decidere come distribuirlo.

## QLoRA: ridurre anche la memoria della base

Con LoRA i parametri addestrabili sono pochi, ma la base può ancora occupare molta memoria. Con [[Quantizzazione LoRA e QLoRA|QLoRA]], la base viene conservata a quattro bit e resta congelata; gli adattatori continuano a essere appresi con precisione maggiore. Le trasformazioni usano pesi dequantizzati per il calcolo, mentre l'ottimizzatore aggiorna i fattori LoRA.

NF4 sceglie livelli di rappresentazione adatti a pesi approssimativamente normali; la doppia quantizzazione comprime anche le scale necessarie a ricostruirli; gli ottimizzatori paginati gestiscono parte della pressione sulla memoria trasferendo stati fra GPU e RAM. Sono interventi su costi differenti. Quantizzare un modello dopo un normale fine-tuning LoRA per distribuirlo non equivale ad averlo addestrato con QLoRA.

## Efficienza dell'architettura e dell'adattamento

Una Mixture of Experts affronta un altro costo: in alcuni livelli sostituisce un solo feed-forward con più esperti, mentre un router sceglie quali attivare per ciascun token. Il numero totale di parametri può essere grande pur elaborandone soltanto una parte per token. Occorre però conservare gli esperti e distribuirne il carico; un router che ne seleziona sempre pochi crea squilibri. MoE e PEFT possono coesistere, ma il primo organizza l'elaborazione del modello e il secondo limita ciò che si addestra per adattarlo.
