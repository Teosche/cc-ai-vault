# Integrazione simbolica e subsimbolica

Capitolo: [[Comprensione del linguaggio, chatbot e integrazione della conoscenza]]

L'integrazione simbolica e subsimbolica combina rappresentazioni esplicite di fatti, regole e relazioni con rappresentazioni distribuite apprese da dati. I due approcci possono svolgere funzioni complementari: il primo rende disponibili strutture controllabili, il secondo tratta regolarità percettive e linguistiche difficili da codificare interamente a mano.

## Due forme di rappresentazione

Una relazione in un grafo può dichiarare che una persona vive in una città. Un embedding rappresenta invece l'entità o un testo mediante coordinate apprese. La somiglianza fra vettori non è la stessa cosa di una proposizione dichiarata, e non ne possiede automaticamente le condizioni di verità.

La rappresentazione simbolica non risolve da sola l'acquisizione della conoscenza; quella neurale non garantisce da sola trasparenza delle inferenze. Il problema è organizzare uno scambio di informazioni coerente con i ruoli dei componenti.

## Architetture con memoria e procedure

[[CoALA]] distingue conoscenza nei parametri e conoscenza esplicita nel codice o nelle memorie dell'agente. Recupero, ragionamento, apprendimento e azioni esterne possono essere coordinati in una procedura decisionale. Il modello linguistico diventa così un componente di un sistema, non l'intera architettura.

L'integrazione con [[Knowledge Graphs]] aggiunge relazioni navigabili e operazioni strutturate. Un generatore può formulare una risposta utilizzando evidenze recuperate, ma deve rispettare il significato delle relazioni e la portata dei risultati.

## Ragionamento e controllo

Nell'esempio LogiCAM, il processo distingue estrazione delle premesse, identificazione del tipo di ragionamento, elaborazione e controllo della risposta. La sequenza rende espliciti ruoli diversi, anziché affidare ogni passaggio a una sola continuazione testuale.

I limiti di allineamento fra modalità e il ricorso a scorciatoie euristiche mostrano però che aggiungere un componente logico non rende automaticamente corretto l'intero sistema. Premesse estratte male o dettagli visivi trascurati possono compromettere la conclusione anche se una fase successiva applica regole appropriate.

## Che cosa valutare

La verifica deve riguardare le interfacce: come i dati diventano premesse, come una query viene interpretata e come i risultati vengono riportati nel linguaggio. La correttezza di un componente non trasferisce automaticamente una garanzia agli altri.

Una soluzione ibrida deve controllare sia la [[Ragionamento deduttivo|validità delle inferenze]] sia il supporto documentale delle affermazioni generate. Una conclusione può infatti derivare correttamente da premesse false, mentre una frase plausibile può introdurre informazioni non sostenute dai dati.
