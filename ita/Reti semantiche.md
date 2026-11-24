# Reti semantiche

Capitolo: [[Problem-solving, ricerca e similarità]]

**Una rete semantica rappresenta conoscenza attraverso elementi collegati da relazioni significative**. L'interesse non è soltanto stabilire che due elementi siano connessi, ma chiarire che *cosa significhi la connessione*: appartenenza a una categoria, localizzazione, compatibilità o un altro legame del dominio. Questo colloca le reti semantiche nel problema più generale della [[The problem of knowledge representation|rappresentazione della conoscenza]].

## Dalla connessione al significato

Un grafo può collegare clienti e prodotti, ma il suo utilizzo richiede di sapere che cosa rappresentino i nodi e gli archi. Se il legame significa «ha acquistato», attraversare il grafo permette di individuare gli acquisti di un cliente o gli acquirenti di un prodotto. Se questo significato è noto soltanto al programma, la struttura dei dati rimane dipendente dall'interpretazione codificata nel software.

Rendere espliciti tipi di entità e relazioni aiuta a organizzare la conoscenza in modo condivisibile. Un'etichetta come `Person` distingue una persona da un luogo; una relazione come `LIVES_IN` specifica il collegamento fra le due entità. Le proprietà permettono di aggiungere informazioni sul nodo o sulla relazione. Nei [[Knowledge Graphs]], nodi, tipi e relazioni rendono esplicita questa organizzazione del dominio.

## Relazioni di categoria e conoscenza del dominio

Le sole etichette non bastano a stabilire che una categoria sia più generale di un'altra. Per esprimere che una giraffa è un mammifero serve una relazione di specializzazione fra categorie. Una tassonomia organizza questo tipo di collegamenti in una gerarchia dal generale al particolare.

Le [[Ontologie]] permettono inoltre di distinguere relazioni come «parte di», «compatibile con» e «dipende da». Il significato di ciascuna relazione determina quali ragionamenti siano giustificati. Una relazione di appartenenza a una categoria non può essere trattata automaticamente come una relazione parte-tutto, e un collegamento qualunque non costituisce una prova di causalità.

La struttura relazionale può quindi rendere leggibile una parte del ragionamento, ma richiede un'**interpretazione coerente**. Il disegno di una freccia non specifica da solo tutte le proprietà logiche del legame.

## Relazioni esplicite e somiglianza

Una rete di relazioni esplicite va distinta da uno spazio di vettori nel quale elementi vicini sono considerati simili. Nel primo caso il collegamento può affermare che una persona vive in una città; nel secondo, una misura può indicare che due documenti hanno contenuti affini. *La vicinanza non equivale alla stessa relazione dichiarata*.

Le [[Metriche di similarità]] possono aiutare a recuperare elementi pertinenti, mentre una rete rende utilizzabili collegamenti specifici fra entità. I due strumenti possono cooperare, ma rispondono a domande differenti: «quanto sono simili?» e «quale relazione li lega?» non sono la stessa richiesta.
