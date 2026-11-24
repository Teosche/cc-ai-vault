# Metriche di valutazione dei Foundation Models

Capitolo: [[Adattamento, valutazione e applicazioni dei Foundation Models]]

Valutare un Foundation Model richiede di definire il compito e il risultato che si considera utile. Una metrica di classificazione, una misura di previsione dei token e una valutazione della fedeltà di una risposta descrivono aspetti differenti. Nessun singolo valore riassume automaticamente tutte le capacità del sistema.

## Dati separati e generalizzazione

Il training set serve ad aggiornare i parametri; il validation set sostiene le scelte di modello e configurazione; il test set deve restare separato per la valutazione finale. Se il test viene utilizzato ripetutamente per scegliere le configurazioni, perde il ruolo di verifica indipendente.

Anche la distribuzione degli esempi conta. Un modello che assegna sempre la classe maggioritaria può ottenere un'accuracy elevata su dati sbilanciati senza riconoscere la classe rara. Occorre esaminare la natura degli errori e non soltanto il loro numero complessivo.

## Classificazione

La matrice di confusione distingue veri positivi, falsi positivi, veri negativi e falsi negativi. Per denominatori non nulli:

$$
\operatorname{Precision}=\frac{TP}{TP+FP},\qquad
\operatorname{Recall}=\frac{TP}{TP+FN},\qquad
F_1=\frac{2PR}{P+R}.
$$

$P$ e $R$ indicano rispettivamente precision e recall. La precision misura quanti positivi previsti sono corretti; il recall quanti positivi reali vengono recuperati. La media armonica penalizza uno squilibrio fra i due. Nel multiclass, la media macro assegna lo stesso peso alle classi; quella weighted pesa per il supporto; la micro aggrega i conteggi prima del calcolo. Weighted e macro non sono sinonimi.

## Previsione linguistica e generazione

La perplexity può essere calcolata come esponenziale della loss media di previsione dei token, se la loss usa logaritmi naturali. Un valore inferiore indica una migliore assegnazione di probabilità ai token del testo di valutazione, nelle condizioni adottate. Il confronto richiede attenzione a tokenizer, dati e convenzioni di calcolo.

Questo non dimostra che ogni risposta sia vera o utile. Ripetizioni, aderenza alla richiesta e affermazioni inventate richiedono controlli aggiuntivi. BLEU permette di valutare traduzioni mediante un confronto con riferimenti. Il suo risultato va letto rispetto al compito, senza estenderlo a un giudizio generale sull'intelligenza del modello.

## Retrieval e risposte fondate sui documenti

Nel retrieval, conta anche l'ordine dei risultati: un documento pertinente in prima posizione è più utile dello stesso documento nascosto in fondo alla lista. Average precision e mean average precision aggregano questa qualità rispetto a query con giudizi di rilevanza.

Per un sistema [[RAG]] occorre separare il recupero dalla generazione. Una risposta può essere fluida ma poco supportata; una citazione può essere presente senza sostenere l'affermazione associata. Fedeltà al contesto, rilevanza della risposta, supporto delle citazioni e copertura delle affermazioni descrivono dimensioni diverse.

Un LLM usato come giudice può aiutare a organizzare la valutazione, ma resta un valutatore fallibile. Criteri espliciti e controlli umani restano necessari per interpretare i punteggi.
