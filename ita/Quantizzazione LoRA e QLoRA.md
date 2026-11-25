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

LoRA significa Low-Rank Adaptation ed è un metodo di [[Fine-tuning e adattamento efficiente|adattamento efficiente]]. Congela i pesi preaddestrati e apprende una variazione compatta. Per una trasformazione $W_0$ da $n$ a $m$ componenti, il risultato sull'input $x$ diventa:

$$
y=W_0x+\frac{\alpha}{r}B(Ax),\qquad
A\in\mathbb{R}^{r\times n},\quad B\in\mathbb{R}^{m\times r}.
$$

Il primo ramo applica la trasformazione originale; il secondo ricava $r$ combinazioni dell'input e le usa per correggere l'output. Si apprendono $r(n+m)$ coefficienti anziché i $mn$ di una variazione arbitraria. **Il rango indica la dimensione intermedia della correzione**, non il numero di blocchi modificati e neppure la precisione numerica. **È la correzione a essere a basso rango**: la matrice base mantiene la propria capacità rappresentativa.

Con rango 8, una trasformazione fra 2.560 e 9.216 componenti richiede $8(2.560+9.216)=94.208$ coefficienti aggiuntivi, contro i 23.592.960 della matrice completa. Il rapporto è circa lo 0,40%, ma riguarda questa singola matrice, non la memoria totale del training. La base, le attivazioni e gli eventuali altri moduli addestrabili hanno ancora un costo.

Nella scala standard, $\alpha=16$ e $r=8$ danno un moltiplicatore 2 al ramo aggiuntivo. Inizializzando $A$ casualmente e $B$ a zero si parte da $BA=0$: la trasformazione iniziale coincide con quella della base. Il gradiente può prima modificare $B$, alimentata da $Ax$, e poi entrambi i fattori. Congelare $W_0$ significa evitare gli aggiornamenti dei suoi pesi, non interrompere la propagazione dell'errore attraverso il modello.

Un piccolo esempio rende visibile la correzione. Siano $W_0$ la matrice identità $2\times2$, $A=(1,1)$, $B=(0{,}1,-0{,}1)^T$, rango 1 e scala 1. Per $x=(2,1)^T$, $Ax=3$ e $B(Ax)=(0{,}3,-0{,}3)^T$. L'output passa da $(2,1)^T$ a $(2{,}3,0{,}7)^T$: entrambe le componenti cambiano, ma lungo una sola direzione determinata da $B$. I numeri sono didattici; mostrano perché basso rango non significa modificare soltanto pochi coefficienti isolati.

## Dall'adattatore separato alla fusione

I fattori possono essere applicati alle proiezioni dell'attenzione, ai feed-forward o ad altri moduli compatibili. Salvare un adattatore separato permette di condividere la base fra più compiti, ma richiede la base corretta e una configurazione coerente dei moduli. I soli fattori non costituiscono un modello completo.

Quando si lavora con pesi non quantizzati, si può incorporare la correzione calcolando $W_{\mathrm{fuso}}=W_0+(\alpha/r)BA$. Nell'esempio precedente si ottiene:

$$
W_{\mathrm{fuso}}=
\begin{pmatrix}
1{,}1 & 0{,}1\\
-0{,}1 & 0{,}9
\end{pmatrix}.
$$

Moltiplicare questa matrice per $(2,1)^T$ restituisce $(2{,}3,0{,}7)^T$, come i due rami separati. Dopo la fusione non servono più i due prodotti aggiuntivi per ogni input; questo spiega il vantaggio sulla latenza. L'equivalenza è algebrica, con possibili differenze di arrotondamento nelle implementazioni numeriche.

## QLoRA: memorizzazione a quattro bit e calcolo a precisione maggiore

LoRA riduce quanto si addestra, ma conserva comunque la grande base. QLoRA riduce anche questo costo: mantiene i pesi base congelati e quantizzati a quattro bit, affiancandoli ad adattatori addestrabili a precisione maggiore. Le due scelte sono indipendenti: il numero di coefficienti LoRA dipende dal rango, i bit dei pesi base dal formato di quantizzazione.

Indicando con $q$ i codici quantizzati, con $c$ i dati necessari alla ricostruzione e con $\widehat W_0=\operatorname{dequant}(q,c)$ la base approssimata, il calcolo diventa:

$$
y=\widehat W_0x+\frac{\alpha}{r}B(Ax).
$$

I codici a quattro bit sono una rappresentazione di memorizzazione. Quando occorrono nelle operazioni, i pesi vengono ricostruiti nella precisione di calcolo, tipicamente BF16, un formato a 16 bit. Attivazioni, prodotti e adattatori non diventano quindi automaticamente a quattro bit. La dequantizzazione ricostruisce valori approssimati: aumentare nuovamente la precisione numerica non recupera l'informazione già persa.

## NF4: distribuire i livelli dove servono

Con quattro bit si possono scegliere 16 codici. Una griglia uniforme distribuisce i livelli a distanza costante, ma molti pesi possono concentrarsi vicino a zero: assegnare la stessa risoluzione alle zone centrali e alle code può utilizzare male i pochi livelli disponibili. NF4 usa una tabella non uniforme costruita a partire dai quantili di una distribuzione normale, con livelli più fitti nella regione centrale e una rappresentazione esatta dello zero.

La quantizzazione opera per blocchi. Per un blocco si calcola una scala $c$, per esempio il massimo valore assoluto, e si normalizzano i pesi dividendo per $c$. Ogni valore normalizzato viene associato a un livello della tabella; si conserva il suo codice a quattro bit e si ricostruisce il peso moltiplicando il livello scelto per $c$. Un blocco di soli zeri può essere rappresentato direttamente come nullo.

Il motivo dei blocchi si vede immaginando molti pesi vicini a 0,02 e un unico peso pari a 2. Una sola scala per l'intero insieme comprime tutti i valori piccoli vicino allo zero normalizzato; suddividerlo limita l'effetto del valore estremo al suo blocco. NF4 sfrutta un'ipotesi sulla distribuzione, non elimina ogni errore: outlier e distribuzioni diverse richiedono una verifica della qualità sul compito.

## Doppia quantizzazione: comprimere anche le scale

Le scale occupano memoria oltre ai codici dei pesi. Con blocchi di 64 pesi e una scala FP32 per blocco, l'overhead medio è $32/64=0{,}5$ bit per peso: si arriva a circa 4,5 bit anziché quattro, prima degli altri metadati.

La doppia quantizzazione comprime le scale stesse. Una configurazione usa otto bit per ciascuna scala di primo livello e una scala a 32 bit ogni 256 di queste scale. L'overhead medio diventa:

$$
\frac{8}{64}+\frac{32}{64\cdot256}\simeq0{,}127\text{ bit per peso}.
$$

Si risparmiano quindi circa 0,373 bit per peso. Applicato idealmente a quattro miliardi di pesi, questo conteggio corrisponde a circa 187 MB decimali, escludendo piccoli dati aggiuntivi. Durante la dequantizzazione si ricostruiscono prima le scale e poi i pesi. Il secondo livello comprime le costanti ausiliarie; non dimezza di nuovo i quattro bit del codice di ciascun peso.

## Come passa il gradiente attraverso una base congelata

Il training usa la base dequantizzata per produrre le attivazioni, confronta l'output con il bersaglio e propaga l'errore fino ai fattori LoRA. Consideriamo per chiarezza una singola trasformazione, senza dropout. Se $g=\partial L/\partial y$ è il gradiente della loss rispetto all'output e $s=\alpha/r$, allora:

$$
\frac{\partial L}{\partial B}=s\,g(Ax)^T,\qquad
\frac{\partial L}{\partial A}=s\,B^Tg\,x^T,
\qquad
\frac{\partial L}{\partial x}=(\widehat W_0+sBA)^Tg.
$$

Le prime due espressioni indicano come apprendere i fattori; la terza trasmette l'errore ai livelli precedenti. La base congelata partecipa dunque al calcolo del gradiente dell'input, pur non ricevendo aggiornamenti dei propri codici quantizzati. Non occorre rendere differenziabile la scelta discreta del codice per addestrare questi adattatori: quel codice resta fisso.

Gli adattatori vengono ottimizzati sulle rappresentazioni prodotte dalla base approssimata. Possono adattarne il comportamento al compito e compensare parte degli effetti della quantizzazione, ma non ricostruiscono necessariamente tutti i pesi originari. Disattivarli restituisce la base quantizzata, che può già differire da quella a precisione maggiore.

## Ottimizzatori paginati e memoria residua

Anche con pochi pesi addestrabili, il training usa attivazioni e stati dell'ottimizzatore. Adam, per esempio, conserva statistiche dei gradienti per regolare gli aggiornamenti. Sequenze più lunghe possono provocare picchi di memoria e far fallire un training che mediamente sembra entrare nella GPU.

Gli ottimizzatori paginati utilizzano la memoria unificata per trasferire pagine dei propri stati fra memoria GPU e RAM della CPU quando necessario. Le riportano sulla GPU per gli aggiornamenti. Rendono gestibili alcuni picchi, ma i trasferimenti hanno un costo e la RAM disponibile resta finita. La paginazione riguarda dove si trovano gli stati, non la precisione con cui sono rappresentati: un ottimizzatore paginato può mantenere stati a 32 bit.

Anche il gradient checkpointing riduce la memoria, con un meccanismo distinto: conserva solo parte delle attivazioni e ricalcola le altre durante la backpropagation. Si scambia quindi memoria con calcolo. Pesi a quattro bit, adattatori compatti e checkpointing non permettono di dedurre la VRAM necessaria dai soli pesi: contano anche lunghezza delle sequenze, batch e implementazione.

## Training quantizzato e quantizzazione per inferenza

Addestrare LoRA su una base a 16 o 32 bit e poi convertire il modello per l'inferenza è diverso da addestrare gli adattatori sulla base già quantizzata. Soltanto nel secondo caso l'apprendimento avviene sulle rappresentazioni approssimate usate da QLoRA. Inoltre, NF4 e una ricetta GGUF Q4_K_M sono rappresentazioni differenti, anche se entrambe impiegano pesi a bassa precisione.

Con pesi quantizzati, la fusione richiede attenzione: i codici non sono valori a cui sommare direttamente $BA$. Ricostruire i pesi, aggiungere la correzione e quantizzare nuovamente introduce un ulteriore arrotondamento. Una piccola variazione può ricadere nello stesso livello e andare persa; mantenere l'adattatore separato la somma invece a precisione maggiore durante il calcolo. Le due configurazioni devono quindi essere valutate nella forma effettivamente utilizzata.

| Procedura                    | Base                           | Che cosa viene appreso              |
| :--------------------------- | :----------------------------- | :---------------------------------- |
| Fine-tuning completo         | Aggiornabile                   | Tutti i pesi previsti dalla ricetta |
| LoRA                         | Congelata                      | Le matrici dell'adattatore          |
| QLoRA                        | Congelata e quantizzata        | Le matrici dell'adattatore          |
| Quantizzazione per inferenza | Convertita a minore precisione | Nessun nuovo adattamento implicito  |
