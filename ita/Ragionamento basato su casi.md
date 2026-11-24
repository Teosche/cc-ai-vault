---
tags:
  - ragionamento
  - casi
  - memoria-episodica
  - similarita
---
Capitolo: [[Ragionamento, inferenza causale e spiegazione]]

Il **ragionamento basato su casi** (*Case-Based Reasoning*, CBR) affronta un nuovo problema utilizzando esperienze precedenti pertinenti. Il suo riferimento caratteristico è il caso concreto: una situazione già incontrata, le informazioni che la descrivono e, quando disponibili, la soluzione adottata e il risultato ottenuto. Il **passato diventa una risorsa per interpretare il presente**, senza richiedere che tutta l'esperienza sia stata prima condensata in regole generali.

Il problema centrale è capire quando un'esperienza passata sia davvero utile. Aver già incontrato una situazione non basta: occorre conservarne gli aspetti significativi, ritrovarla quando serve e stabilire che cosa possa essere trasferito al problema corrente. Questi passaggi collegano memoria e ragionamento: la memoria rende disponibile il precedente, mentre il ragionamento ne valuta la pertinenza e il possibile impiego.

## Che cosa rende un'esperienza un caso utilizzabile

Un caso deve essere rappresentato in modo da permettere il confronto. In una partita di baseball, per esempio, il solo ricordo «è stato effettuato un lancio veloce» offre poche indicazioni. Diventa più informativo se include il battitore, la situazione di gioco e l'esito del lancio. In [[SOAR Cognitive Architecture|SOAR]], gli episodi possono conservare attributi come inning, giocatori sulle basi, battitore, tipo di lancio e risultato. La rappresentazione lega così un'azione alle circostanze nelle quali è stata eseguita.

Questa funzione distingue la memoria episodica da quella semantica. Sapere che cosa sia un lancio veloce è conoscenza generale sul gioco; ricordare che un certo lancio contro un determinato battitore ha prodotto un fuoricampo è conoscenza di un evento. Un episodio può contenere elementi utili alla decisione proprio perché conserva il collegamento fra situazione, azione e conseguenza.

In [[CoALA]], la stessa distinzione permette di organizzare l'esperienza degli agenti linguistici. La memoria episodica può contenere interazioni, traiettorie di gioco e resoconti di precedenti cicli decisionali; la memoria semantica conserva invece conoscenze sul mondo e sull'agente. Il fatto che un episodio sia registrato non stabilisce però quando debba essere recuperato. Perché possa sostenere un nuovo ragionamento serve un criterio di pertinenza.

## Dal caso conservato al caso pertinente

La ricerca di precedenti richiede di stabilire in che senso due situazioni siano simili. La somiglianza dipende dalle caratteristiche rappresentate: due partite possono essere vicine per punteggio e inning, ma differenti per il battitore, che potrebbe essere decisivo per la scelta del lancio. Recuperare il precedente più simile rispetto a caratteristiche poco utili può quindi produrre una raccomandazione inadeguata. La similarità è una relazione costruita per il compito, non una proprietà assoluta delle situazioni.

Quando i casi sono rappresentati da vettori numerici, una distanza permette di misurare lo scostamento fra le loro caratteristiche. Per due vettori $x,y\in\mathbb{R}^d$, la distanza euclidea è

$$
d(x,y)=\sqrt{\sum_{j=1}^{d}(x_j-y_j)^2}.
$$

Ogni differenza $x_j-y_j$ confronta la stessa caratteristica nei due casi; il quadrato evita che scarti di segno opposto si annullino. La somma combina le differenze e la radice restituisce una misura complessiva dello scostamento. Un valore piccolo indica vicinanza nella rappresentazione scelta, senza dimostrare da solo che i casi richiedano la stessa soluzione.

La scala delle caratteristiche influenza il risultato. Una variabile numericamente molto ampia può dominare il confronto anche quando è poco importante per il compito. La standardizzazione e la scelta di una distanza appropriata servono a rendere il confronto più sensato; la selezione delle caratteristiche richiede comunque conoscenza del dominio. Le [[Metriche di similarità|metriche di similarità]] traducono queste scelte in un criterio numerico di confronto.

Il recupero non deve necessariamente dipendere da una sola distanza. Negli agenti linguistici, un ricordo può essere selezionato anche perché recente o significativo. Un criterio che combina recenza, importanza e rilevanza distingue tre domande: quando è avvenuto l'episodio, quanto conta e quanto è pertinente al problema attuale. La similarità fra embedding può contribuire alla rilevanza, ma non esaurisce gli altri criteri.

## Come i precedenti possono sostenere una previsione

Il k-NN rende concreto il passaggio dalla ricerca di casi alla produzione di un risultato. Per un nuovo elemento, il metodo trova i $k$ esempi più vicini e combina i valori a essi associati. Nella classificazione, ogni vicino porta con sé una classe e il voto di maggioranza seleziona quella più rappresentata; un voto pesato può assegnare maggiore influenza ai casi più simili. Nella regressione si possono invece combinare valori numerici mediante una media.

Indicando con $N_k(x)$ i vicini del nuovo elemento $x$ e con $c(z)$ la classe del vicino $z$, il voto maggioritario si scrive

$$
\widehat c(x)\in\operatorname*{arg\,max}_{c}
\sum_{z\in N_k(x)}\mathbf{1}[c(z)=c].
$$

La funzione indicatrice vale 1 quando il vicino appartiene alla classe considerata e 0 altrimenti. La somma conta quindi quanti vicini sostengono ogni classe; l'operatore $\operatorname*{arg\,max}$ individua le classi con il conteggio più alto. Se più classi hanno lo stesso numero di voti, occorre una regola ulteriore per risolvere la parità. La formula esplicita così come gli esempi recuperati contribuiscano alla previsione.

Il k-NN è non parametrico: non riassume preventivamente l'intero insieme di esempi in un vettore di parametri di dimensione fissa. Questo non elimina le scelte progettuali. Rappresentazione, distanza, regola di aggregazione e numero di vicini influenzano il risultato; $k$ può essere scelto mediante validazione incrociata. Il metodo costituisce quindi una forma di ragionamento basato sulla memoria, ma non descrive da solo tutte le operazioni necessarie a riutilizzare e adattare una soluzione complessa.

## Dall'episodio alla modifica del comportamento

Il riuso dell'esperienza può andare oltre una previsione ottenuta per aggregazione. Nel caso del baseball in SOAR, le regole iniziali propongono due operatori di lancio senza determinare quale scegliere. Questa situazione è un impasse: la conoscenza disponibile non basta a decidere. L'agente cerca allora un'informazione ulteriore nella memoria episodica.

Il precedente recuperato riguarda un'altra partita: il battitore era Parra, era stato scelto un lancio veloce e il risultato era stato un fuoricampo. L'episodio diventa utile perché collega proprio uno degli operatori candidati a un esito sfavorevole contro lo stesso battitore. Il risultato dell'elaborazione è una nuova regola: quando sono proposti due operatori, uno è il lancio veloce e il battitore è Parra, quell'operatore viene scartato.

Il passaggio dalla soluzione del sottoproblema alla costruzione di una regola procedurale prende il nome di chunking. Il ricordo resta un evento specifico; la regola appresa consente invece di intervenire direttamente sulla selezione degli operatori quando si ripresentano le condizioni pertinenti. Registrare episodi e apprendere regole sono perciò attività diverse: l'apprendimento episodico passivo conserva esperienze senza generalizzarle automaticamente.

La distinzione può essere seguita anche attraverso le operazioni di CoALA: il recupero porta informazioni nella memoria di lavoro, il ragionamento le elabora e l'apprendimento scrive informazione nelle memorie a lungo termine. Queste categorie chiariscono il contributo di ciascun passaggio, ma non rendono identici i meccanismi delle due architetture.

## Quando il precedente giustifica la scelta

Una decisione sostenuta da casi è tanto più convincente quanto più è chiaro perché quei casi siano pertinenti. Un sistema di raccomandazione può spiegare di aver suggerito un film perché simile ad altri apprezzati dall'utente. Questa è una [[Argomentazione e spiegazione|giustificazione locale]] della raccomandazione: indica quali esperienze l'hanno sostenuta. Non spiega ancora tutte le decisioni del sistema né dimostra che le caratteristiche utilizzate per il confronto siano appropriate.

Il confronto con [[RAG]] chiarisce un'altra distinzione. Il RAG recupera documenti da fornire a un generatore, ma un documento può essere una definizione o una descrizione generale, senza rappresentare un caso già risolto. Il recupero documentale può sostenere una memoria di casi; la sola presenza del recupero non basta però a identificare un ragionamento per casi.

Rimangono infine limiti legati ai dati e al costo della ricerca. In alta dimensionalità, anche gli esempi più vicini possono essere troppo distanti per sostenere una previsione informativa. Casi rumorosi, poco numerosi o descritti mediante caratteristiche inadatte indeboliscono il trasferimento. Il valore dell'esperienza dipende quindi dalla possibilità di confrontare situazioni pertinenti e di controllare quali differenze contino per la decisione corrente.
