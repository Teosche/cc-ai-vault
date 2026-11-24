# Ottimizzazione

Capitolo: [[Problem-solving, ricerca e similarità]]

Ottimizzare significa **scegliere**, fra le alternative consentite, quelle che assumono il valore migliore rispetto a un criterio. Una soluzione può soddisfare un obiettivo senza essere la più veloce, economica o accurata. L'ottimizzazione rende esplicito questo confronto attraverso una funzione da minimizzare o massimizzare.

## Dalla soluzione ammissibile al criterio di qualità

Nel problema del taxi, raggiungere una destinazione è un obiettivo; scegliere fra percorsi differenti richiede anche valutazioni di durata, sicurezza e costo. Negli [[Utility-Based Agents|agenti basati su utilità]], una funzione interna esprime le preferenze fra esiti. Se gli esiti sono incerti, il confronto considera la loro utilità attesa.

Nell'apprendimento di un modello, le alternative possono essere vettori di parametri anziché percorsi. Il problema diventa trovare i parametri che riducono una funzione di errore. Rimane la stessa struttura generale: un insieme di candidati e un criterio di confronto. Cambiano però lo spazio delle soluzioni e i metodi utilizzati per esplorarlo.

## L'esempio dei minimi quadrati

Nella regressione lineare una previsione assume la forma

$$
\widehat y_i=\theta_0+\sum_{j=1}^{d}\theta_jx_{ij}.
$$

Le caratteristiche $x_{ij}$ e i valori osservati $y_i$ sono dati; i coefficienti $\theta_j$ devono essere scelti. Il criterio dei minimi quadrati minimizza la somma degli scarti al quadrato:

$$
L(\theta)=\sum_{i=1}^{N}(y_i-\widehat y_i)^2.
$$

Elevare gli scarti al quadrato evita la compensazione fra errori positivi e negativi e attribuisce un peso maggiore agli errori più grandi. L'obiettivo non è memorizzare un singolo scarto, ma ridurre l'errore complessivo del modello sui dati considerati.

Introducendo nella matrice $X$ una colonna di valori 1 per l'intercetta, le previsioni sono $X\theta$ e la funzione diventa $L(\theta)=(y-X\theta)^\top(y-X\theta)$. Annullare il gradiente conduce alle equazioni normali:

$$
X^\top X\theta=X^\top y.
$$

Quando $X^\top X$ è invertibile, la soluzione si scrive $\theta=(X^\top X)^{-1}X^\top y$. La condizione di invertibilità è essenziale per questa scrittura: non si può applicare l'inversa a una matrice singolare. Le equazioni normali sono quindi distinte dalla particolare forma con l'inversa.

## Aggiornamenti mediante gradiente

Un approccio iterativo modifica i parametri nella direzione opposta al gradiente della funzione da minimizzare:
$$
\theta_{t+1}=\theta_t-\eta\nabla L(\theta_t),
$$

dove $\eta$ è il learning rate. Il gradiente descrive la variazione locale della funzione; il passo negativo cerca di ridurne il valore. Il risultato dipende anche dall'ampiezza del passo: valori troppo grandi possono produrre instabilità, mentre valori molto piccoli possono rallentare l'apprendimento. La formula non garantisce da sola il raggiungimento di un ottimo globale per qualsiasi funzione.

Il batch gradient descent calcola un aggiornamento utilizzando l'intero insieme di addestramento. Lo stochastic gradient descent aggiorna i parametri su un esempio alla volta; il metodo mini-batch usa gruppi di esempi. Cambiano frequenza e costo degli aggiornamenti, oltre alla regolarità delle variazioni. Il confronto riguarda come stimare e applicare il gradiente, non tre definizioni diverse dell'obiettivo.

## Funzione da ottimizzare e valutazione del risultato

La funzione utilizzata per l'addestramento può differire dalla misura scelta per rendere interpretabili i risultati. Una regressione può essere addestrata minimizzando gli errori quadratici e valutata anche mediante l'errore assoluto medio. Quest'ultimo misura lo scarto medio in valore assoluto, senza essere una percentuale di classificazioni corrette.

È quindi necessario separare il criterio matematico che guida gli aggiornamenti dalla valutazione dell'utilità del modello per il compito. Ridurre una funzione sui dati di addestramento non rende automaticamente adeguata qualsiasi previsione su dati nuovi.
