# Interazione uomo-robot

Capitolo: [[Percezione, interazione e robotica]]

L'interazione uomo-robot collega comunicazione e azione in un ambiente condiviso. Una richiesta deve diventare un obiettivo interpretabile dal robot e un comportamento comprensibile alla persona. La riuscita dipende sia dal significato delle parole sia da riferimenti fisici, capacità del corpo e feedback reciproco.

## Stabilire a che cosa ci si riferisce

La richiesta «prendi quello» può essere chiara a una persona che segue il gesto dell'interlocutore, ma il robot deve collegare parola, gesto e oggetto. Questo collegamento è una forma di grounding: associa una rappresentazione linguistica a un referente nella situazione percepita.

Se sul tavolo ci sono due tazze rosse, la sola categoria non basta. Posizione, sguardo o una domanda di chiarimento possono risolvere l'ambiguità. Se invece il robot non vede una delle tazze, può avere una rappresentazione incompleta e interpretare erroneamente la richiesta come univoca. La qualità del dialogo dipende quindi anche dalla percezione.

## Trasformare l'intenzione in un'azione realizzabile

Dopo aver identificato l'oggetto, il sistema deve stimarne una posizione utile al movimento. Occorre collegare il riferimento della camera a quello del corpo e verificare che l'oggetto sia raggiungibile. Un pianificatore sceglie poi una sequenza compatibile con la configurazione del robot; il controllo traduce tale sequenza in comandi agli attuatori.

Questa catena chiarisce il divario fra obiettivo, piano ed esecuzione. Il linguaggio può descrivere un'intenzione corretta, mentre il piano può scegliere un percorso inadatto o il movimento può non ottenere la presa prevista. Le osservazioni successive permettono di distinguere questi errori e correggerli.

## Coordinare comunicazione e feedback

Il robot può segnalare quale oggetto ha identificato prima di muoversi, permettendo alla persona di correggerlo. Uno spostamento della testa può rendere visibile la direzione dell'attenzione; una risposta verbale può comunicare un'incertezza. La comunicazione aiuta a rendere condiviso lo stato del compito, non soltanto a produrre frasi naturali.

La prospettiva sociale dello sviluppo considera dimostrazioni e correzioni come risorse per acquisire abilità. Un'esperienza può essere conservata come episodio, mentre una procedura appresa descrive come agire in situazioni simili. Occorre mantenere distinto il ricordo di un singolo successo da una competenza generalizzabile.

## Verificare l'esito insieme alla persona

Il feedback fisico e quello umano possono segnalare problemi diversi. Una posizione articolare raggiunta documenta il movimento del braccio; non dimostra necessariamente che sia stato afferrato l'oggetto richiesto. La persona può inoltre correggere un risultato fisicamente riuscito ma semanticamente sbagliato.

La valutazione considera dunque comprensione della richiesta, identificazione del referente, riuscita dell'azione e possibilità di interrompere o correggere il comportamento. Gli [[Agenti autonomi interattivi]] forniscono l'organizzazione del ciclo; le [[Sfide dell’interazione multimodale]] spiegano le difficoltà nel collegare i canali informativi che lo alimentano.
