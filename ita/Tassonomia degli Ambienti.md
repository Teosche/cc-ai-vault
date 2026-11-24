---
tags:
  - tassonomie
  - ambienti
  - agenti
  - classificare
---

Source: 02_Introductiontoagents
## **Fully** vs **Partially Observable**

Un'ambiente si dice **fully** quando ciò che l'agente vede al suo interno è completamente sufficiente per arrivare ad una decisione ottimale.

Un'ambiente si dice **parzialmente osservabile** quando ciò che l'agente vede al suo interno *non* è sufficiente per arrivare ad una decisione ottimale.

Come strutturare la memoria è un problema chiave: gli [[Hidden Markov Models]] permettono di aggiornare una distribuzione di probabilità sugli stati nascosti usando la sequenza di osservazioni.

## **Deterministic** vs **Stochastic**

Un'ambiente si dice **deterministico** quando le azioni di un agente all'interno di esso determinano in modo univoco il risultato.  

 I giochi con i dadi sono stocastici. 
 Prendendo per esempio  il backgammon, sebbene si abbia la possibilità d muovere i pezzi in modo deterministico, l'esito di un'azione implica anche il lancio dei dadi, che non è possibile prevedere.

## **Continuous** vs **Discrete**

Un'ambiente **continuo** indica che lo spazio delle azioni possibili o delle percezioni può essere infinito.

Un'ambiente **discreto** indica un ambiente in cui si ha un numero finito di scelte di azione ed un numero finito di elementi che si possono percepire.

### **Adversarial** vs. **Benign**

Con ambiente **avversariale** si intende un ambiente ove esiste un'avversario che ti osserva attivamente e che contrasta il raggiungimento di un'obiettivo.

Con ambiente **benigno** s'intende un'ambiente che non ha un obiettivo di contrasto.

### **Episodic** vs **Sequential**

In un'ambiente **episodico** l'esperienza dell'agente è suddivisa in task atomici ed indipendenti. L'agente riceve una singola percezione e compie una singola azione; l'episodio successivo non dipende in alcun modo dalle azioni passate. L'agente in questo ambiente non pianifica a lungo termine.

In un'ambiente **sequenziale** la decisione presa nel momento presente può influenzare gli stati futuri dell'ambiente e tutte le decisioni successive.

### **Single-Agent** vs **Multi-Agent**

In un contesto **single-agent**, l'agente opera isolato nel suo ambiente, concentrandosi esclusivamente sulla propria performace senza entità "intelligenti" che interagiscono con lui.

In un contesto **multi-agent**, l'agente agisce in un'ambiente condiviso con altre entità autonome.
Questi ambienti si suddividono in due sottocategorie:
- Competitivi:
  L'obiettivo dell'avversario minimizza direttamente l'obiettivo attuale.
- Parzialmente Cooperativi
  L'obiettivo comune massimizza la performance di tutti, pur essendoci elementi di competizione. In questi ambienti il comportamento [[Agente Razionale|razionale]] fa spesso emergere la necessità di comunicare tra agenti.
