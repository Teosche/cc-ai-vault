# Proximal Policy Optimization

Capitolo: [[Reinforcement learning e processi decisionali]]

Proximal Policy Optimization, o PPO, cerca di migliorare una policy usando le esperienze disponibili senza modificarla in modo troppo brusco. Se una singola raccolta di dati fortunata facesse cambiare drasticamente le probabilità delle azioni, l'agente potrebbe perdere un comportamento utile. PPO-Clip affronta questo rischio modificando l'obiettivo dell'aggiornamento.

## Confrontare policy nuova e precedente

Si raccolgono traiettorie con una policy fissata $\pi_{\mathrm{old}}$ e si stimano i vantaggi $\hat A_t$. Per ogni azione osservata si calcola il rapporto:

$$
\rho_t(\theta)=\frac{\pi_\theta(a_t\mid s_t)}{\pi_{\mathrm{old}}(a_t\mid s_t)}.
$$

Un rapporto pari a uno significa probabilità invariata; 1,5 significa che la nuova policy assegna a quell'azione una probabilità una volta e mezza quella precedente. Il rapporto permette di valutare come l'aggiornamento cambi la rilevanza delle azioni contenute nel batch.

PPO-Clip massimizza la media campionaria di:

$$
\min\left(\rho_t\hat A_t,
\operatorname{clip}(\rho_t,1-\varepsilon,1+\varepsilon)\hat A_t\right).
$$

La funzione clip tronca il rapporto nell'intervallo indicato. Il minimo sceglie il contributo più prudente fra quello originale e quello limitato. La procedura usa più aggiornamenti su minibatch del batch raccolto, mantenendo fissa la policy di riferimento durante quel ciclo.

## Che cosa fa il clipping

Come esempio, poniamo $\varepsilon=0,2$ e vantaggio $\hat A=2$. Se il rapporto cresce a 1,5, il prodotto normale vale 3, ma quello troncato vale 2,4: l'obiettivo usa 2,4. Per quel campione, aumentare ancora la probabilità oltre il rapporto 1,2 non fornisce ulteriore beneficio nel termine della policy.

Con vantaggio negativo, per esempio -2, conviene ridurre la probabilità dell'azione. Se il rapporto scende a 0,5, il prodotto normale vale -1 e quello troncato -1,6; il minimo conserva -1,6. Anche qui, andare molto oltre la riduzione prevista non offre altro vantaggio nel termine. Se invece il rapporto si muove nella direzione dannosa, il minimo conserva la penalizzazione.

Il clipping modifica l'incentivo, ma non impone un vincolo rigido su ogni probabilità. Parametri condivisi e altri termini della loss possono comunque spostarla oltre l'intervallo. Alcune implementazioni controllano anche la divergenza fra policy e interrompono gli aggiornamenti se diventa troppo grande.

## Raccolta, critic e nuovi dati

PPO viene comunemente usato come metodo on-policy actor-critic. Il critic viene addestrato su target di ritorno e aiuta a stimare i vantaggi; un termine di entropia può sostenere l'esplorazione. Dopo un numero limitato di epoche sul batch si raccolgono nuove traiettorie con la policy aggiornata. Il riuso locale del batch non equivale al replay indefinito di esperienze molto vecchie.

Nell'RLHF un modello linguistico può fungere da policy e un reward model può valutare le risposte. PPO è una possibile procedura per aggiornarlo, mentre un eventuale termine che limita la distanza da un modello di riferimento svolge un ruolo ulteriore. Né PPO né il clipping stabiliscono che il reward corrisponda a verità o comportamento desiderabile: questo dipende dalla costruzione del segnale e dalla valutazione del sistema.
