---
tags:
  - agenti-razionali
  - stati-interni
  - modelli
  - percepire
---

Per superare i limiti dell'**osservabilità parziale** l'agente deve tener traccia della porzione di mondo che non può vedere in questo momento. Esso quindi mantiene uno **stato interno** che dipende dalla conologia dei percetti passati.

L'aggiornamento dello stato interno richiede di codificare nel programma due tipi  di conoscenza:

- **Transition Model**
  Informazioni su come il mondo cambia nel tempo.
  Comprende:
	  - Effetti delle azioni dell'agente sull'ambiente.
	  - Evoluzione dell'ambiente indipendentemente dall'agente.
- **Sensor Model**
  Informazioni su come lo stato del mondo si riflette nei percetti ricevuti dai sensori

```python
function MODEL-BASED-REFLEX-AGENT(percept) returns an action
    persistent: state, una descrizione dello stato interno corrente
                transition_model, conoscenza di come il mondo evolve
                sensor_model, conoscenza di come i percetti riflettono lo stato
                rules, un insieme di regole condizione-azione
                action, l'ultima azione eseguita (inizialmente nessuna)
                
    state  <- UPDATE-STATE(state, action, percept, transition_model, sensor_model)
    rule   <- RULE-MATCH(state, rules)
    action <- rule.ACTION
    return action
```
