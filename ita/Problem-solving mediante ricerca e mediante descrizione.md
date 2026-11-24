# Problem-solving mediante ricerca e mediante descrizione

Capitolo: [[Problem-solving, ricerca e similarità]]

La risoluzione di un problema richiede sia una **rappresentazione** delle informazioni rilevanti sia un **procedimento** che consenta di ottenere una risposta. La rappresentazione specifica quali situazioni, proprietà e relazioni il sistema può distinguere; il procedimento utilizza questa conoscenza per costruire o verificare possibili soluzioni.

## Costruire una sequenza di azioni

Nel problem-solving mediante ricerca, la domanda tipica è come arrivare da una situazione iniziale a un obiettivo. Un taxi deve individuare una sequenza di svolte e spostamenti; conoscere soltanto la posizione corrente non basta, perché occorre considerare gli effetti futuri delle azioni.

In una rappresentazione atomica, ciascuna città può essere trattata come uno stato distinto. Il procedimento esplora le connessioni fra questi stati. La semplicità della descrizione permette di concentrarsi sulla struttura dei percorsi, ma può diventare insufficiente quando la soluzione dipende da altre caratteristiche, come il carburante disponibile.

## Esplicitare la conoscenza che definisce il problema

Una descrizione più articolata rende esplicite *proprietà*, *oggetti* e *relazioni*. In una rappresentazione fattorizzata lo stato è descritto mediante variabili: per il taxi possono contare la posizione e il carburante disponibile. In una rappresentazione strutturata si distinguono anche gli oggetti e le loro relazioni, per esempio il taxi, il passeggero e il fatto che il passeggero si trovi nel veicolo. La descrizione rende così utilizzabili condizioni che il solo nome di una città non esprime.

Un insieme di assiomi può sostenere il ragionamento necessario a costruire un piano, mentre l'aggiunta di nuova conoscenza può estendere la competenza del sistema. La descrizione non consiste quindi soltanto nel raccontare il problema in linguaggio naturale: deve rendere disponibili informazioni utilizzabili dal procedimento di ragionamento.

## Due aspetti che possono cooperare

Descrivere un problema e cercarne la soluzione possono essere parti dello stesso sistema. Una descrizione più informativa può guidare la ricerca; una ricerca può essere il meccanismo utilizzato per trovare le conseguenze di conoscenze formalizzate. Il confronto non va quindi ridotto a «ricerca senza conoscenza» contro «descrizione senza elaborazione».

Nel caso DENDRAL, conoscenze specifiche sulle strutture molecolari restringono le alternative che il sistema deve considerare. La rappresentazione del dominio cambia l'efficacia della ricerca, senza trasformare la soluzione in una semplice lettura passiva dei dati. Più in generale, la qualità della descrizione e l'organizzazione dell'esplorazione vanno valutate insieme.
