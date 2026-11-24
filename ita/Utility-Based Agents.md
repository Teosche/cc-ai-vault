---
tags:
  - agenti-razionali
  - funzioni-di-utilita
  - decisioni
  - ottimizzare
---
L'utilizzo di **Utility-Based Agents** fornisce soltanto una distinzione binaria grezza tra stati: 
- Stati felici -> Obiettivo raggiunto.
- Stati infelici -> Obiettivo non raggiunto.

In  ambienti complessi esistono molteplici sequenze di azioni che permettono di raggiungere l'obiettivo, ma alcune sono più veloci, più sicure, più economiche o più confortevoli di altre. Per confrontare il gradi di desiderabilità dei diversi stati si introduce il concetto economico e matematico di **utilità**.

La **Funzione di Utilità** **$U(s)$** internizza la misura di prestazione esterna, assegnando un valore numerico reale ad ogni stato $s$ per misurarne la preferenza relativa.

In contesti stocastici o parzialmente osservabili, l'esito di un'azione non è certo. Un'agente razionale basato su utilità seleziona l'azione che massimizza l'**utilità attesa**, calcolata ponderando l'utilità di ciascun possibile stato f inale $s'$ con la probabilità $P(s' \mid s, a)$.

$$\text{Expected Utility}(a \mid s) = \sum_{s'} P(s' \mid s, a) \cdot U(s')$$
### Vantaggi
- **Risoluzione dei conflitti tra obiettivi**
  Gestisce in modo razionale il trade-off tra obiettivi in contrasto.
- **Decisioni sotto incertezza**
  Permette di prendere decisioni ottimali quando ci sono più obiettivi da perseguire, nessuno dei quali è raggiungibile con certezza
- **Agenti Model-Free**
  Un'agente basato su utilità può essere basato su modello, oppure può imparare direttamente qual è l'azione migliore in una determinata situazione senza dover costruire un modello esplicito del mondo.
