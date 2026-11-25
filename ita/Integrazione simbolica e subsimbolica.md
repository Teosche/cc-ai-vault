# Integrazione simbolica e subsimbolica

Capitolo: [[Comprensione del linguaggio, chatbot e integrazione della conoscenza]]

L'integrazione simbolica e subsimbolica combina fatti e procedure espliciti con rappresentazioni apprese da dati. Un modello neurale può riconoscere formulazioni diverse o interpretare immagini; una componente simbolica può conservare relazioni, applicare regole o eseguire una query strutturata. Il problema è far cooperare queste capacità senza perdere il significato nel passaggio fra le rappresentazioni.

## Due modi di rendere disponibile un contenuto

Un embedding colloca un testo in uno spazio numerico in cui certe relazioni di similarità diventano utili. Una tripla come «Anna, appartiene a, laboratorio L» afferma invece una relazione precisa fra entità. Due frasi vicine nello spazio degli embedding non sono per questo logicamente equivalenti: «tutti sono ammessi» e «non tutti sono ammessi» possono condividere molto lessico ma esprimono condizioni differenti.

La struttura simbolica consente di controllare quali relazioni vengano usate. Non garantisce però che siano state acquisite correttamente. Il modello neurale affronta invece flessibilità e ambiguità, con rappresentazioni meno direttamente leggibili. L'integrazione deve assegnare a ciascun componente un ruolo verificabile.

## Un percorso completo dalla frase alla query

Consideriamo la domanda «chi dirige il laboratorio di Anna?». Un componente linguistico identifica Anna e la relazione di appartenenza. Una query recupera il laboratorio L e poi chi lo dirige. Il generatore trasforma il risultato «Marco» in una risposta discorsiva.

L'operazione strutturata evita di affidare il collegamento a una semplice somiglianza testuale. Restano tuttavia possibili errori prima e dopo la query: due persone possono chiamarsi Anna, il grafo può essere obsoleto e il generatore può aggiungere un incarico non presente. La correttezza della query non certifica l'intera catena.

## Procedure e memoria in un agente

In [[CoALA]], parte della conoscenza procedurale è implicita nei pesi e parte è esplicita nel codice. Il modello può proporre un'azione; il programma ne interpreta il formato, esegue la chiamata e riporta l'osservazione nella memoria di lavoro. Retrieval, ragionamento e aggiornamento della memoria diventano operazioni distinte e coordinabili.

Il sistema può così usare un modello per interpretare la richiesta e uno strumento per un conteggio esatto, anziché chiedere al solo generatore di svolgere tutte le funzioni. Anche l'aggiornamento di procedure esplicite è una forma di cambiamento persistente, ma richiede controllo perché modifica il comportamento futuro dell'agente.

## Ragionamento multimodale e LogiCAM

LogiCAM organizza la soluzione in estrazione delle premesse, identificazione del tipo di ragionamento, ragionamento e controllo finale. Le evidenze visive e il testo devono prima diventare premesse; il sistema deve poi distinguere quando applicare una deduzione e quando usare un'inferenza euristica o conoscenza comune.

Se un'immagine mostra tre oggetti e l'estrattore ne registra soltanto due, il ragionatore può applicare correttamente una regola a premesse incomplete e ottenere una risposta sbagliata. Analogamente, una conclusione credibile può spingere a saltare una condizione logica. Il controllo deve riguardare sia l'estrazione sia la validità dei passaggi, non soltanto la plausibilità dell'ultima frase.

Una soluzione ibrida è utile quando rende separabili questi punti di errore. La spiegazione deve poter indicare quali dati sono diventati premesse e quali operazioni hanno prodotto la conclusione, senza trattare la presenza di una componente simbolica come garanzia automatica.
