# Agenti autonomi interattivi

Capitolo: [[Percezione, interazione e robotica]]

Un agente autonomo interattivo sceglie azioni in funzione di obiettivi, informazioni disponibili e risultati precedenti. L'ambiente può essere fisico, digitale o conversazionale. L'autonomia consiste nel decidere come procedere entro capacità e vincoli assegnati; l'interazione fornisce nuove informazioni e rende possibile correggere il comportamento.

## Dall'obiettivo a un ciclo operativo

Consideriamo un agente che debba confrontare due prodotti. Deve individuare quali proprietà servono, cercarle, leggere i risultati e decidere se bastino. Se una ricerca fallisce, può cambiare query; se i dati sono incompatibili, può approfondire prima di formulare il confronto.

Il ciclo Think-Act-Observe organizza questa attività. Il sistema elabora il passo successivo, esegue un'azione e integra l'osservazione nel contesto. ReAct usa un'alternanza esplicita di elaborazione e azione per evitare che il piano resti scollegato dai risultati. Lo strumento può restituire un errore o un dato inatteso: entrambi devono influenzare la scelta successiva.

## Azioni che elaborano e azioni che modificano l'ambiente

In [[CoALA]], il retrieval porta informazioni persistenti nella memoria di lavoro; il ragionamento elabora il contenuto attivo; l'apprendimento modifica ciò che rimane disponibile a lungo termine. Le azioni di grounding collegano invece il sistema all'esterno, per esempio tramite una domanda a una persona, una query a un servizio o il comando di un dispositivo.

La distinzione permette di seguire gli effetti. Recuperare una procedura rende disponibile un testo; eseguire quella procedura su un servizio ne cambia lo stato. Scrivere in memoria una conclusione la rende influente anche in futuro. Queste operazioni richiedono verifiche differenti.

## Proporre, valutare e scegliere

Una procedura decisionale può generare azioni candidate, confrontarle rispetto all'obiettivo e selezionarne una. La pianificazione può includere azioni interne: prima di agire, l'agente può cercare un'informazione mancante o controllare un'ipotesi. La memoria di lavoro collega ogni fase al contesto corrente.

Per un robot, un comando come «raggiungi la tazza» deve essere tradotto in una stima della posizione e in operazioni eseguibili dal controllo. Il livello linguistico può proporre l'obiettivo; il feedback deve accertare che il movimento e l'eventuale presa siano riusciti. Dichiarare il completamento perché il comando è stato inviato interromperebbe il ciclo troppo presto.

## Coordinare più agenti

Più agenti possono distribuire raccolta di dati, analisi e verifica. Un coordinatore centrale può assegnare compiti; in un'organizzazione distribuita, il coordinamento emerge da scambi fra partecipanti. La specializzazione è utile quando le informazioni arrivano ai componenti che ne hanno bisogno.

La condivisione di un obiettivo non elimina errori di coordinamento: due agenti possono lavorare sulla stessa parte e ometterne un'altra. Con obiettivi diversi può emergere conflitto; in contesti competitivi una cooperazione indesiderata può creare collusione. Per valutare il sistema occorre quindi osservare sia i singoli risultati sia gli effetti delle interazioni, ricostruendo il percorso fra richiesta, piano ed esecuzione.
