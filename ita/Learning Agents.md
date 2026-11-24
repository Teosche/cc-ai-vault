---
tags:
  - agenti-apprendenti
  - sistemi-intelligenti
  - valutazioni
  - apprendere
---

**Alan Turing** nel 1950 osservò che programmare macchine intelligenti a mano richiede uno sforzo impraticabile. La soluzione ottimale è *costruire macchine capaci di apprendere e poi istruirle*. Qualsiasi agente può essere strutturato come agente che apprende.

Elementi fondamentali dei **Learning Agents** sono:

- **Performance Element**
  E' l'agente interno vero e proprio responsabile di selezionare le azioni esterne in base ai percetti.
- **Learning Element**
  Responsabile di apportare i miglioramenti al sistema. Riceve il feedback dal critico e stabilisce come modificare l'elemento di prestazione per operare meglio in futuro.
- **Critic**
  Valuta la condotta dell'agente confrontando i percetti riscontrati con un *fixed perfmormance standard*.
  Lo standard di prestazione deve essere concettualmente esterno all'agente, in modo che esso non possa modificarlo a proprio piacimento per autogiustificare il proprio comportamento. Il critico restituisce segnali di *reward* o *penality*.
- **Problem Generator**
  Responsabile di suggerire azioni esplorative. Se l'elemento di prestazione agisse da solo, continuerebbe a ripetere le azioni che ritiene migliori in base alle conoscenze attuali. Il generatore di problemi spinge l'agente a tentare azioni sub-ottimali nel breve termine per scoprire conoscenze più informative nel lungo termine.

L'apprendimento di un'**agente intelligente** si riassume quindi come il processo di modifica progressiva di ciascun componente dell'agente (modelli di transizione, sensori, regole, condizione-azione o funzioni di utilità) per portarlo in maggiore sintonia con i feedback dell'ambiente, migliorandone le prestazioni complessive nel tempo.
