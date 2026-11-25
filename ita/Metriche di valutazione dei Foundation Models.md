# Metriche di valutazione dei Foundation Models

Capitolo: [[Adattamento, valutazione e applicazioni dei Foundation Models]]

Una valutazione misura quanto un modello risolva un compito definito su dati che non ha usato per adattarsi a quel compito. Prima di scegliere una metrica occorre stabilire quale errore conta: perdere un caso raro, classificare male una recensione, recuperare un documento inutile o inventare un dettaglio in una risposta sono problemi diversi.

## Separare apprendimento e verifica

Il training set aggiorna i parametri; il validation set permette di scegliere configurazioni, soglie e momento di arresto; il test set misura il risultato finale dopo queste scelte. Continuare a scegliere in base al test lo trasforma di fatto in un altro validation set. Anche un prompt ottimizzato ripetutamente sugli stessi esempi può adattarsi alla valutazione.

La cross-validation ripete la procedura su diverse suddivisioni. In una cross-validation a dieci fold si addestra su nove parti e si valuta sulla parte restante, ruotandone il ruolo. Se serve anche selezionare iperparametri, tale selezione deve restare interna ai dati di addestramento di ciascuna prova. La variabilità fra prove aiuta a capire quanto il risultato dipenda dalla suddivisione.

La separazione deve rispettare l'origine dei dati. Slice dello stesso paziente o frammenti quasi identici dello stesso documento non dovrebbero essere distribuiti fra training e test quando si vuole misurare la generalizzazione a pazienti o documenti nuovi. Altrimenti il sistema può riconoscere caratteristiche già incontrate, producendo una stima troppo favorevole.

## Leggere gli errori di classificazione

La matrice di confusione confronta classe reale e prevista. Per una classe positiva, $TP$ sono i positivi riconosciuti, $FN$ quelli persi, $FP$ i negativi dichiarati positivi e $TN$ i negativi riconosciuti. L'accuracy è la quota di previsioni corrette sul totale, ma può nascondere uno scarso riconoscimento della classe rara.

Supponiamo di avere 100 esempi, di cui 10 positivi. Il modello riconosce 8 positivi, ne perde 2 e segnala erroneamente 12 negativi. Ha dunque 8 veri positivi, 2 falsi negativi, 12 falsi positivi e 78 veri negativi: l'accuracy è 86%. Un modello che rispondesse sempre «negativo» otterrebbe 90%, pur perdendo tutti i positivi.

Per distinguere queste situazioni si usano:

$$
P=\frac{TP}{TP+FP},\qquad R=\frac{TP}{TP+FN},\qquad F_1=\frac{2PR}{P+R}.
$$

La precision risponde a «quante segnalazioni positive sono corrette?»; il recall a «quanti positivi reali sono stati trovati?». Nell'esempio valgono 40% e 80%, mentre $F_1$ è circa 53,3%. Abbassare una soglia può aumentare il recall facendo crescere anche i falsi allarmi. La scelta dipende dal costo relativo degli errori, non soltanto dal punteggio più alto.

Nel multiclass si può calcolare una misura per classe e poi aggregarla. La media macro dà uguale peso alle classi; quella weighted pesa per la loro numerosità; la micro somma prima i conteggi. La macro rende più visibile una classe rara gestita male. Occorre specificare anche come vengono trattati denominatori nulli.

## Valutare previsione e generazione linguistica

La cross-entropy misura quanta probabilità il modello assegna ai token effettivamente osservati. La perplexity è l'esponenziale della loss media quando si usano logaritmi naturali: loss più bassa significa perplexity più bassa. Il confronto richiede dati, tokenizzazione e convenzioni compatibili. Prevedere meglio un corpus non equivale però a rispondere correttamente a ogni domanda fattuale.

BLEU confronta sequenze di parole della traduzione generata con traduzioni di riferimento, penalizzando anche uscite troppo brevi. Può cogliere somiglianze utili, ma una traduzione corretta con parole diverse può essere penalizzata. Per riassunti e risposte occorre inoltre controllare informazioni mantenute, aggiunte indebite e rispetto della richiesta.

## Valutare ricerca e risposte documentali

Nel retrieval conta la posizione dei documenti pertinenti. Per una query con due documenti rilevanti, trovati al primo e al terzo posto, le precision nelle posizioni rilevanti sono $1$ e $2/3$. L'average precision è quindi $(1+2/3)/2\simeq0,833$. Il denominatore considera tutti i documenti rilevanti previsti dal giudizio di riferimento; perderne uno peggiora il risultato. La mean average precision media questo valore fra query. La nDCG permette inoltre di rappresentare gradi diversi di rilevanza e riduce il peso dei risultati lontani dalla cima.

Per un [[RAG]] vanno valutati separatamente recupero e generazione. La fedeltà controlla che le affermazioni siano sostenute dal contesto; la rilevanza della risposta che risolvano la domanda. La precision delle citazioni verifica se i riferimenti sostengono le frasi associate, mentre la loro copertura verifica quante affermazioni dispongano di supporto. Un testo può essere fluido e pertinente ma contenere un fatto inventato. Un LLM usato come giudice può accelerare questi controlli, purché i criteri siano espliciti e il suo accordo con valutazioni umane venga verificato.
