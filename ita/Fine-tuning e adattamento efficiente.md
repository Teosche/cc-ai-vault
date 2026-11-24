# Fine-tuning e adattamento efficiente

Capitolo: [[Adattamento, valutazione e applicazioni dei Foundation Models]]

Il fine-tuning prosegue l'addestramento di un modello preaddestrato su un compito o insieme di dati più specifico. Riutilizza rappresentazioni già acquisite e modifica parametri per adattarne il comportamento. È distinto dal prompting, che condiziona l'inferenza tramite il contesto.

## Adattamento supervisionato e istruzioni

Nel fine-tuning supervisionato gli esempi associano input e output desiderati. L'instruction fine-tuning utilizza richieste e risposte per favorire un comportamento coerente con le istruzioni. Questo passaggio è importante perché un modello addestrato a continuare testo non coincide automaticamente con un assistente conversazionale.

Aggiornare tutti i pesi offre maggiore libertà di adattamento, ma richiede memoria e calcolo. Non garantisce, da solo, prestazioni migliori in ogni situazione: disponibilità dei dati, obiettivo e procedura di valutazione restano determinanti.

## Linear probing e parametri congelati

Si può mantenere congelato il modello di rappresentazione e addestrare soltanto una testa o un classificatore che ne utilizzi le caratteristiche. Il costo di addestramento si riduce perché il numero di parametri aggiornati è limitato. Il limite è che le rappresentazioni precedenti non vengono adattate al nuovo compito.

Il Parameter-Efficient Fine-Tuning cerca modalità di adattamento con pochi parametri aggiuntivi o selezionati. La scelta dipende da dove sia necessario intervenire: input, stati intermedi o trasformazioni del modello.

## Prompt tuning e prefix tuning

Nel prompt tuning si apprendono vettori aggiunti all'input, mantenendo congelato il modello. Sono parametri continui addestrabili, non necessariamente parole leggibili. Il metodo va quindi distinto dalla scrittura manuale di una frase nel prompt.

Il prefix tuning introduce rappresentazioni apprese utilizzate nei livelli interni; il P-tuning impiega un prompt encoder per parametrizzare i vettori. Questi metodi cambiano il punto d'intervento e la procedura di ottimizzazione. Non sono sinonimi, anche se condividono il riuso di un modello congelato.

## LoRA e aggiornamento a basso rango

LoRA rappresenta una variazione di una matrice di pesi mediante fattori di rango ridotto. Con $W\in\mathbb R^{m\times n}$ si può scrivere

$$
W_{\mathrm{adattato}}=W+sBA,
\qquad B\in\mathbb R^{m\times r},\quad A\in\mathbb R^{r\times n},
$$

dove $r$ è piccolo rispetto alle dimensioni originali e $s$ è un fattore di scala. Si apprendono i fattori mantenendo congelato $W$. I parametri dei fattori sono $r(m+n)$ anziché $mn$ per l'intera matrice.

Per una trasformazione fra 512 e 2048 dimensioni, il peso completo contiene 1.048.576 coefficienti; con rango 8 i fattori ne contengono 20.480, escludendo bias e altri componenti. Il risparmio riguarda quella trasformazione, non automaticamente l'intera memoria necessaria all'addestramento.

La scelta delle matrici da adattare è progettuale: LoRA non è definita dall'applicazione esclusiva ai feed-forward. Non è neppure necessario promettere prestazioni equivalenti al fine-tuning completo per ogni modello e compito.

## Preferenze e rinforzo

L'RLHF aggiunge un altro tipo di segnale: si costruisce un modello di ricompensa che valuta risposte e si adatta una policy per aumentare il punteggio. Questa descrizione non specifica da sola l'algoritmo di ottimizzazione. In particolare, una citazione di RLHF non basta a documentare [[Proximal Policy Optimization]].
