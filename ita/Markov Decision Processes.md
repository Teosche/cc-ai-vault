# Markov Decision Processes

Capitolo: [[Reinforcement learning e processi decisionali]]

Un problema decisionale sequenziale collega lo stato in cui si trova un agente, l'azione scelta, lo stato successivo e il feedback ricevuto. La scelta corrente può influenzare le possibilità future; il suo valore non può quindi essere giudicato sempre dal solo risultato immediato.

## Stato e dipendenza dalla storia

L'idea markoviana è utilizzare uno stato che renda disponibile l'informazione necessaria per descrivere l'evoluzione successiva. In un modello decisionale, il passaggio successivo viene considerato rispetto allo stato corrente e all'azione. Non è l'affermazione che qualsiasi osservazione sensoriale sia già uno stato completo: in un ambiente parzialmente osservabile, l'input corrente può essere insufficiente.

Un [[Model-Based Reflex Agents|agente riflesso basato su modello]] aggiorna una descrizione interna usando percetti precedenti e conoscenza dell'ambiente, così da conservare informazioni che l'osservazione corrente non rende disponibili.

## Azioni, ricompense e preferenze

Il ciclo di apprendimento comprende azioni e ricompense. Definire il feedback è una scelta centrale: una misura poco adeguata può favorire comportamenti che aumentano il punteggio senza soddisfare l'obiettivo desiderato. Anche attribuire una ricompensa alle azioni che hanno contribuito a produrla può essere difficile quando gli effetti sono ritardati.

Negli [[Utility-Based Agents|agenti basati su utilità]], la funzione di utilità esprime quanto siano desiderabili gli esiti, mentre le probabilità rappresentano l'incertezza sulla loro comparsa. La scelta considera entrambi: un risultato molto desiderabile ma improbabile deve essere confrontato con gli altri esiti possibili e con il loro valore.
