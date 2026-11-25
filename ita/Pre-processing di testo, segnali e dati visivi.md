# Pre-processing di testo, segnali e dati visivi

Capitolo: [[Adattamento, valutazione e applicazioni dei Foundation Models]]

Il pre-processing costruisce le unità su cui il modello può operare. Un Transformer riceve vettori disposti in una sequenza: il testo deve diventare token, un'immagine deve essere convertita in regioni rappresentabili e un video deve conservare anche la dimensione temporale. Queste trasformazioni decidono quali differenze il modello potrà riconoscere.

## Dal testo agli embedding

Il tokenizer segmenta il testo usando il proprio vocabolario. Un termine raro può essere diviso in più sottoparti, mentre uno frequente può corrispondere a un solo token. Il risultato dipende dalle regole e dai dati con cui il tokenizer è stato costruito: non esiste un numero fisso di token per parola.

A ogni token corrisponde un identificativo intero. L'identificativo seleziona una riga della matrice di embedding, ottenendo il vettore iniziale. Il numero assegnato al token è un indice, non una misura del suo significato: due identificativi vicini non rappresentano necessariamente parole simili. Usare un tokenizer diverso da quello previsto dal modello può selezionare righe sbagliate anche se la sequenza rimane formalmente valida.

Token speciali rappresentano confini o ruoli, come inizio della risposta e separazione fra messaggi. Quando esempi di lunghezza diversa vengono elaborati insieme, il padding ne uniforma la lunghezza e una maschera indica le posizioni da ignorare. Questa maschera ha uno scopo diverso dalla maschera causale, che impedisce di accedere ai token futuri durante la previsione autoregressiva.

## Lunghezza e segmentazione dei documenti

Se il testo supera la finestra di contesto, il troncamento elimina una parte dell'input. Per una classificazione può rimuovere proprio la frase decisiva. Nei sistemi di ricerca si ricorre invece al chunking: si dividono i documenti in segmenti, ciascuno indicizzato separatamente.

Un segmento come «non è consentito in questi casi» è poco utile se il soggetto e i casi si trovano nel paragrafo precedente. Titoli, confini semantici e una sovrapposizione fra segmenti aiutano a conservare il significato. Segmenti troppo lunghi mescolano invece argomenti diversi e occupano più contesto. La scelta deve preservare informazioni recuperabili, rispettando i [[Token, contesto e costi computazionali dei Transformer|limiti del modello]].

## Preparare audio e parlato

Un file audio contiene campioni di ampiezza nel tempo. La frequenza di campionamento indica quanti campioni descrivono un secondo: la sequenza deve rispettare quella prevista dal modello. Se il modello richiede 16 kHz, un audio a 8 kHz va ricampionato; cambiare soltanto il valore dichiarato della frequenza altererebbe l’interpretazione temporale dei campioni.

L’estrattore di caratteristiche converte poi il segnale nella rappresentazione richiesta. Whisper, per esempio, utilizza uno spettrogramma log-mel: una rappresentazione di come l’energia si distribuisce fra bande di frequenza nel tempo. La scala mel organizza le frequenze secondo una sensibilità legata all’udito, mentre la scala logaritmica comprime le differenze di intensità.

La durata richiede segmentazione, padding o troncamento compatibili con il modello. Tagliare una registrazione può interrompere una parola o perdere la frase decisiva; il padding aggiunge invece posizioni che non corrispondono a nuovo parlato. Nel riconoscimento del parlato, la preparazione acustica resta distinta dalla tokenizzazione del testo da produrre: un processor può coordinare entrambi i passaggi senza renderli la stessa operazione.

## Trasformare un'immagine in token

Nel Vision Transformer un'immagine di altezza $H$ e larghezza $W$ viene divisa in patch quadrate di lato $P$. Se le dimensioni sono divisibili per $P$, il numero di patch è $N=HW/P^2$. Un'immagine di 224 per 224 pixel con patch di lato 16 produce quindi 196 patch.

Ogni patch viene appiattita e proiettata nella dimensione interna del modello. Un'informazione posizionale distingue, per esempio, una regione in alto a sinistra da una in basso a destra. Nel classificatore ViT si aggiunge anche un class token, la cui rappresentazione finale viene usata dalla testa di classificazione. Ridurre il lato delle patch aumenta il dettaglio disponibile, ma anche il numero di token e il costo dell'attenzione.

## Conservare il tempo nei video

Una clip video contiene sia una griglia spaziale sia una sequenza temporale. Nel campionamento uniforme dei frame si scelgono immagini a intervalli regolari e si divide ciascuna in patch 2D. Gli embedding dei frame vengono concatenati: con $T$ frame e $S$ patch per frame si ottengono $TS$ token. Le informazioni posizionali devono permettere di distinguere sia dove si trovi la regione sia quando venga osservata.

Il Tubelet Embedding costruisce invece un token da una porzione di spazio su più frame consecutivi. Si appiattisce il blocco tridimensionale e lo si proietta nella dimensione degli embedding. In questo modo una parte dell’informazione temporale viene combinata già nella costruzione del token; con patch per frame viene invece combinata nell’elaborazione successiva. La tubelet occupa coordinate spaziali fisse e non insegue automaticamente un oggetto in movimento.

Con otto frame di 32 per 32 pixel e patch di lato 16 si producono 32 token. Raggruppando gli stessi frame in tubelet lunghe due frame, con la stessa estensione spaziale, si producono 16 token. La sequenza più corta riduce il costo, ma il raggruppamento comprime i dettagli dell’intervallo. Anche la distanza fra frame campionati conta: un evento breve può cadere fra due campioni.

La costruzione dei token va distinta dall’architettura che li elabora. ViViT può usare attenzione congiunta spazio-temporale, due encoder in successione oppure una fattorizzazione delle operazioni o delle teste. Queste alternative sono spiegate in [[Vision Transformer e imaging medico]]: cambiano quali token si confrontano e quando vengono fuse le informazioni.

## Sequenze di segnali

Un ECG è una sequenza di misure nel tempo. Segmentarlo significa scegliere quali intervalli presentare al modello e come convertirli in vettori, non cercare identificativi in un vocabolario di parole. La frequenza di campionamento determina la distanza temporale fra misure; alterarla senza conservarne il significato cambia la rappresentazione del fenomeno. La preparazione deve quindi rispettare la struttura del segnale e la configurazione con cui il modello è stato addestrato.
