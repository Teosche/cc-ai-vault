# Da hello alla risposta in Qwen3.5

Capitolo: [[Architettura Transformer e Foundation Models]]

Quando un modello riceve hello, non elabora direttamente cinque lettere come farebbe un programma che confronta stringhe. Il testo viene convertito in identificatori, poi in vettori che attraversano la rete. Il risultato finale è un insieme di punteggi sui possibili token successivi. Il programma di generazione sceglie un token, lo aggiunge al contesto e ripete il processo. In [[Qwen3.5 4B architettura e funzionamento]] questo percorso attraversa 32 blocchi con meccanismi differenti.

## Dalle lettere all'identificatore

Il tokenizer suddivide il testo in unità del proprio vocabolario. Una parola può corrispondere a un token oppure richiederne diversi; anche spazi, punteggiatura e marcatori speciali partecipano alla codifica. Il tokenizer deve essere compatibile con il modello: gli identificatori non hanno un significato universale.

La codifica BPE applica regole di fusione apprese durante la costruzione del tokenizer. Per hello, le regole del GGUF locale uniscono prima e con l, poi l con o, quindi el con lo e infine h con ello. Il risultato è un solo token, hello, associato all'ID 14556. Non viene ricalcolata la frequenza delle coppie nella conversazione corrente: si applicano priorità già memorizzate.

| Testo di input | ID nel tokenizer locale |
| :--- | ---: |
| `hello` | 14556 |
| `Hello` | 9419 |
| ` hello` con spazio iniziale | 23066 |
| ` world` con spazio iniziale | 1814 |

Maiuscole e spazi possono quindi cambiare il token. Il simbolo Ġ, eventualmente mostrato dagli strumenti prima di hello o world, rappresenta internamente lo spazio: non è una lettera aggiunta al testo originale.

Nella chat reale vengono codificati anche i ruoli dei messaggi, le istruzioni e la cronologia. Un messaggio utente con hello è racchiuso da delimitatori e seguito dall'indicazione che deve parlare l'assistente. Il prossimo token viene previsto dopo l'ultimo elemento dell'intero prompt, non necessariamente subito dopo hello. Completare una stringa e rispondere a un messaggio sono perciò due richieste differenti.

## La matrice di embedding

L'ID 14556 è un indice, non una descrizione numerica del significato di hello. Il modello usa quell'indice per recuperare una riga della matrice di [[Metodi di embedding|embedding]]. La riga contiene 2.560 valori appresi e costituisce la rappresentazione iniziale del token.

La configurazione del componente linguistico stabilisce 248.320 righe, indicate da vocab_size, e 2.560 componenti per riga, indicate da hidden_size. Il primo valore è la dimensione del vocabolario predisposto per il modello, comprensiva dell'eventuale padding; il secondo è una scelta dell'architettura. Non derivano dalla lunghezza di hello. Le dimensioni vengono stabilite nella progettazione; il contenuto numerico viene appreso nell'addestramento.

L'unico prodotto necessario per contare i parametri di questa matrice è **248.320 × 2.560 = 635.699.200**. Il risultato indica il numero di coefficienti dell'intera tabella. Per hello viene recuperata una sola riga, quindi un vettore di 2.560 componenti.

Queste componenti non sono etichette leggibili come inglese, saluto o cordialità. L'informazione è distribuita. L'embedding iniziale di uno stesso ID è fisso durante l'inferenza; le rappresentazioni costruite nei blocchi cambiano invece con il prefisso. Nel decoder causale non possono dipendere da parole future.

## Il percorso nei blocchi

L'ID non cambia mentre attraversa il modello: viene trasformato il vettore associato alla sua posizione. Il primo blocco riceve l'embedding; il secondo riceve il risultato del primo e così via. Non ci sono 32 parole intermedie né una traduzione obbligatoria del token in concetti nominati.

I primi tre blocchi sono Gated DeltaNet. La rappresentazione produce vettori e segnali con cui aggiornare e interrogare lo stato ricorrente del blocco. Questo stato riassume associazioni del prefisso e viene corretto considerando la differenza fra informazione nuova e informazione già recuperabile. I gate regolano conservazione e aggiornamento. Ogni blocco possiede pesi e stato propri.

Il quarto blocco usa la full attention. La rappresentazione di hello che vi entra è già stata trasformata tre volte: query, key e value non vengono quindi calcolati direttamente dall'embedding originale. Il gruppo di quattro blocchi si ripete otto volte, fino al blocco 32.

Ogni blocco include normalizzazione, un percorso residuale e una feed-forward. La normalizzazione regola la scala dei valori; il percorso residuale aggiunge il contributo calcolato alla rappresentazione precedente; la feed-forward espande e trasforma il vettore prima di riportarlo alla dimensione del modello. Non è assegnato manualmente a ogni blocco un compito come grammatica o significato: la distribuzione delle trasformazioni viene appresa.

## Query key e value di hello

Una query è una proiezione della rappresentazione corrente usata per valutare la compatibilità con altre posizioni. Una key è una proiezione che permette questo confronto. Un value contiene l'informazione che verrà combinata. Sono vettori numerici, non tre stringhe o tre proprietà permanenti della parola.

Le matrici che producono questi vettori sono condivise fra le posizioni del blocco. Non esiste una matrice query dedicata a hello. Gli stessi pesi applicati a rappresentazioni differenti producono risultati differenti. La query di hello nel blocco 4 può quindi essere diversa da quella nel blocco 8, e in ciascun blocco esistono più teste.

Consideriamo un esempio didattico con le posizioni say e hello. Se la query della posizione hello attribuisce a quelle posizioni coefficienti 0,7 e 0,3, l'uscita della testa combina il 70% del value di say e il 30% del value di hello. Le key determinano i coefficienti del recupero; i value determinano il contenuto combinato. I coefficienti dell'esempio non sono attivazioni misurate di Qwen.

La maschera causale impedisce a hello di usare le posizioni future. Se l'intero input fosse soltanto un token, senza delimitatori, l'attenzione avrebbe una sola posizione disponibile e le assegnerebbe coefficiente 1. Il blocco continuerebbe comunque a trasformare il vettore attraverso le altre operazioni.

Nel Qwen reale i contributi delle teste vengono riuniti, modulati e proiettati nuovamente a 2.560 componenti. Le informazioni posizionali, introdotte tramite RoPE su query e key, permettono di tenere conto anche dell'ordine. Per ottenere i valori numerici effettivi di una query occorre eseguire il modello con un contesto e un blocco precisi.

## Dalla rappresentazione finale ai token candidati

Dopo il blocco 32 e la normalizzazione finale, la testa linguistica produce un logit per ciascuna posizione del vocabolario. Un logit è un punteggio non normalizzato. Non è ancora una probabilità e non misura la verità di una possibile frase. In Qwen la proiezione d'uscita condivide i pesi della matrice di embedding, evitando una seconda tabella indipendente delle stesse dimensioni.

La softmax trasforma i punteggi in una distribuzione di probabilità. Per generare la continuazione si usano quelli relativi all'ultima posizione del prompt. La rete produce quindi punteggi; il generatore seleziona un ID; il tokenizer riconverte gli ID in testo. L'API può presentare all'utente la stringa risultante.

## Temperatura top k e top p

Il greedy decoding sceglie il token con il punteggio più alto a ogni passo. Il sampling estrae invece un token dalla distribuzione. La temperatura controlla quanto questa distribuzione è concentrata: valori positivi più bassi favoriscono maggiormente i candidati principali, quelli più alti distribuiscono maggiormente la probabilità. Queste impostazioni non modificano i pesi.

Immaginiamo, soltanto per illustrare la scelta, che i candidati abbiano queste probabilità:

| Candidato | Probabilità |
| :--- | ---: |
| ` world` | 40% |
| `!` | 25% |
| ` there` | 15% |
| `,` | 10% |
| ` friend` | 6% |
| Fine della risposta | 4% |

Con top-k uguale a 3 rimangono soltanto i primi tre candidati, le cui probabilità vengono rinormalizzate. Se si campiona, ciascuno dei tre può ancora essere scelto: top-k non equivale a prendere sempre il primo.

Con top-p uguale a 0,75 si conserva il gruppo minimo dei candidati più probabili che raggiunge almeno il 75% complessivo. I primi due arrivano al 65%; aggiungendo il terzo si raggiunge l'80%, quindi vengono conservati tre token. Con soglia 0,60 ne basterebbero due. Top-k fissa il numero dei candidati, top-p una massa cumulativa; il numero conservato da top-p varia con la distribuzione.

Il generatore può combinare filtri e vincoli di formato, secondo l'ordine previsto dal runtime. Una scelta più prevedibile o un JSON grammaticalmente valido non garantiscono la correttezza fattuale.

## Continuazione e riuso del passato

Se nell'esempio viene scelto il token 1814, il testo diventa hello world. Il nuovo ID viene trasformato nel proprio embedding e attraversa tutti i blocchi per prevedere il token seguente. L'esempio descrive una possibile continuazione, non una risposta garantita di Qwen.

Non occorre ricalcolare integralmente il prefisso. Nei blocchi di full attention la KV cache conserva key e value precedenti; nei blocchi DeltaNet viene riutilizzato e aggiornato lo stato ricorrente. Sono strutture temporanee, non modifiche permanenti ai pesi. Il prefill elabora il prompt iniziale, mentre il decode genera progressivamente la continuazione. Il processo termina con un segnale di fine, un limite di lunghezza o un'altra condizione di arresto.
