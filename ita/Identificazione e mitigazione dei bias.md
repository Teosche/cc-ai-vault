# Identificazione e mitigazione dei bias

Capitolo: [[Explainable AI, etica e bias]]

Un bias è una distorsione sistematica che orienta rappresentazioni o decisioni. Può derivare dai dati, dall'obiettivo di training o dalle condizioni d'uso. Per affrontarlo bisogna trasformare un sospetto generico in un'ipotesi verificabile: quale caratteristica sembra influenzare il modello, in quali casi e con quali conseguenze?

## Distinguere dipendenze diverse

Una scorciatoia statistica permette di ottenere buone prestazioni usando un indizio che non corrisponde alla proprietà cercata. Un classificatore di api può associare la categoria ai fiori, perché spesso compaiono insieme nel training. Finché anche il test contiene la stessa associazione, il difetto può restare nascosto; quando cambia lo sfondo, le prestazioni possono peggiorare.

Un bias sociale può riguardare rappresentazioni di gruppi o differenze negli errori. Un bias cognitivo riguarda invece un andamento sistematico del ragionamento. Nel belief bias, una conclusione credibile viene accettata più facilmente anche quando non segue dalle premesse. La formulazione linguistica può quindi orientare la risposta indipendentemente dai dati che dovrebbero determinarla.

## Costruire un controllo mirato

Per verificare una dipendenza dallo sfondo, si confrontano casi con oggetto presente o assente e sfondi diversi, mantenendo il più possibile comparabili gli altri elementi. Se il punteggio segue i fiori anche senza api, l'ipotesi acquista supporto. I [[Metodi di interpretabilità dei modelli|metodi di attribuzione]] aiutano a scegliere quali regioni controllare, ma una heatmap da sola non quantifica la regolarità sull'intera distribuzione.

Per differenze fra gruppi si confrontano errori e condizioni di raccolta su campioni adeguati. Una diversa frequenza nei report generati può suggerire un problema di rappresentazione; per stabilire differenze nelle prestazioni servono invece etichette di riferimento e misure sugli errori. Sono evidenze diverse.

## Ridurre la dipendenza dalla formulazione

Nel Table QA, una domanda contenente espressioni come «esattamente» può attivare associazioni con risposte affermative e spostare il comportamento verso «sì», anche quando la tabella richiede «no». Una strategia genera riformulazioni equivalenti, raccoglie evidenze e aggrega le risposte. Lo scopo è far dipendere l'esito dai dati anziché da una singola formulazione.

L'equivalenza delle varianti è essenziale: cambiare «almeno» in «esattamente» modifica la domanda. Anche una maggioranza può ripetere lo stesso errore se tutte le varianti mantengono la stessa scorciatoia. La valutazione deve quindi controllare sia il significato delle domande sia la correttezza sulle tabelle.

SACD organizza il debiasing in determinazione del bias, analisi e revisione cognitiva del prompt. Prima individua una possibile distorsione, poi ne esplicita l'effetto e infine riformula il contesto per orientare una decisione più aderente al compito. Agisce durante l'inferenza, senza richiedere necessariamente nuovi pesi.

## Verificare la correzione

Se il problema nasce da dati sbilanciati, intervenire soltanto sul prompt può essere insufficiente; se nasce da una domanda ambigua, aggiungere dati di training può non risolverlo. La correzione deve corrispondere al meccanismo ipotizzato e venire confrontata su casi nuovi, includendo gruppi e condizioni diversi. Una riduzione dell'errore in una prova documenta un miglioramento circoscritto, non l'eliminazione di ogni distorsione.
