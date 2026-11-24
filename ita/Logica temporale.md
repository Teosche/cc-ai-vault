# Logica temporale

Capitolo: [[Logica del primo ordine, programmazione logica e altre logiche]]

La **logica temporale** permette di esprimere come la verità di una condizione si distribuisce nel tempo. Dire «il robot è fermo» descrive uno stato; dire «il robot resterà sempre fermo», «prima o poi si fermerà» o «si fermerà al prossimo passo» impone condizioni diverse sulla sua evoluzione. Gli operatori temporali rendono esplicite queste differenze.

È una famiglia di formalismi. Qui partiamo dalla **Linear Temporal Logic**, abbreviata **LTL**, interpretata lungo una singola sequenza di stati. In questa versione gli enunciati restano veri o falsi: la dimensione aggiunta è il tempo, non il grado di verità della [[Logica fuzzy]].

## Dagli stati alle sequenze

Un'esecuzione viene rappresentata come una **traccia** $s_0,s_1,s_2,\ldots$, in cui ogni stato specifica quali proposizioni siano vere. Assumiamo il modello standard di LTL con passi discreti e tracce infinite. Un sistema che termina può essere rappresentato ripetendo indefinitamente lo stato finale. Un passo non corrisponde necessariamente a un secondo: può rappresentare una transizione del sistema.

Consideriamo una macchina che riceve una richiesta, lavora e completa l'operazione. Usiamo tre proposizioni: $r$ significa «c'è una richiesta», $b$ «la macchina sta lavorando» e $d$ «l'operazione è completata».

| Stato | $r$ | $b$ | $d$ |
| :--- | :---: | :---: | :---: |
| $s_0$ | vero | falso | falso |
| $s_1$ | falso | vero | falso |
| $s_2$ | falso | falso | vero |
| $s_3$ e tutti i successivi | falso | falso | falso |

Una formula temporale si valuta a partire da una posizione della traccia. Nello stato $s_0$, $d$ è falsa, ma possiamo già domandarci se diventerà vera in seguito. La risposta riguarda l'intera continuazione, non soltanto lo stato corrente.

## Gli operatori fondamentali

L'operatore **$X$**, *next*, guarda al passo immediatamente successivo. $Xd$ è vera in una posizione se $d$ è vera nello stato seguente. Nella traccia è falsa in $s_0$ e vera in $s_1$.

L'operatore **$F$**, *eventually*, afferma che una condizione vale almeno una volta da adesso in avanti. $Fd$ è vera in $s_0$, perché $d$ diventa vera in $s_2$. Non impone un limite massimo all'attesa e, nella semantica adottata, include anche lo stato corrente: è vera anche in $s_2$.

L'operatore **$G$**, *globally*, richiede che una condizione valga nello stato corrente e in tutti i successivi. $Gd$ è falsa in $s_0$: il completamento non è una condizione che rimane sempre vera. La distinzione fra $d$, $Fd$ e $Gd$ è quindi la distinzione fra «adesso», «almeno una volta» e «sempre».

L'operatore **$U$**, *until*, combina due condizioni. $p\,U\,q$ richiede che prima o poi $q$ sia vera e che $p$ rimanga vera in tutti gli stati precedenti a quel momento. In $s_1$, $b\,U\,d$ è vera: la macchina lavora fino al completamento in $s_2$. Se il completamento non arrivasse mai, la formula sarebbe falsa anche se la macchina continuasse a lavorare per sempre. Nello stato in cui $q$ si verifica non è necessario che valga ancora $p$; se $q$ vale già adesso, l'obbligo su $p$ non si applica.

## Esprimere requisiti di un sistema

Supponiamo che una macchina non debba mai lavorare con una protezione aperta. Indicando con $a$ la proposizione «la protezione è aperta», il requisito si scrive:

$$
G\neg(b\land a).
$$

È una proprietà di **safety**: vieta che accada una situazione indesiderata. Per dimostrarne la violazione basta trovare uno stato raggiungibile in cui lavoro e protezione aperta coincidano. Tuttavia una macchina sempre inattiva soddisferebbe questo requisito senza svolgere il proprio compito.

Occorre allora anche una proprietà di **liveness**, che richieda un progresso. «Ogni volta che c'è una richiesta, prima o poi l'operazione viene completata» si esprime con:

$$
G(r\rightarrow Fd).
$$

In ogni posizione in cui $r$ è vera deve esistere un completamento da quella posizione in avanti. La formula non stabilisce «entro cinque secondi»: per vincoli quantitativi servono ulteriori strumenti, come logiche temporali metriche. Inoltre, se sono ammesse richieste concorrenti, questa formula non associa ogni risposta alla sua richiesta: per quel requisito occorre un modello che ne distingua le identità.

Anche l'ordine degli operatori conta. $GFd$ significa che da ogni posizione ci sarà ancora un completamento: su una traccia infinita, $d$ deve verificarsi infinite volte. $FGd$ significa invece che prima o poi si raggiunge un punto da cui $d$ resta sempre vera. Una lampadina che continua ad accendersi e spegnersi soddisfa $GF\,accesa$, ma non $FG\,accesa$.

## Futuri alternativi e verifica

Un sistema può avere più esecuzioni possibili, a causa di scelte interne o eventi esterni. Una formula LTL descrive una traccia; nella verifica di un sistema si richiede normalmente che valga su **tutte le tracce ammesse** a partire dagli stati iniziali. Osservare una singola esecuzione corretta non basta a dimostrare questa proprietà.

La **Computation Tree Logic**, o **CTL**, permette invece di combinare operatori temporali con quantificatori espliciti sui percorsi: $A$ significa «per tutti i percorsi» ed $E$ «esiste un percorso». $AFd$ richiede che ogni possibile esecuzione raggiunga un completamento; $EFd$ richiede che ne esista almeno una capace di raggiungerlo. La possibilità di riuscire e la garanzia di riuscire sono requisiti differenti.

Il **model checking** verifica se un modello di transizione soddisfa una formula e, quando la proprietà non vale, può produrre un controesempio. La garanzia riguarda il modello e le ipotesi considerate: se mancano comportamenti reali, la verifica non li copre. Inoltre lo spazio degli stati può crescere enormemente con il numero di componenti. La logica temporale rende precisi i requisiti sull'evoluzione, ma non elimina il costo della verifica.
