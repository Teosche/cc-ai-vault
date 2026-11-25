# Policy Gradient Methods

Capitolo: [[Reinforcement learning e processi decisionali]]

I policy gradient modificano direttamente una policy parametrizzata per aumentare il ritorno atteso. Una rete con parametri $\theta$ produce $\pi_\theta(a\mid s)$: per azioni discrete può fornire probabilità, per azioni continue i parametri di una distribuzione da cui campionare il comando. L'obiettivo è rendere più probabili le scelte che conducono a risultati migliori.

## Apprendere da una traiettoria

L'agente raccoglie una traiettoria seguendo la policy. Il metodo REINFORCE usa il ritorno osservato per orientare gli aggiornamenti delle probabilità delle azioni compiute. L'idea non richiede di conoscere quale azione fosse ottima: confronta il comportamento con gli esiti ottenuti.

Una forma operativa, con ritorno futuro $G_t$ e baseline $b(s_t)$, è:

$$
\Delta\theta=\alpha\sum_t\nabla_\theta\log\pi_\theta(a_t\mid s_t)
\left[G_t-b(s_t)\right].
$$

Questa espressione è una stima campionaria dell'aggiornamento nella formulazione episodica; le ponderazioni dipendono dalla convenzione scelta per l'obiettivo scontato. Il gradiente del logaritmo indica come aumentare la probabilità dell'azione osservata. Il fattore tra parentesi decide quanto rinforzarla o ridurla rispetto a un riferimento.

## Perché confrontare con una baseline

Un ritorno di 8 può essere buono in uno stato difficile e deludente in uno stato da cui normalmente si ottiene 10. Sottrarre un valore atteso permette di distinguere questi casi. Se la baseline è 6, il segnale è positivo e vale 2; se è 10, è negativo e vale -2.

Una baseline dipendente dallo stato e indipendente dall'azione non modifica, nelle condizioni del teorema, il valore atteso del gradiente. Può però ridurne la varianza: l'aggiornamento dipende meno dalla difficoltà generale della situazione. Non elimina la casualità delle traiettorie e la precisione della stima rimane importante.

## Actor e critic

Un actor-critic apprende una funzione di valore, il critic, da utilizzare come guida della policy, l'actor. La funzione vantaggio è $A^\pi(s,a)=Q^\pi(s,a)-V^\pi(s)$: misura quanto scegliere $a$ sia migliore del comportamento medio previsto in $s$.

In una procedura semplice, l'errore $r+\gamma V(s')-V(s)$ fornisce una stima del vantaggio e permette di aggiornare l'actor prima di attendere la fine dell'episodio. Il critic viene a sua volta addestrato a stimare i ritorni. Si riduce parte della variabilità di Monte Carlo, introducendo però errori legati all'approssimazione del critic e al bootstrapping.

## Potenzialità e stabilizzazione

Rappresentare direttamente una distribuzione evita di enumerare tutti i comandi continui e permette comportamenti stocastici. Tuttavia, un aggiornamento troppo grande può rendere la nuova policy molto diversa da quella che ha prodotto i dati, peggiorando le prestazioni. [[Proximal Policy Optimization]] affronta proprio questo problema limitando l'incentivo a modifiche eccessive durante l'ottimizzazione di un batch.
