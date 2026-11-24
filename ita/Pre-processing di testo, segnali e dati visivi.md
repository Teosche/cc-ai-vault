# Pre-processing di testo, segnali e dati visivi

Capitolo: [[Adattamento, valutazione e applicazioni dei Foundation Models]]

Il pre-processing trasforma i dati nella rappresentazione richiesta dal modello. Non è un passaggio puramente tecnico: decide quali unità vengono distinte, come sono codificate e quale contesto resta disponibile. La stessa architettura generale può ricevere sequenze molto diverse se l'input viene rappresentato in modo coerente.

## Testo e tokenizzazione

Il testo viene segmentato in token, poi convertito in identificativi del vocabolario. Un token può corrispondere a una parola, una sottoparte, un carattere o un byte. La scelta del metodo, la dimensione del vocabolario e i dati di addestramento del tokenizer influenzano il risultato.

Gli identificativi devono essere compatibili con il modello: non basta sostituire arbitrariamente un tokenizer con un altro che produca sequenze della stessa lunghezza. Il vettore iniziale viene cercato nella tabella di embedding in corrispondenza dell'identificativo assegnato.

Token speciali possono indicare confini, separazioni e posizioni da ignorare. I limiti di contesto sono espressi in token, non in un numero universale di parole. Per documenti lunghi, il chunking deve bilanciare specificità e contesto, evitando frammenti che perdano il significato delle informazioni recuperate.

Il rapporto fra numero di token, finestra di contesto, attenzione, memoria e consumo è sviluppato in [[Token, contesto e costi computazionali dei Transformer]].

## Immagini come sequenze di patch

Un Vision Transformer suddivide l'immagine in patch. I valori di ogni patch vengono trasformati in un vettore di dimensione comune; l'informazione posizionale permette di distinguere la collocazione delle patch. Nel modello di classificazione considerato si aggiunge un class token, la cui rappresentazione finale alimenta il classificatore.

La sequenza non nasce quindi da parole, ma da regioni dell'immagine. La proiezione deve preservare le informazioni utili per il compito e rispettare la configurazione prevista dal modello.

## Video e dimensione temporale

Un video contiene variazioni nello spazio e nel tempo. Si possono campionare frame e ricavare patch da ciascuno, oppure costruire tubelet che comprendono regioni su più frame consecutivi. La scelta determina quali strutture spazio-temporali diventano unità dell'elaborazione.

La rappresentazione del tempo non può essere sostituita dal solo ordine spaziale delle patch. Occorre distinguere dove si trova una regione e quando viene osservata. Alcune architetture separano elaborazione spaziale e temporale per organizzare il costo del calcolo.

## Segnali numerici

Una sequenza fisiologica, come un ECG, contiene valori numerici anziché simboli di un vocabolario linguistico. Trattarla come sequenza è un'idea di rappresentazione, non una procedura completa di preparazione: frequenza di campionamento, segmentazione e modalità di trasformazione devono essere specificate per il modello.
