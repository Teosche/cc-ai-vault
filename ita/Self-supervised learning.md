# Self-supervised learning

Capitolo: [[Paradigmi di apprendimento]]

Il self-supervised learning costruisce il segnale di addestramento a partire dai dati stessi. Non richiede che un annotatore fornisca un'etichetta manuale per ogni esempio del compito costruito; rimangono però una funzione obiettivo, target di previsione e scelte sul modo di trasformare gli input.

## Costruire un compito dai dati

Nel linguaggio, i token di un testo possono fornire il target per prevedere una parte mancante o il token successivo. Il modello riceve una porzione di informazione e viene ottimizzato rispetto a quella che deve ricostruire. La supervisione viene quindi derivata dalla struttura del dato, anziché aggiunta come categoria esterna.

Il termine non significa «apprendimento senza obiettivo». La scelta di che cosa nascondere o prevedere determina quali regolarità possano essere utili. Mascherare posizioni e prevedere continuazioni impongono vincoli differenti, come spiegato in [[Obiettivi di pre-training linguistico]].

## Apprendimento contrastivo

Un'altra impostazione confronta rappresentazioni di coppie positive e negative. Trasformazioni della stessa immagine possono fornire viste positive; altre immagini possono funzionare da negativi. Il compito favorisce rappresentazioni che conservino informazioni utili nonostante le trasformazioni considerate.

In CLIP, la coppia immagine-descrizione costituisce il positivo e le altre combinazioni del batch forniscono negativi impliciti. Le associazioni naturali fra modalità forniscono un segnale ricco, ma non privo di scelte o di possibili errori nei dati.

Non ogni tecnica contrastiva è self-supervised: se le coppie positive vengono costruite usando categorie annotate, la supervisione è diversa. Occorre specificare da dove provenga il criterio di associazione.

## Preaddestramento e riuso

Le rappresentazioni apprese possono essere congelate oppure ulteriormente adattate a compiti supervisionati. Il vantaggio consiste nel riutilizzare struttura acquisita da grandi quantità di dati, riducendo la necessità di apprendere tutto da un dataset specifico.

Resta necessario verificare il trasferimento al nuovo dominio. Il fatto che il target sia ricavato dai dati non impedisce al modello di apprendere correlazioni indesiderate o informazioni poco pertinenti per l'applicazione. Self-supervision e correttezza delle conoscenze non sono la stessa proprietà.
