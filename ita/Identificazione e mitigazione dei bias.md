# Identificazione e mitigazione dei bias

Capitolo: [[Explainable AI, etica e bias]]

Identificare un bias richiede stabilire quale comportamento sia problematico e rispetto a quale compito o popolazione. Il termine non indica sempre la stessa cosa: il bias numerico di uno strato, una preferenza architetturale e una distorsione sociale sono concetti differenti. Qui interessa soprattutto l'uso sistematico di informazioni inappropriate o la distorsione delle risposte.

## Osservare le dipendenze del modello

Un classificatore può utilizzare lo sfondo invece dell'oggetto. La previsione corretta su alcuni esempi non basta a escludere questa dipendenza: cambiando il contesto, la regolarità potrebbe non essere più utile. Metodi di attribuzione e visualizzazione possono aiutare a individuare caratteristiche sospette.

Il confronto deve restare legato al metodo: una heatmap suggerisce dove cercare, ma non fornisce da sola una dimostrazione causale o una verifica completa della fairness.

## Bias cognitivi e formulazione linguistica

I modelli linguistici possono essere influenzati dalla forma di una domanda o dalla plausibilità della conclusione. Il belief bias riguarda il rischio di giudicare un argomento in funzione di ciò che sembra credibile, anziché del rapporto logico fra premesse e conclusione.

Il caso Table QA illustra associazioni linguistiche che possono spingere verso risposte affermative anche quando i dati richiedono il contrario. Generare varianti della domanda e aggregare evidenze mira a ridurre la dipendenza dalla formulazione superficiale. La procedura deve comunque essere verificata sul compito, senza considerare il voto di maggioranza una garanzia di verità.

## Interventi e nuova valutazione

Una strategia di debiasing può analizzare il tipo di distorsione e riformulare il contesto o la richiesta. Nel metodo SACD descritto, il processo comprende determinazione del bias, analisi e revisione del prompt. Si tratta di un intervento sul comportamento in inferenza, non necessariamente di una modifica dei dati o dei pesi.

Gli interventi vanno valutati su esempi e condizioni pertinenti. Migliorare una particolare prova non dimostra che tutti i bias siano rimossi. È necessario mantenere separate l'ipotesi sul problema, l'azione correttiva e la misura del risultato.

## Distorsioni sociali e supervisione

L'analisi deve includere gli effetti dell'uso: una rappresentazione distorta può influenzare scelte che assegnano opportunità o risorse. Una spiegazione del modello è utile per rendere il problema osservabile, ma non sostituisce la valutazione delle conseguenze.

L'analisi tecnica individua le distorsioni e ne valuta le conseguenze. I criteri di [[Etica, fairness e accountability nell’IA|fairness e accountability]] riguardano anche l'accettabilità di quelle conseguenze e la responsabilità delle decisioni: rilevare un bias non determina da solo come il sistema debba essere utilizzato.
