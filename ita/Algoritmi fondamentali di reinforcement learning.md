Capitolo: [[Reinforcement learning e processi decisionali]]

Quando transizioni e ricompense non sono note, l'agente apprende dalle sequenze osservate. Ogni transizione fornisce uno stato $s$, un'azione $a$, una ricompensa $r$ e uno stato successivo $s'$. La difficoltà è trasformare questi campioni in una stima delle conseguenze future e in una policy migliore, senza confondere un esito fortunato con una regolarità affidabile.

## Monte Carlo e temporal-difference

Monte Carlo stima il valore attraverso i ritorni di episodi completati. Se una stessa situazione compare in più prove, si possono mediare i ritorni osservati da quel punto. Il metodo non richiede un modello delle transizioni, ma deve attendere il seguito dell'episodio e può ricevere segnali molto variabili.

Temporal-difference, o TD, aggiorna invece una stima usando la ricompensa immediata e la stima dello stato successivo:

$$
V(s)\leftarrow V(s)+\alpha\left[r+\gamma V(s')-V(s)\right].
$$

La parentesi è l'errore TD: confronta la vecchia previsione con il target $r+\gamma V(s')$. Il passo $\alpha$ regola quanto correggerla. Utilizzare una stima per costruirne un'altra prende il nome di bootstrapping e permette aggiornamenti prima della fine dell'episodio, introducendo però dipendenza dall'accuratezza delle stime correnti.

## Da una stima di valore alla scelta

Per scegliere senza conoscere le transizioni è utile stimare $Q(s,a)$, il valore di ogni azione nello stato. Una policy greedy sceglie il valore massimo, ma può ignorare azioni ancora poco provate. Una policy epsilon-greedy sceglie con probabilità $\varepsilon$ un'azione casuale e altrimenti un'azione con valore massimo. L'esplorazione raccoglie informazioni; lo sfruttamento usa quelle già disponibili.

SARSA aggiorna $Q(s,a)$ verso $r+\gamma Q(s',a')$, dove $a'$ è l'azione effettivamente selezionata dalla policy nel nuovo stato. Il nome richiama la sequenza stato, azione, ricompensa, stato, azione. È on-policy perché apprende il valore del comportamento seguito, compresa la sua esplorazione.

Q-learning usa invece il valore massimo disponibile nel nuovo stato:

$$
Q(s,a)\leftarrow Q(s,a)+\alpha\left[r+\gamma\max_bQ(s',b)-Q(s,a)\right].
$$

L'azione $b$ rappresenta la migliore continuazione stimata, anche se l'agente esplorativo sceglierà altro. È quindi off-policy: la policy appresa dal target può differire da quella che produce i dati.

## Un aggiornamento svolto

Supponiamo che $Q(s,a)=2$, la ricompensa osservata sia 1 e nel nuovo stato i valori siano 3 e 5. Con $\gamma=0,9$, Q-learning costruisce il target $1+0,9\cdot5=5,5$. Con $\alpha=0,1$, la nuova stima è $2+0,1(5,5-2)=2,35$. Non sostituisce il valore con l'esito di una sola prova: lo corregge parzialmente.

Se la policy esplorativa sceglie nel nuovo stato l'azione di valore 3, SARSA usa il target 3,7 e aggiorna a 2,17. La differenza riflette ciò che i due metodi stanno stimando. Se la transizione è terminale, il termine di valore futuro è zero.

Questi esempi tabellari chiariscono i meccanismi. Le garanzie di convergenza richiedono condizioni su visite alle coppie stato-azione e passi di apprendimento; non si trasferiscono automaticamente alle reti neurali delle [[Deep Q-Networks]].
