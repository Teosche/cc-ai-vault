# Tecnologie semantiche

Capitolo: [[Tecnologie semantiche, Knowledge Graphs e ontologie]]

Le tecnologie semantiche organizzano i dati rendendo esplicito il significato delle entità e delle relazioni. Memorizzare un valore non basta a sapere che cosa rappresenti, come sia collegato ad altri valori e quali confronti siano leciti. La semantica fornisce un principio di interpretazione che rende i dati utilizzabili in un contesto condiviso.

## Dal dato al contesto

Un archivio può collegare identificativi di clienti e prodotti. Per rispondere alla domanda «chi ha acquistato questo prodotto?» occorre sapere che un identificativo rappresenta un cliente, un altro un prodotto e il collegamento un acquisto. Se tale conoscenza esiste soltanto nel programma che legge l'archivio, cambiare applicazione può richiedere di ricostruire la stessa interpretazione.

I [[Knowledge Graphs]] rendono espliciti elementi e legami del dominio. Il vantaggio concettuale non è semplicemente usare frecce al posto di tabelle, ma associare alla struttura un significato comprensibile sia alle persone sia ai sistemi che la utilizzano. La struttura deve essere accompagnata da un accordo su nomi, categorie e relazioni.

## Organizzare categorie e relazioni

Un'etichetta identifica una categoria, ma non specifica necessariamente le relazioni con le altre. Sapere che un prodotto è una mela e un altro una banana non basta, da solo, a rendere disponibile al sistema la categoria più generale «frutta». Una tassonomia esplicita la gerarchia di generalizzazione e specializzazione.

Le [[Ontologie]] estendono questa organizzazione con relazioni come parte-tutto, compatibilità e dipendenza, oltre a proprietà delle relazioni stesse. La semantica stabilisce così che cosa si intenda con un collegamento e quali elaborazioni siano appropriate. Non basta che due elementi abbiano nomi simili per considerarli equivalenti.

## Integrare descrizioni diverse

L'interoperabilità richiede che sistemi diversi possano interpretare coerentemente informazioni scambiate o collegate. Due archivi possono usare termini differenti per concetti in parte comuni, oppure attribuire allo stesso termine ruoli diversi. L'integrazione deve perciò tenere conto delle categorie del dominio, non soltanto dei nomi dei campi.

Un esempio è il collegamento fra un dominio sanitario e uno di attività fisica. Pazienti, medici, utenti e allenatori possono essere ricondotti a una categoria generale di persona senza perdere i loro ruoli specifici. Appuntamenti e sessioni di allenamento possono condividere la descrizione generale di una sessione, mantenendo attributi e partecipanti propri. Il vocabolario comune consente l'integrazione senza dichiarare identiche tutte le categorie.

## Rendere utilizzabile la struttura

Una rappresentazione semantica sostiene interrogazioni su collegamenti e categorie e può fornire contesto a sistemi generativi. Le operazioni richieste da una domanda possono includere attraversamenti, filtri, conteggi e aggregazioni. Il valore aggiunto rispetto al solo confronto fra testi è la disponibilità di relazioni esplicite fra entità.

Una struttura esplicita non garantisce però che le informazioni siano vere o complete. Se entità e collegamenti sono ricavati automaticamente da testi, la qualità dell'estrazione e l'interpretazione delle relazioni restano determinanti. La tracciabilità aiuta a controllare il risultato, ma non sostituisce la verifica della conoscenza rappresentata.
