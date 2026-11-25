# Continual learning

Capitolo: [[Paradigmi di apprendimento]]

Il continual learning studia come un modello possa apprendere da dati che arrivano progressivamente, conservando ciò che ha già acquisito. Un classificatore può dover riconoscere nuove classi oppure affrontare le stesse classi con condizioni di acquisizione diverse. Spesso i dati precedenti non sono più disponibili per un nuovo addestramento completo.

## Perché il nuovo apprendimento può cancellare il precedente

I pesi di una rete vengono riutilizzati per molti esempi. Quando il modello viene ottimizzato soltanto sui dati nuovi, gli aggiornamenti possono spostare rappresentazioni utili ai casi vecchi. Si parla di catastrophic forgetting quando le prestazioni precedenti peggiorano fortemente pur migliorando sul nuovo compito.

Immaginiamo un riconoscitore addestrato prima su cifre e poi soltanto su lettere. Una buona accuratezza finale sulle lettere non dimostra che il sistema sappia ancora riconoscere le cifre. Il problema è mantenere un equilibrio fra plasticità, necessaria per acquisire novità, e stabilità, necessaria per conservare competenze.

## Conservare informazione senza bloccare ogni cambiamento

Elastic Weight Consolidation protegge maggiormente i parametri stimati come importanti per i compiti precedenti. Durante l'apprendimento del nuovo compito aggiunge una penalità agli scostamenti dai vecchi valori, con intensità diversa per parametro. L'immagine intuitiva è quella di molle più rigide sui pesi importanti: una penalizzazione uniforme rischierebbe di impedire anche cambiamenti utili.

Nella formulazione EWC, l'importanza viene approssimata attraverso la *diagonale dell'informazione di Fisher*, che misura quanto la distribuzione predetta sia sensibile a piccoli cambiamenti del parametro sui dati precedenti. È una stima locale: non rappresenta tutte le interazioni fra pesi.

Il replay conserva invece una selezione di esempi precedenti e li ripresenta insieme ai dati nuovi. Il modello riceve così un segnale concreto su ciò che deve mantenere. iCaRL combina esempi rappresentativi e conservazione delle precedenti risposte del modello per sostenere l'apprendimento incrementale di classi. La memoria occupata e il modo di scegliere gli esempi influenzano il risultato.

## Valutare l'intera storia

Dopo ogni fase occorre misurare le prestazioni sia sui contenuti appena appresi sia su quelli precedenti. Una matrice che riporti ogni compito dopo ciascuna fase permette di distinguere acquisizione, dimenticanza ed eventuale trasferimento positivo. La valutazione deve dichiarare anche se al test il modello conosca l'identità del compito: scegliere fra classi di un solo compito è più semplice che scegliere fra tutte quelle incontrate.

Aggiungere un documento alla memoria esterna di un agente può conservarne l'accessibilità senza aggiornare i pesi. Questo è utile, ma affronta un meccanismo diverso dalla conservazione delle capacità neurali durante addestramenti successivi.
