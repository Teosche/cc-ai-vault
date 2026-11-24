
Capitolo: [[Tecnologie semantiche, Knowledge Graphs e ontologie]]

Interrogare un Knowledge Graph significa formulare una richiesta sulla struttura e sulle informazioni che contiene. Una domanda può riguardare un'entità, le sue proprietà, gli elementi a essa collegati oppure un percorso che attraversa più relazioni. Il linguaggio di interrogazione permette di tradurre questa richiesta in una forma elaborabile dal sistema.

## Dalla domanda al modello

In un grafo di clienti e prodotti, la domanda «quali clienti hanno acquistato questo prodotto?» richiede di identificare il prodotto e seguire le relazioni di acquisto. La domanda «quanti acquisti ha ricevuto?» usa collegamenti dello stesso tipo, ma richiede un conteggio. Il significato della relazione e la forma del risultato atteso devono quindi essere definiti separatamente.

Una richiesta più complessa può attraversare una gerarchia o cercare un collegamento fra entità non direttamente adiacenti. In questi casi non è sufficiente consultare una proprietà isolata: bisogna esaminare una parte della rete e interpretare il percorso trovato.

## Cypher e property graph

Cypher permette di interrogare i grafi. Il modello sottostante distingue nodi, etichette, tipi di relazione e proprietà. Prima di formulare una query occorre quindi sapere quali di questi elementi rappresentino la conoscenza cercata.

Nell'esempio di persone e città, chiedere dove viva una persona richiede il collegamento `LIVES_IN`. Chiedere da quando vi abiti richiede invece una proprietà di quel collegamento. La differenza è concettuale prima che sintattica: una data associata alla relazione non deve essere interpretata come un attributo della città.

Una query corretta rispetto alla struttura non è necessariamente corretta rispetto al significato. Se due archivi usano categorie differenti, l'interrogazione può richiedere le corrispondenze definite nelle [[Ontologie]]. Recuperare tutti i nodi con un nome simile non equivale a risolvere l'integrazione semantica.

## Operazioni e tipi di risultato

Le richieste possono includere filtri, ordinamenti e aggregazioni. Un filtro restringe i candidati secondo una condizione; un ordinamento li dispone secondo un criterio; un'aggregazione riassume più elementi, per esempio mediante un conteggio. Sono operazioni distinte dall'identificazione dei collegamenti da attraversare.

Questa distinzione è utile anche per i sistemi generativi. Una domanda che richiede conteggi o confronti fra entità può aver bisogno di un'elaborazione strutturata e non soltanto del recupero di testi simili. Un modello linguistico può aiutare a interpretare la domanda, ma il risultato deve rimanere coerente con il modello dei dati e con le operazioni effettivamente eseguite.
