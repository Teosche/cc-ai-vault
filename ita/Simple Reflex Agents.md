---
tags:
  - agenti-riflessi
  - ambienti
  - reagire
---

Gli **agenti a riflesso semplice** costituiscono la forma più elementare di agente.
Essi selezionano esclusivamente le azioni basandosi esclusivamente sul percetto corrente, ignorando completamente l'intera cronologia dei percetti passati.

### Condition-Action Rules

Le decisioni vengono prese mediante connessioni dirette chiamate **regole condizione-azione**

	if (condizione) then (azione)

```python
function SIMPLE-REFLEX-AGENT(percept) returns an action
    persistent: rules, insieme di regole condizione-azione
    
    state  <- INTERPRET-INPUT(percept)
    rule   <- RULE-MATCH(state, rules)
    action <- rule.ACTION
    return action
```

### Limiti

- **Requisito di Osservabilità Completa**
  L'agente riflesso semplice funziona correttamente solo se l'ambiente è **completamente osservabile**.

  Se l'ambiente è **parzialmente osservabile** il percetto corrente non contiene le informazioni sufficienti per determinare lo stato reale del mondo.

- **Cicli Infiniti**
  In ambienti parzialmente osservabili, questo tipo di agenti deterministici cadono inevitabilmente in loop infiniti.

	#### Soluzione
	Introducendo **casualità** nella selezione della azioni si possono evitare i loop infiniti. Ciò interrompe la ciclicità delle reazioni deterministiche quando un'agente si trova bloccato a causa della sua limitata capacità di percepire l'ambiente.
