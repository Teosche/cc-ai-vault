# Deep Q-Networks

Capitolo: [[Reinforcement learning e processi decisionali]]

Una Deep Q-Network, o DQN, usa una rete neurale per approssimare $Q(s,a)$, il ritorno atteso scegliendo un'azione e continuando secondo il comportamento obiettivo. Nella forma comune per azioni discrete, la rete riceve lo stato e restituisce un valore per ogni azione. Un agente di gioco può ricevere immagini e produrre valori per «sinistra», «destra» e «fermo».

## Costruire un target da una transizione

Q-learning propone come obiettivo la ricompensa osservata più il valore della migliore azione successiva. Nella DQN stabilizzata, per una transizione $(s,a,r,s')$, il target è:

$$
y=r+\gamma(1-d)\max_bQ(s',b;\theta^-).
$$

$d$ vale uno se la transizione termina l'episodio e zero altrimenti; $\theta^-$ identifica i parametri della target network. La rete principale, con parametri $\theta$, viene aggiornata per avvicinare $Q(s,a;\theta)$ a $y$, per esempio minimizzando l'errore quadratico medio su un minibatch. Durante questo aggiornamento il target è trattato come costante.

Se la ricompensa è 1, lo stato non è terminale, $\gamma=0,9$ e la target network stima come miglior valore successivo 5, il target è 5,5. Se la rete principale prevede 2, l'ottimizzazione cerca di ridurre la differenza di 3,5. Nel deep learning la correzione riguarda i pesi condivisi, non una sola cella di tabella.

## Riutilizzare esperienze senza seguirne l'ordine

Le transizioni vengono conservate in un replay buffer e campionate per formare minibatch. Questo permette di riusare esperienze e riduce la correlazione fra esempi consecutivi. Un minibatch può contenere situazioni provenienti da momenti o episodi diversi, invece di molte immagini quasi identiche dello stesso istante di gioco.

Il replay non rende il campione automaticamente rappresentativo: esperienze importanti possono essere rare o venire eliminate quando il buffer si riempie. Poiché i dati possono essere stati prodotti da policy precedenti, il loro riuso si accorda con la natura off-policy di Q-learning.

## Rallentare il cambiamento dell'obiettivo

Usare immediatamente gli stessi pesi per previsione e target crea un obiettivo mobile. Una modifica a $Q(s,a)$ può cambiare anche il valore usato per costruire il successivo aggiornamento. La target network mantiene per un certo periodo una copia più stabile, aggiornata periodicamente dalla rete principale. Il replay affronta soprattutto correlazione e riuso dei dati; la target network affronta l'instabilità del target. Sono meccanismi complementari.

L'azione durante l'apprendimento può essere scelta con una policy epsilon-greedy, così da raccogliere anche informazioni su alternative poco provate. Al test si può valutare una policy maggiormente greedy, dichiarando la procedura.

## Campo di applicazione e limiti

DQN è naturale quando si può calcolare il massimo su un insieme discreto gestibile di azioni. Con un comando continuo, come un angolo qualsiasi del braccio, trovare il massimo non è più una semplice selezione fra poche uscite. Per questi casi sono utili altre famiglie, fra cui i [[Policy Gradient Methods]]. Approssimazione neurale, bootstrapping e apprendimento off-policy possono comunque produrre instabilità: i dispositivi di stabilizzazione migliorano la procedura senza costituire una garanzia universale.
