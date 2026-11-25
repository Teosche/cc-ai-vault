# Robot antropomorfo Reachy

Capitolo: [[Percezione, interazione e robotica]]

Reachy è una piattaforma robotica aperta di Pollen Robotics pensata per sperimentare manipolazione e interazione. La forma antropomorfa riguarda soprattutto testa, torso e braccia: offre gesti e movimenti riconoscibili, mentre il software determina quali capacità percettive e decisionali siano effettivamente disponibili. Le caratteristiche variano fra generazioni e configurazioni; l'esempio seguente considera Reachy 2023.

## Collegare il corpo alle informazioni disponibili

Nella configurazione completa, testa e braccia permettono di orientare la percezione e interagire con oggetti. Il braccio dispone di sette gradi di libertà, oltre all'apertura della pinza. Un grado di libertà è una possibilità indipendente di movimento: più articolazioni consentono di raggiungere una stessa zona con configurazioni diverse.

La testa include camere e antenne motorizzate; il torso ospita anche componenti per elaborazione e comunicazione audio. Le camere forniscono osservazioni visive, mentre la lettura delle posizioni articolari descrive la configurazione del corpo. Sensori e attuatori coprono quindi ruoli complementari nel ciclo percezione-azione.

## Dalla configurazione del braccio a una posizione nello spazio

La cinematica diretta calcola posizione e orientamento dell'estremità del braccio a partire dai valori delle articolazioni. La cinematica inversa affronta il problema opposto: data una posa desiderata della pinza, cerca una configurazione articolare che la realizzi. La posa include sia dove si trovi la pinza sia come sia orientata.

Per indicare un oggetto, il livello decisionale può scegliere un obiettivo nello spazio; il sistema deve poi convertirlo in una configurazione raggiungibile. Possono esistere più soluzioni oppure nessuna per la posa richiesta. Il punto iniziale e i vincoli del corpo influenzano la soluzione. Il modello cinematico descrive relazioni geometriche e non sostituisce da solo il controllo delle forze e dell'esecuzione.

## Organizzare il software di interazione

Il Python SDK permette di leggere sensori e stati articolari e inviare comandi agli attuatori, appoggiandosi all'infrastruttura del robot. I componenti ROS 2 offrono accesso alle funzioni robotiche a un livello diverso. L'SDK semplifica la costruzione di comportamenti, mentre i problemi di temporizzazione e controllo più stretti richiedono componenti adeguati.

Un'integrazione con un modello linguistico può interpretare «guarda l'oggetto rosso» e selezionare un comportamento. La percezione deve individuare l'oggetto; il controllo deve orientare la testa; una nuova osservazione deve verificare il risultato. Questa è una possibile architettura applicativa, non una capacità automaticamente inclusa dalla presenza del robot.

## Teleoperazione e autonomia

Nella teleoperazione una persona guida il corpo a distanza e riceve feedback dal robot. In un comportamento autonomo, le scelte vengono invece prodotte dalla policy o dal programma di controllo in funzione delle osservazioni. Eseguire un gesto guidato da una persona e decidere quando eseguirlo sono capacità diverse.

Reachy rende quindi concreto il collegamento fra corpo, percezione e decisione. Le prestazioni di un'applicazione vanno misurate sull'intera catena, distinguendo riconoscimento del referente, movimento richiesto ed esito effettivo nell'[[Interazione uomo-robot|interazione con la persona]].
