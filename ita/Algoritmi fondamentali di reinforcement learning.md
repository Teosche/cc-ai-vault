# Algoritmi fondamentali di reinforcement learning

Capitolo: [[Reinforcement learning e processi decisionali]]

Il reinforcement learning riguarda un agente che interagisce con un ambiente e utilizza ricompense per migliorare il comportamento. Non riceve necessariamente, per ogni situazione, un'etichetta che indichi l'azione corretta. Deve invece collegare le scelte compiute alle conseguenze osservate.

## Feedback e attribuzione del risultato

A uno stato segue un'azione, poi un nuovo stato e una ricompensa. Una difficoltà è definire un feedback coerente con il compito; un'altra è capire quali scelte precedenti abbiano contribuito a un risultato. Una ricompensa tardiva può dipendere da una sequenza di azioni, non soltanto dall'ultima.

L'interazione può essere illustrata attraverso un simulatore, ma non è corretto trasformare l'esempio in un requisito universale: il principio riguarda apprendimento dall'esperienza, non l'obbligo di usare sempre un simulatore o l'impossibilità di utilizzare esperienze registrate.

## Prestazione, apprendimento ed esplorazione

Nell'architettura generale dei [[Learning Agents]], l'elemento di prestazione seleziona azioni e l'elemento di apprendimento ne modifica il funzionamento. Il critico fornisce una valutazione e il generatore di problemi suggerisce azioni esplorative. Ripetere soltanto ciò che appare già conveniente può impedire di scoprire alternative migliori.

Questa separazione aiuta a comprendere il problema, ma non specifica ancora un particolare algoritmo RL. Un criterio di valutazione, una politica di scelta e una regola di aggiornamento devono essere descritti esplicitamente prima di poter parlare di una procedura completa.

## Preferenze umane come segnale

Nell'RLHF, un modello di ricompensa assegna punteggi alle risposte e una policy viene adattata per aumentare la ricompensa. Il sistema apprende quindi rispetto al feedback rappresentato da quel modello, che non coincide automaticamente con verità o qualità complessiva delle risposte.

La distinzione fra obiettivo dichiarato e misura effettivamente utilizzata rimane fondamentale anche per modelli linguistici. Ottenere un punteggio più alto non basta a dimostrare che ogni comportamento sia appropriato.
