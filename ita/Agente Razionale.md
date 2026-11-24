---
tags:
  - agenti-razionali
  - sistemi-agentici
  - cognitive
---
Con **agente** si indica una qualsiasi entità capace di percepire il proprio **[[Tassonomia degli Ambienti|ambiente]]** attraverso dei **[[Architettura e Funzionamento degli Agenti Intelligenti|sensori]]**, e di agire su di esso attraverso degli **[[Architettura e Funzionamento degli Agenti Intelligenti|attuatori]]**.

Volendo dare una definizione strutturale, e non comportamentale, un'**agent architecture** è data da:

*agent = architecture + program*

Dove con *"architettura"* si definisce il dispositivo fisico che fornisce i dati dal mondo al *"programma"*, ovvero la logica interna che mappa le percezioni in azioni concrete.

Un **agente razionale** seleziona sempre l'azione che massimizza la sua **misura di prestazione**, ovvero il criterio per definire il successo, basandosi sulle sequenze di percezioni passate e sulle conoscenze incorporate di cui dispone.

*Essere razionali non significa essere onniscenti*: la razionalità massimizza le prestazioni attese, non quelle effettive, e comporta spesso la necessità di esplorare e di imparare.

L'**onniscenza** richiede la conoscenza dell'esito reale ed effettivo delle proprio azioni, il che è impossibile nella realtà.
La **razionalità** dipende invece unicamente dalla sequenza di  percetti fino a quel momento. L'agente razionale non è esente da errori dovuti ad eventi stocastici, ma sceglie la decisione migliore rispetto alla conoscenza disponibile.

Un **sistema agentico** rappresenta un'insieme di agenti che pianificano ed eseguono compiti complessi senza la necessità di un input umano costante.
Prendono decisioni e perseguono obiettivi partendo da specifiche ad alto livello, operano con diversi gradi di autonomia e possono adattarsi al cambiamento.

Il processo decisionale è denominato **[[Architettura e Funzionamento degli Agenti Intelligenti|Perception-Action-Cycle]].**

### **Concetti chiave**

- **Percept**
  L'input sensoriale ricevuto dall'agentei in un singolo istante temporale.
- **Percept Sequence**
  La cronologia completa di tutto ciò che l'agente ha percepito dall'inizio della sua esecuzione.
- **Agent Function**
  La mappa matematica astratta che associa a qualsiasi sequenza di percetti un'azione.
- **Agent Program**
  Concreta implementazione del sistema fisico, implementa le agent functions.
- **Control Policy for the Agent**
  Termine utilizzato per indicare la funzione o il meccanismo decisionale che mappa l'input dei sensori all'output degli attuatori.
- **Agent Program**
  L'implementazione concreta della funzione dell'agente eseguita su un'architettura computazionale.
- **Information Gathering**
  L'esecuzione di azioni al fine di modificare i percetti futuri.
- **Learning**
  La capacità di imparare il più possibile da ciò che si percepisce per adattarsi all'ambiente.
- **Autonomy**
  *Nella misura in cui un agente si affida alla conoscenza pregressa del suo progettista anziché alle proprie percezioni, diciamo che l'agente manca di autonomia. Un agente razionale dovrebbe essere autonomo: dovrebbe apprendere ciò che può per compensare una conoscenza pregressa parziale o errata.*
