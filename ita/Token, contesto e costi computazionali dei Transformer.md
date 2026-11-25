# Token, contesto e costi computazionali dei Transformer

Capitolo: [[Architettura Transformer e Foundation Models]]

Quando chiediamo a un modello di riassumere un documento, deve prima elaborare il testo ricevuto e poi generare una risposta. Il numero di **token** determina quanto testo deve trattare; la **finestra di contesto** stabilisce quanto può tenerne disponibile; l'attenzione e le altre operazioni determinano il lavoro necessario. Seguire questa richiesta permette di distinguere spazio disponibile, calcolo, memoria e costo.

## Dal documento ai token

Il **tokenizer** divide il testo in unità del proprio vocabolario: parole intere, sottoparole, caratteri o byte, secondo il metodo adottato. Con le sottoparole, una parola rara può essere rappresentata combinando pezzi già noti. BPE, WordPiece e Unigram costruiscono queste unità con criteri diversi.

Un esempio concreto è la parola inglese «Subject»: un tokenizer può rappresentarla con i token «Sub» e «ject». Generare quella parola richiede allora due token. Anche punteggiatura e marcatori speciali possono occupare posizioni. La segmentazione cambia con il tokenizer e la lingua: non esiste una conversione universale fra parole e token.

A ogni token corrisponde un **identificativo**, usato per recuperare il suo [[Metodi di embedding|embedding]], il vettore elaborato dal modello. Token, identificativo ed embedding sono quindi passaggi diversi della trasformazione del testo in dati numerici.

I **parametri** sono invece i valori appresi della rete. Un modello con sette miliardi di parametri conserva gli stessi pesi sia ricevendo cento token sia ricevendone mille. Aumentare il testo richiede più elaborazione e memoria temporanea, senza aggiungere parametri.

## Quanto documento e quanta risposta entrano nel contesto

Il **contesto** è la sequenza disponibile alla richiesta: può includere istruzioni, messaggi precedenti, documento e domanda. La **finestra di contesto** ne limita la lunghezza. Nella generazione decoder-only ordinaria, senza rimuovere parti della sequenza, anche i token prodotti occupano posizioni:

$$
N_{input}+N_{output}\leq C.
$$

Qui $C$ è la capacità configurata. Se vale 8192 token e l'input completo ne occupa 7000, rimangono al massimo **1192 token per la risposta**. Un servizio può imporre anche un limite di output inferiore. Contare soltanto la domanda corrente esclude il resto del materiale effettivamente inviato.

Se la cronologia viene reinviata a ogni turno, una chat più lunga aumenta l'input delle richieste successive. Se il documento non entra, occorre selezionare passaggi, dividerlo, riassumerlo o troncarlo: ogni scelta può perdere informazioni. Il [[RAG]] recupera passaggi pertinenti per fornire al generatore evidenze selezionate. Avere spazio per un documento intero non garantisce comunque che il modello ne utilizzi correttamente ogni dettaglio.

## Elaborare il prompt e generare la risposta

Nel **prefill**, il modello elabora i token del prompt e costruisce le rappresentazioni che serviranno alla generazione. Molte operazioni sulle posizioni possono essere eseguite in parallelo. Da questa elaborazione viene scelto il primo token della risposta.

Nel **decode**, il token scelto viene elaborato per prevedere il successivo, che a sua volta entra nel contesto. Per produrre «Il gatto dorme», assumendo per semplicità un token per parola, la scelta di «gatto» avviene dopo quella di «Il», e «dorme» dopo «gatto». Questa dipendenza rende progressiva la generazione ordinaria.

Un documento lungo aumenta il lavoro iniziale; una risposta lunga richiede più passi successivi. **Attesa prima del primo token** e **tempo necessario a completare la risposta** descrivono quindi aspetti diversi della richiesta.

## Perché un input lungo fa crescere il lavoro dell'attenzione

La [[Architettura Transformer e meccanismo di attenzione|self-attention densa]] confronta ogni query con le key della sequenza. Con $n$ token, la matrice dei confronti ha $n$ righe e $n$ colonne, quindi $n^2$ posizioni. È questo il significato della **crescita quadratica**:

| Lunghezza | Posizioni nella matrice dei confronti |
| ---: | ---: |
| 1000 token | 1.000.000 |
| 2000 token | 4.000.000 |

Raddoppiare la lunghezza quadruplica le posizioni della matrice, per testa e per livello. La maschera causale ne vieta una parte, ma le coppie consentite crescono ancora quadraticamente. A dimensioni del modello fissate, il costo dei prodotti di attenzione sull'intera sequenza è $O(n^2)$, oppure $O(n^2d_k)$ esplicitando la dimensione delle key.

Questo non significa che tutta la richiesta impieghi esattamente quattro volte il tempo: feed-forward, altre operazioni e hardware incidono diversamente. **FlashAttention** calcola l'attenzione senza conservare l'intera matrice in memoria e riduce i trasferimenti di dati; mantiene però l'attenzione densa esatta e la sua crescita quadratica delle interazioni. L'attenzione locale o sparsa riduce invece le coppie consultabili.

Lo stesso ragionamento vale per le patch dei Transformer visivi. A risoluzione fissa, dimezzare il lato delle patch quadruplica il numero di patch e, considerando soltanto quei token, moltiplica per sedici le coppie dell'attenzione densa. Non esiste però una conversione universale fra token visivi e testuali.

## Come la KV cache evita di ripetere lavoro

Quando arriva un nuovo token, ricalcolare da zero tutto il testo precedente sarebbe uno spreco. In un decoder causale, le rappresentazioni dei token precedenti non cambiano per effetto di quelli futuri. La **KV cache** conserva quindi, a ogni livello, i vettori key e value già calcolati e li riutilizza.

Se sono disponibili 1000 token e ne viene aggiunto uno, si calcolano le nuove key e value e si confronta la nuova query con le key disponibili. Non occorre ricostruire tutte le rappresentazioni passate. Il modello deve comunque consultare il contesto: il costo dell'attenzione **per singolo passo con cache** cresce linearmente con la sua lunghezza. Produrre molti token richiede molti passi, quindi questa proprietà non rende costante il costo dell'intera risposta.

**Il vantaggio si paga in memoria**. Una cache densa occupa circa:

$$
M_{KV}\approx 2\,L\,B\,n\,H_{KV}\,d_h\,s.
$$

Il fattore 2 conta key e value; $L$ i livelli, $B$ le sequenze elaborate insieme, $n$ i token conservati, $H_{KV}$ le teste key/value, $d_h$ i valori per testa e $s$ i byte per valore. La stima esclude overhead. Contesti più lunghi e più richieste simultanee richiedono più cache; **MQA** e **GQA** riducono le teste key/value condividendole fra teste query. Il [[CAG]] sfrutta il riuso di prefissi già elaborati. La cache conserva calcoli, non risposte pronte e non nuovi pesi appresi.

## Dalla memoria ai consumi e al prezzo

La memoria deve contenere anche i pesi. Con $P$ parametri a $b$ bit, servono circa $Pb/8$ byte: sette miliardi di parametri a 16 bit occupano **14 GB decimali per i soli pesi**. Cache, attivazioni e memoria di lavoro aumentano il totale; nell'addestramento si aggiungono gradienti e stato dell'ottimizzatore.

Le ottimizzazioni agiscono su componenti diverse. La **quantizzazione** riduce i bit, con effetti da verificare sulla qualità; il **batching** elabora più richieste insieme per sfruttare l'hardware, senza garantire meno attesa a ciascun utente. **PagedAttention** organizza la cache in blocchi per ridurre sprechi e frammentazione.

Il **calcolo** conta operazioni, la **memoria** misura spazio occupato, il **tempo** misura la durata e l'**energia** dipende dalla potenza assorbita nel tempo, $E=\int P(t)\,dt$. Il pre-training ripete elaborazioni e aggiornamenti su grandi quantità di dati; l'inferenza usa i pesi appresi. Riutilizzare checkpoint evita nuovi addestramenti da zero, ma molte richieste accumulano comunque consumi. Per stimare energia ed emissioni servono misure, non soltanto un conteggio di token.

Il prezzo di un servizio può distinguere token in ingresso e in uscita. Con tariffe per milione $r_{in}$ e $r_{out}$, uno schema elementare è $\operatorname{costo}=N_{in}r_{in}/10^6+N_{out}r_{out}/10^6$. Tariffe ed eventuali sconti sulla cache dipendono dal servizio: **crescita del calcolo, energia e fattura sono grandezze diverse**.
