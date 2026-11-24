---
tags:
  - ragionamento
  - argomentazione
  - spiegabilita
  - giustificare
---

Capitolo: [[Ragionamento, inferenza causale e spiegazione]]

Argomentare significa rendere esplicite le ragioni che sostengono una conclusione; spiegare significa chiarire un risultato, un comportamento o una decisione rispetto a una domanda. Le due attività si incontrano quando un sistema artificiale deve rendere comprensibile perché abbia prodotto una risposta e su quali informazioni si sia basato. Una trattazione rigorosa deve tenere insieme tre aspetti: il sostegno alla conclusione, il comportamento effettivo del sistema e la possibilità di controllare il rapporto fra i due.

Una risposta linguisticamente convincente non soddisfa da sola queste esigenze. Può presentare un argomento scorretto, utilizzare premesse false oppure descrivere un procedimento diverso da quello che ha determinato la decisione. Per valutare una spiegazione bisogna quindi prima chiarire che cosa si voglia giustificare e poi scegliere strumenti adeguati a quell'oggetto.

## Dalle premesse alla conclusione

Un argomento rende riconoscibili le informazioni assunte come premesse e la conclusione ricavata da esse. Nel sillogismo «tutti gli uomini sono mortali; Socrate è un uomo; dunque Socrate è mortale», la conclusione segue necessariamente dalle due premesse. La struttura consente di verificare il passaggio: la proprietà attribuita a tutti gli uomini viene applicata a un individuo appartenente a quella categoria.

Questa necessità caratterizza il [[Ragionamento deduttivo]], ma non qualsiasi ragionamento. Nel [[Ragionamento induttivo]], i casi osservati sostengono una generalizzazione che si estende oltre di essi senza essere garantita definitivamente. Presentare bene le ragioni significa anche indicare questa differenza: un precedente favorevole o una regolarità empirica non devono essere esposti come una prova deduttiva.

La validità del passaggio va inoltre distinta dalla verità delle premesse. L'inferenza «se piove, il terreno è asciutto; piove; dunque il terreno è asciutto» ha una forma valida, ma dipende da una regola empiricamente inadeguata. Spiegare il passaggio rende visibile il problema e permette di localizzarlo nella premessa, anziché confonderlo con un errore della forma logica.

Nel caso di un sistema artificiale, alla domanda «quali ragioni sostengono la risposta?» si aggiunge una domanda sul comportamento: «quali informazioni hanno effettivamente influenzato il sistema?». Le due domande sono collegate, ma una giustificazione plausibile non dimostra ancora che il modello abbia utilizzato proprio quelle ragioni.

## Dalla risposta corretta alla comprensione del modello

Consideriamo un classificatore che riconosce un'ape in un'immagine contenente anche un fiore. La risposta è corretta, ma non chiarisce se il modello abbia riconosciuto l'insetto o sfruttato il contesto. Se anche un'immagine di solo fiore viene classificata come ape, la qualità del primo risultato non basta più a giustificare fiducia nel comportamento complessivo. Serve un'analisi di ciò a cui il modello risponde.

La spiegazione può allora servire a capire perché sia stata prodotta quella classe, perché ne sia stata esclusa un'altra, quando il sistema possa fallire e come correggerlo. L'obiettivo determina anche il livello dell'analisi. Una spiegazione locale riguarda una singola previsione o una parte limitata dello spazio degli input; una spiegazione globale cerca di descrivere il comportamento del modello su un insieme più ampio di situazioni.

Nel [[Ragionamento basato su casi]], dire che un film è stato consigliato perché simile ad altri apprezzati dall'utente giustifica quella raccomandazione attraverso precedenti riconoscibili. Non descrive però tutte le raccomandazioni che il sistema produrrebbe. Analogamente, individuare i pixel rilevanti per una fotografia non basta a sapere se il classificatore utilizzi sistematicamente la stessa caratteristica. Passare da spiegazioni locali a una visione globale richiede di confrontare più casi, tenendo conto che l'aggregazione può nascondere differenze importanti.

Un'altra distinzione riguarda il momento e il modo in cui si ottiene la spiegazione. Nei modelli intrinsecamente interpretabili, come modelli lineari mantenuti sufficientemente semplici, la struttura permette un'analisi diretta. I metodi post-hoc analizzano invece un modello già addestrato, anche quando la sua struttura è complessa. Questi metodi possono utilizzare proprietà interne specifiche, come i gradienti delle reti neurali, oppure essere applicabili a famiglie diverse di modelli. Questi criteri sono indipendenti dalla distinzione locale/globale: descrivono gli strumenti disponibili, non una graduatoria di correttezza delle spiegazioni.

## Attribuire una previsione alle caratteristiche dell'input

Per indagare il classificatore dell'ape, una possibilità è stimare quali caratteristiche influenzino il punteggio della classe. Il gradiente descrive quanto varia localmente quel punteggio quando si modifica una caratteristica dell'input. Se una piccola modifica di un pixel produce una variazione elevata del punteggio, il gradiente in quella direzione è elevato. Si ottiene così una misura della sensibilità locale del modello.

Sensibilità e importanza non coincidono però in ogni situazione. In una regione di saturazione, il punteggio può cambiare poco e il gradiente risultare piccolo anche quando l'informazione presente nell'input ha avuto un ruolo importante. Guardare soltanto il punto finale può quindi nascondere il contributo di caratteristiche significative.

Gli Integrated Gradients affrontano il problema confrontando l'input con un riferimento, detto baseline. Fra i due si costruisce un percorso di input intermedi; su questi punti si calcolano i gradienti del punteggio rispetto alle caratteristiche. La media dei contributi lungo il percorso viene poi moltiplicata per la differenza fra input e baseline. L'attribuzione considera così il cambiamento dal riferimento all'input, anziché la sola sensibilità nell'input finale.

La baseline è parte del significato della spiegazione: scegliere un riferimento diverso può modificare le attribuzioni. Il metodo richiede differenziabilità e produce principalmente un'analisi locale; attribuire un punteggio alle singole caratteristiche può inoltre non rendere esplicite tutte le loro interazioni. Per interpretare la mappa occorre quindi sapere quale confronto sia stato effettuato e quale limite abbia la scomposizione adottata.

Per le immagini, CAM e Grad-CAM permettono invece di visualizzare regioni associate a una classe attraverso le mappe di attivazione convoluzionali. CAM richiede una struttura nella quale le mappe finali siano aggregate mediante global average pooling, cioè una media spaziale per ciascuna mappa, e seguite da uno strato di classificazione. Grad-CAM utilizza i gradienti del punteggio della classe per assegnare pesi alle mappe e combinarle, mantenendo un'informazione spaziale sulle regioni rilevanti. Generalizza così CAM senza richiedere esattamente la stessa architettura, purché siano disponibili le attivazioni e i gradienti necessari.

Nel caso dell'ape, una mappa concentrata sul fiore può suggerire che il contesto sostenga la previsione. Confrontare mappe prodotte per classi diverse o da modelli differenti aiuta a indagare i bias. Rimane un'analisi della previsione: una regione evidenziata non costituisce automaticamente una causa del fenomeno nel mondo. Questo limite collega l'interpretabilità all'[[Inferenza causale]].

## Dall'individuazione delle caratteristiche alla spiegazione testuale

Una mappa mostra dove si concentra un'attribuzione, ma può non chiarire quale concetto accomuni gli elementi rilevanti. Per esplorare questo aspetto, DiffExplainer permette di esaminare caratteristiche che attivano un classificatore. Se immagini ricche di fiori sostengono la classe «ape» anche senza l'insetto, diventa possibile formulare un'ipotesi sulla dipendenza dal contesto. L'analisi passa dalla sola correttezza dell'etichetta all'esame delle regolarità sfruttate dal modello.

DEXTER collega questa esplorazione a una spiegazione linguistica. Una descrizione come «A picture of a flower» orienta il generatore di immagini Stable Diffusion; il classificatore valuta quanto le immagini ottenute attivino la classe che si vuole analizzare. La ricerca delle descrizioni è collegata alla massimizzazione di questa attivazione: interessa individuare contenuti capaci di sostenere la previsione. Le immagini vengono poi descritte mediante didascalie e un'elaborazione testuale combina le informazioni in un report. Il risultato cerca così di rendere leggibili le caratteristiche che sostengono il comportamento del classificatore.

Nell'esempio dell'ape, la generazione di un fiore capace di attivare la classe può sostenere l'ipotesi che il modello dipenda dall'habitat associato all'insetto. Il report deve conservare questo grado di certezza: propone un'interpretazione delle evidenze raccolte, da controllare su ulteriori situazioni. Una descrizione chiara è utile perché comunica l'ipotesi e i suoi indizi; non rende automaticamente universale la conclusione.

## Giustificare le risposte e le azioni degli agenti linguistici

Negli agenti linguistici, la spiegazione deve considerare anche la sequenza che conduce all'azione. Fra obiettivo, interpretazione della richiesta, costruzione del piano ed esecuzione possono comparire errori diversi. Osservabilità, registrazione degli eventi e controllo degli output aiutano a capire in quale passaggio sia emerso il problema: un piano coerente può essere stato costruito a partire da un obiettivo frainteso, oppure essere stato eseguito in modo scorretto.

In [[CoALA]], il ragionamento elabora il contenuto della memoria di lavoro, mentre il dialogo può includere confronto e risoluzione collaborativa dei compiti. Strategie come Chain-of-Thought, Tree-of-Thoughts e dibattito fra modelli cercano di migliorare l'elaborazione rendendo disponibili passaggi intermedi o alternative. Questi elementi offrono materiale da controllare, ma la loro presenza non dimostra la validità della conclusione: possono permanere bias, difficoltà di spiegazione e un prolungamento dell'elaborazione senza un corrispondente miglioramento del risultato.

Per controllare il sostegno informativo della risposta, [[RAG]] aggiunge un collegamento esplicito con documenti recuperati. Il generatore riceve domanda e passaggi pertinenti, e può associare alle affermazioni citazioni verificabili. Questo permette di domandarsi se una certa informazione sia effettivamente presente nel documento e se giustifichi il passaggio compiuto. La citazione aiuta quindi a ricostruire l'argomento, purché venga verificata la corrispondenza fra evidenza e conclusione.

Anche il recupero basato su grafi può rendere seguibili i collegamenti fra entità ed evidenze, ma attraversare una sequenza di relazioni non basta a dimostrare la correttezza della risposta. Occorre controllare il significato di quelle relazioni e l'inferenza costruita su di esse. Nei sistemi linguistici come nei classificatori, la spiegazione è utile quando rende distinguibili i dati disponibili, il procedimento analizzato e la portata della conclusione.
