Capitolo: [[Reinforcement learning e processi decisionali]]

Un Markov Decision Process, o MDP, descrive un **ambiente in cui un agente prende decisioni successive e ne valuta le conseguenze**. È formato da stati $S$, azioni $A$, probabilità di transizione $P(s'\mid s,a)$, ricompense e un fattore di sconto $\gamma$. Lo stato rappresenta la situazione rilevante; l'azione influenza la distribuzione della situazione successiva; la ricompensa attribuisce un valore immediato alla transizione.

## Che cosa significa «markoviano»

**La proprietà di Markov richiede che lo stato corrente contenga l'informazione necessaria a prevedere la transizione successiva**, data l'azione. Una volta noto tale stato, *la storia precedente non aggiunge informazione rilevante per quella previsione*. Questo è un requisito sulla rappresentazione, non l'affermazione che ogni dato sensoriale sia sufficiente.

Una fotografia di un robot in movimento può mostrare la posizione ma non la velocità. Due situazioni visivamente simili possono allora evolvere diversamente. Includere la velocità nello stato può risolvere questa ambiguità; in un ambiente parzialmente osservabile occorre mantenere memoria o una stima dello stato nascosto.

## Policy, ricompensa e ritorno

La policy $\pi(a\mid s)$ assegna probabilità alle azioni nello stato $s$; una policy deterministica ne seleziona una sola. La ricompensa $R_{t+1}$ arriva dopo l'azione $A_t$. Il ritorno scontato è:

$$
G_t=R_{t+1}+\gamma R_{t+2}+\gamma^2R_{t+3}+\cdots.
$$

Un $\gamma$ vicino a zero favorisce il breve termine; uno vicino a uno attribuisce più peso al seguito. Con ricompense limitate e $\gamma<1$, il ritorno rimane finito anche in processi che possono continuare indefinitamente. Nei compiti episodici si può usare anche $\gamma=1$ quando il ritorno è ben definito.

Come esempio didattico, un'azione può terminare subito con ricompensa 2, mentre un'altra conduce a una ricompensa 0 e, al passo seguente, a 5. Con $\gamma=0,9$ la seconda sequenza vale 4,5; con $\gamma=0,2$ vale 1. La scelta dipende quindi da come si valuta il futuro, non soltanto dal numero più grande fra le ricompense.

## Dare valore a stati e azioni

$V^\pi(s)$ è il ritorno atteso partendo da $s$ e seguendo la policy $\pi$. $Q^\pi(s,a)$ è il ritorno atteso scegliendo prima $a$ e seguendo poi $\pi$. Questi valori includono le ricompense future, mentre la ricompensa immediata riguarda una sola transizione.

Indicando con $r(s,a,s')$ la ricompensa media della transizione, l'equazione di Bellman esprime il valore attraverso il seguito:

$$
V^\pi(s)=\sum_a\pi(a\mid s)\sum_{s'}P(s'\mid s,a)
\left[r(s,a,s')+\gamma V^\pi(s')\right].
$$

Si considera ogni azione secondo la policy, ogni possibile stato successivo secondo la transizione e si sommano ricompensa immediata e valore futuro scontato. Per cercare il valore ottimo $V^*$, la media sulle azioni viene sostituita dalla scelta del massimo. Questa scomposizione rende possibili gli aggiornamenti della [[Programmazione dinamica]].

La bontà della soluzione dipende anche da stato e reward. Premiare il semplice movimento verso una meta può incoraggiare scorciatoie indesiderate; uno stato incompleto può rendere insufficiente la policy più accurata nel modello scelto.
