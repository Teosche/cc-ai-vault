Capitolo: [[Percezione, interazione e robotica]]

La percezione trasforma segnali sensoriali in rappresentazioni utilizzabili per decidere. L'azione modifica l'ambiente e quindi i segnali che diventeranno disponibili. Nelle architetture cognitive questi processi sono collegati a memoria, apprendimento e ragionamento: interpretare una scena dipende da ciò che il sistema cerca e da ciò che sa già.

## Selezionare informazione utile

Un robot può ricevere immagini, suoni, informazioni tattili e misure della configurazione del proprio corpo. La visione può individuare un oggetto, mentre una misura di posizione articolare indica dove si trovi il braccio. I segnali contribuiscono a una descrizione utilizzabile della situazione, con livelli diversi di incertezza.

L'attenzione limita la quantità di informazione da elaborare. Un meccanismo selettivo può orientare lo sguardo verso un oggetto; uno restrittivo può circoscrivere la ricerca agli oggetti rossi; uno soppressivo può ridurre il peso di regioni irrilevanti. La richiesta «trova la tazza rossa» rende quindi alcuni segnali più utili di altri.

Questa nozione cognitiva di attenzione riguarda la selezione rispetto a obiettivi e risorse. Il meccanismo di attenzione del Transformer è una specifica operazione sui vettori: può essere un componente del sistema, ma non esaurisce il problema cognitivo.

## Agire per percepire meglio

Se una tazza è coperta da un altro oggetto, spostare il punto di vista può renderla riconoscibile. L'azione serve allora ad acquisire informazione prima ancora che a completare la manipolazione. La nuova immagine modifica la rappresentazione e può cambiare la decisione successiva.

La pianificazione anticipa una sequenza di azioni per raggiungere un obiettivo; la selezione dinamica sceglie invece in funzione della situazione corrente. Possono cooperare: il robot può pianificare un percorso ma correggerlo quando appare un ostacolo. Conservare il piano senza riesaminare i percetti renderebbe inefficace il ciclo.

La prospezione è la capacità di anticipare possibilità future. Una simulazione interna può confrontare gli effetti attesi di due movimenti prima dell'esecuzione. Il risultato della simulazione rimane una previsione: dopo l'azione, l'osservazione permette di confrontare ciò che era atteso con ciò che è avvenuto.

## Corpo e sviluppo delle capacità

L'[[Embodied Cognition]] considera il corpo parte delle condizioni che strutturano la cognizione. Altezza della camera, forma della pinza e libertà delle articolazioni cambiano sia le osservazioni disponibili sia le azioni possibili. Conoscere linguisticamente una tazza non equivale a sapere come afferrarla con qualsiasi corpo.

Le [[Developmental cognitive architectures|architetture dello sviluppo]] studiano l'acquisizione progressiva di abilità durante la vita del sistema. Esplorare individualmente permette di apprendere le conseguenze delle proprie azioni; l'interazione sociale offre dimostrazioni, correzioni e obiettivi condivisi. Le prospettive individuale e sociale descrivono occasioni complementari per costruire competenze.

L'architettura deve dunque coordinare capacità che cambiano insieme. Migliorare il riconoscimento senza collegarlo alla selezione dell'azione può lasciare invariata la capacità di completare un compito reale.
