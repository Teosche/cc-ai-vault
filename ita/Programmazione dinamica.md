Capitolo: [[Reinforcement learning e processi decisionali]]

La programmazione dinamica risolve un problema decisionale riutilizzando le soluzioni dei suoi sottoproblemi. In un MDP, il valore di una situazione dipende dal valore delle situazioni raggiungibili: invece di enumerare ogni sequenza completa, si aggiornano stime che possono essere riusate da più decisioni. Per questi metodi si assumono note le transizioni e le ricompense dell'ambiente.

## Valutare una policy

La policy evaluation risponde alla domanda: quanto ritorno si ottiene seguendo questa policy? Si inizializzano i valori, per esempio a zero, e si applica ripetutamente l'**equazione di Bellman**, mediando le conseguenze delle azioni secondo la policy. Ogni aggiornamento incorpora un'informazione sul seguito.

Consideriamo un esempio deterministico con due stati non terminali, A e B. Da B la policy raggiunge il termine con ricompensa 5; da A raggiunge B con ricompensa 0. Il valore terminale è zero e $\gamma=0,9$. Partendo da valori nulli, un aggiornamento sincrono assegna 5 a B e ancora 0 ad A, perché usa le vecchie stime. Al giro seguente A assume valore $0+0,9\cdot5=4,5$. Il valore dell'esito si è propagato indietro senza simulare separatamente tutte le storie possibili.

## Migliorare la policy

La policy improvement confronta le azioni usando i valori appena stimati e sceglie quelle con il miglior ritorno atteso. Se in A esiste anche un'azione che termina subito con ricompensa 2, il confronto è fra 2 e 4,5: conviene passare da B.

**La policy iteration alterna valutazione della policy e miglioramento**. Si parte da un comportamento, se ne stimano le conseguenze e lo si rende greedy rispetto a quella stima; poi si valuta il nuovo comportamento. Nel caso tabellare finito, con le condizioni consuete del problema scontato, questo processo porta a una policy ottima. La valutazione riguarda il comportamento attuale; il miglioramento lo cambia.

## Aggiornare direttamente il valore ottimo

La value iteration incorpora invece la scelta dell'azione migliore in ciascun aggiornamento:

$$
V_{k+1}(s)=\max_a\sum_{s'}P(s'\mid s,a)
\left[r(s,a,s')+\gamma V_k(s')\right].
$$

$k$ indica il giro di aggiornamento. Si prova ciascuna azione nel modello, si valuta il seguito attraverso $V_k$ e si conserva il massimo. Dopo aver ottenuto valori sufficientemente stabili, si estrae la policy scegliendo l'azione che massimizza la stessa espressione. Non serve completare una valutazione separata per ogni policy intermedia.

## Perché l'esperienza cambia il problema

Questi aggiornamenti sommano sulle possibilità descritte da un modello noto. Se l'agente non conosce $P$ o le ricompense, deve imparare dall'interazione o stimare quel modello. Gli [[Algoritmi fondamentali di reinforcement learning|aggiornamenti temporal-difference]] utilizzano transizioni campionate al posto della somma completa. Inoltre, anche con un modello noto, uno spazio di stati enorme può rendere troppo costoso aggiornare una tabella: da qui l'uso di approssimatori.
