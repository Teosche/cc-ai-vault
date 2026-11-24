---
tags:
  - ragionamento
  - incertezza
  - probabilita
  - decisioni
---

Capitolo: [[Ragionamento, inferenza causale e spiegazione]]

Il **ragionamento in condizioni di incertezza** riguarda situazioni nelle quali l'agente non dispone di una descrizione completa e certa dello stato del mondo o delle conseguenze delle proprie azioni. **L'incertezza non sospende la necessità di agire**: un sistema deve comunque utilizzare l'evidenza disponibile, aggiornare le proprie conoscenze e scegliere fra alternative i cui risultati possono non essere garantiti.

## Da dove nasce l'incertezza

L'incertezza può derivare da quattro sorgenti principali: i **limiti** dei sensori, la **presenza di avversari** che rendono difficile interpretare ciò che accade, la **natura stocastica dell'ambiente** e l'**ignoranza** dovuta a conoscenze incomplete. Sono condizioni differenti, che possono presentarsi insieme.

Un sensore può essere rumoroso oppure non rendere accessibile una parte dello stato. Un avversario può nascondere informazioni o rendere il proprio comportamento meno prevedibile. Un ambiente stocastico può avere più esiti possibili anche quando si conosce la situazione di partenza. L'ignoranza può invece riguardare regolarità che sarebbero conoscibili, ma che l'agente non ha ancora appreso.

L'**osservabilità** e la **conoscenza delle leggi dell'ambiente** sono proprietà distinte. Un agente può conoscere le regole di un gioco di carte senza vedere le carte coperte; al contrario, può vedere l'intera schermata di un nuovo videogioco senza sapere quali effetti producano i comandi. Conoscere il modello e osservare lo stato sono dunque due problemi distinti.

Anche determinismo e osservabilità non coincidono. Un ambiente deterministico può risultare imprevedibile per chi non osserva tutte le variabili rilevanti. Si può inoltre distinguere fra un modello *nondeterministico*, che ammette più esiti senza necessariamente quantificarli, e un modello *stocastico*, che assegna probabilità esplicite agli esiti. La distinzione riguarda la descrizione delle alternative: elencare ciò che può accadere non equivale a specificare quanto sia probabile.

## Perché la razionalità non richiede certezza

La definizione di [[Agente Razionale|agente razionale]] dipende dalla misura di prestazione, dalla conoscenza iniziale, dalle azioni disponibili e dalla sequenza di percetti accumulata. L'agente deve scegliere l'azione che ci si attende massimizzi la prestazione sulla base di queste informazioni. ***La razionalità riguarda quindi la qualità della scelta rispetto all'evidenza disponibile***, non la conoscenza anticipata dell'esito effettivo.

La razionalità va distinta dall'onniscienza: una scelta ragionevole può avere un risultato sfavorevole a causa di eventi non prevedibili. Non si può giudicare la correttezza della decisione soltanto dal risultato di una singola esecuzione. Allo stesso tempo, la mancanza di certezza non giustifica ignorare informazioni che si potrebbero raccogliere: raccogliere informazioni e apprendere possono essere necessari per scegliere razionalmente.

Nell'esempio del taxi che non vede oltre un camion fermo, l'agente può non conoscere la causa dell'arresto del traffico. Mantiene quindi una descrizione interna inevitabilmente incompleta, ma deve continuare a decidere. Un modello interno non elimina necessariamente l'incertezza; permette di organizzarla usando la storia dei percetti e la conoscenza disponibile.

## Probabilità come rappresentazione e aggiornamento della conoscenza

La probabilità permette di trattare misure incerte e conoscenze incomplete. La regola di Bayes consente di aggiornare le probabilità alla luce di nuove evidenze, mentre le reti bayesiane forniscono un formalismo per rappresentare e utilizzare conoscenza incerta.[

Indicando con $H$ un'ipotesi e con $E$ un'evidenza, per $P(E)>0$:

$$
P(H\mid E)=\frac{P(E\mid H)P(H)}{P(E)}.
$$

$P(H)$ è la probabilità dell'ipotesi prima di incorporare l'evidenza considerata; $P(E\mid H)$ esprime quanto l'evidenza sarebbe compatibile con l'ipotesi; $P(H\mid E)$ è la probabilità aggiornata. Il denominatore normalizza il risultato. La formula si ottiene uguagliando le due scritture della probabilità congiunta: $P(H\cap E)=P(E\mid H)P(H)=P(H\mid E)P(E)$.

Il punto concettuale è che la compatibilità di un'osservazione con un'ipotesi non basta da sola a stabilire la probabilità dell'ipotesi. Occorre considerare anche la probabilità iniziale e le spiegazioni alternative dell'evidenza. Per questo $P(E\mid H)$ e $P(H\mid E)$ non sono intercambiabili.

Come **esempio numerico costruito per questa nota**, supponiamo che un componente sia guasto con probabilità $0{,}10$, che un allarme scatti nel $90\%$ dei guasti e anche nel $20\%$ dei casi senza guasto. Assumendo che «guasto» e «non guasto» esauriscano le possibilità, la probabilità dell'allarme è $0{,}90\cdot0{,}10+0{,}20\cdot0{,}90=0{,}27$. Dopo l'allarme, la probabilità di guasto è dunque $0{,}09/0{,}27=1/3$. L'allarme aumenta la probabilità iniziale, ma non la porta al $90\%$: quella percentuale descriveva la probabilità dell'allarme dato il guasto, non l'inversa.

## Dalle credenze alla scelta dell'azione

Stimare che cosa sia probabile e decidere che cosa convenga fare sono operazioni collegate ma differenti. La teoria delle decisioni combina probabilità e utilità. Le probabilità riguardano gli esiti; l'utilità rappresenta quanto tali esiti siano desiderabili.[

Un agente può avere più obiettivi in conflitto, come velocità e sicurezza, oppure obiettivi che non possono essere raggiunti con certezza. La funzione di utilità permette di confrontare queste alternative. L'**utilità attesa** è una media delle utilità degli esiti possibili, pesata con le rispettive probabilità.

Come formalizzazione di questa definizione, per un insieme finito di esiti $s'$ e informazioni correnti $I$:

$$
EU(a\mid I)=\sum_{s'}P(s'\mid a,I)\,U(s'),
\qquad
 a^*\in\operatorname*{arg\,max}_{a}EU(a\mid I).
$$

Il criterio consiste nel confrontare le azioni in base alle conseguenze previste e al loro valore. Non è una procedura che renda automaticamente disponibili il modello, le probabilità o l'utilità corretta. Costruire e mantenere tali rappresentazioni è già un problema complesso.

## Informarsi, apprendere e aggiornare il comportamento

La raccolta di informazioni può essere essa stessa un'azione razionale. Cercare un nuovo percetto può migliorare le decisioni successive; apprendere può compensare conoscenze iniziali incomplete o scorrette. Il ragionamento sotto incertezza non coincide quindi con una scelta effettuata una volta per tutte: può inserirsi in un ciclo di osservazione, revisione e azione.

Il sistema esperto MYCIN trattava conoscenze mediche incerte mediante *certainty factors*. Questi fattori appartengono a un particolare calcolo dell'incertezza, distinto dagli approcci probabilistici. Non è corretto interpretarli automaticamente come probabilità bayesiane soltanto perché esprimono un grado di supporto.

Per studiare il tema, la distinzione centrale rimane fra ciò che l'agente osserva, ciò che inferisce e ciò che decide. Le tre componenti si influenzano, ma non sono equivalenti: una credenza molto probabile non determina da sola quale azione sia migliore, così come un'azione razionale non garantisce un esito favorevole in ogni singolo caso.
