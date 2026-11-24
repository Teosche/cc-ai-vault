# Knowledge Graphs

Capitolo: [[Tecnologie semantiche, Knowledge Graphs e ontologie]]

Un Knowledge Graph organizza informazioni su un dominio attraverso entità e relazioni, fornendo il contesto necessario per interpretarle. I nodi rappresentano elementi del dominio e gli archi ne esprimono i collegamenti. Il carattere di conoscenza dipende dal significato attribuito a questa struttura, non dalla sola presenza di un grafo.

## Nodi, etichette e proprietà

Nel Labeled Property Graph, un nodo può avere etichette e proprietà. Un'etichetta ne indica una categoria, come persona o luogo; una proprietà è una coppia chiave-valore, come nome o età. La stessa struttura consente quindi di distinguere il ruolo generale dell'entità dai dati che la descrivono.

Una relazione collega un nodo iniziale a un nodo finale, ha un tipo e può avere proprietà proprie. Nell'esempio di persone e luoghi, `LIVES_IN` collega una persona alla città in cui vive; una proprietà della relazione può indicare da quando. La data riguarda quel rapporto, non necessariamente la persona o la città considerate separatamente.

La direzione dell'arco fa parte della rappresentazione. Non implica da sola che il rapporto reale sia asimmetrico: l'interpretazione di un legame come amicizia dipende dalle convenzioni adottate nel modello. Anche la possibilità di applicare vincoli deve essere distinta dalla flessibilità generale del property graph: uno schema flessibile non obbliga a rinunciare a ogni controllo.

## Dalla struttura alla conoscenza

Un grafo clienti-prodotti può permettere di contare gli acquisti, ma una semplice etichetta non stabilisce che un prodotto possa sostituirne un altro. Per rispondere a questa seconda domanda servono categorie e relazioni più ricche. Le [[Ontologie]] e le tassonomie forniscono un'organizzazione aggiuntiva rispetto alla sola memorizzazione di nodi e archi.

Un Knowledge Graph non va quindi identificato automaticamente con un database a grafo, né ogni sua implementazione deve coincidere con il particolare modello dei property graph. Il database riguarda il modo in cui la struttura viene memorizzata e interrogata; la rappresentazione della conoscenza riguarda ciò che entità e relazioni significano.

## Interrogazioni attraverso le relazioni

I database a grafo sono particolarmente pertinenti quando le domande riguardano percorsi e connessioni: come siano collegate due persone, quali livelli compongano una gerarchia o quali parti appartengano indirettamente a un prodotto. Le relazioni di profondità variabile rendono naturale ragionare in termini di attraversamenti.

Neo4j memorizza dati come nodi, etichette, relazioni e proprietà e utilizza il modello Labeled Property Graph. I [[Linguaggi di interrogazione dei Knowledge Graphs|linguaggi di interrogazione]] permettono di specificare quali nodi e relazioni cercare e quali proprietà, conteggi o aggregazioni restituire.

Questo non rende ogni interrogazione automaticamente economica. Il lavoro necessario dipende anche da quanta struttura viene esplorata e da quali condizioni devono essere verificate. Il vantaggio degli attraversamenti non va trasformato nell'affermazione generale che qualsiasi query abbia costo costante, indipendente dai dati.

## Collegamento con recupero e generazione

Un grafo può fornire a un modello generativo informazioni connesse: non soltanto passaggi testuali affini alla domanda, ma entità collegate e percorsi che sostengono un confronto. Questo è utile quando la risposta richiede informazioni distribuite fra più elementi o operazioni strutturate, come filtri e aggregazioni.

Le [[Metriche di similarità]] restano utili per cercare punti di partenza pertinenti, mentre il grafo può espandere il contesto attraverso relazioni esplicite. Sono funzioni complementari. La qualità della risposta rimane legata alla correttezza delle informazioni e al modo in cui vengono utilizzate, non alla sola scelta della struttura a grafo.
