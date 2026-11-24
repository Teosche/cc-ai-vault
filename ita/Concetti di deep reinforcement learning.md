# Concetti di deep reinforcement learning

Capitolo: [[Reinforcement learning e processi decisionali]]

L'incontro fra modelli neurali e apprendimento da ricompense permette di adattare sistemi complessi usando un segnale di valutazione. L'adattamento di modelli linguistici tramite feedback umano o ricompense strutturate offre un esempio di questo collegamento.

## Modello di ricompensa e policy

Nell'RLHF, il modello di ricompensa valuta le risposte, mentre la policy determina il comportamento generativo da adattare. Si tratta di ruoli distinti: assegnare un punteggio non equivale a scegliere direttamente ogni token corretto. Il segnale viene utilizzato per orientare l'apprendimento della policy.

Il reward può anche essere collegato a verifiche esterne di un risultato. Il ricorso a un verificatore cambia ciò che viene premiato, ma non elimina la necessità di controllare che il criterio corrisponda al compito. Un segnale interno, per esempio legato alle distribuzioni prodotte, valuta invece una proprietà diversa.

## Portata del collegamento con il deep learning

Un modello preaddestrato può essere ulteriormente adattato nel corso della vita dell'agente. La presenza di reti neurali non specifica però se il metodo utilizzi funzioni di valore, ottimizzazione diretta della policy o altre procedure. Queste distinzioni richiedono una trattazione algoritmica dedicata.

Perciò RLHF, [[Deep Q-Networks]], [[Policy Gradient Methods]] e [[Proximal Policy Optimization]] non vanno utilizzati come sinonimi. I primi contenuti disponibili forniscono un esempio d'impiego del rinforzo nei modelli neurali, non una spiegazione completa delle famiglie del DRL.
