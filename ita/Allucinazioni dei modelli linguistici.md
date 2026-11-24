# Allucinazioni dei modelli linguistici

Capitolo: [[Comprensione del linguaggio, chatbot e integrazione della conoscenza]]

Un modello linguistico può produrre affermazioni plausibili ma errate o prive di supporto. La fluidità dell'espressione e il tono sicuro non sono prove di correttezza. Il problema emerge quando il lettore interpreta la generazione come se fosse una consultazione infallibile di conoscenze verificate.

## Probabilità del testo e verità

L'obiettivo di previsione dei token favorisce continuazioni compatibili con il contesto di addestramento. Questo non introduce automaticamente una verifica esterna per ogni fatto generato. Un testo può quindi avere una forma convincente senza rappresentare correttamente dati, eventi o fonti.

Occorre distinguere un errore rispetto al mondo da un'affermazione non sostenuta dal contesto richiesto. Nei compiti ancorati a documenti, anche un'informazione vera può risultare impropria se il compito richiede di usare soltanto i passaggi forniti e non è possibile attribuirla a essi.

## Recuperare evidenze

Il [[RAG]] fornisce documenti pertinenti al generatore e può ridurre la dipendenza dalla sola conoscenza parametrica. La risposta viene costruita usando domanda e contesto recuperato. La possibilità di citare passaggi rende più agevole verificarne il supporto.

La riduzione degli errori non equivale alla loro eliminazione. Il recupero può selezionare documenti irrilevanti, incompleti o non aggiornati; la generazione può usare male dati corretti. Le due fasi devono essere controllate separatamente.

## Citazioni e attribuzione

Una citazione è utile se sostiene l'affermazione associata. La presenza di un riferimento non rende vera qualunque frase vicina: bisogna verificare ciò che il documento dice e la portata della conclusione. La copertura delle affermazioni e la precisione delle citazioni sono misure differenti.

Anche un giudizio espresso da un altro LLM può essere errato. Usarlo per valutare una risposta aggiunge una procedura di controllo, non un certificato di verità.

## Comportamento del sistema complessivo

Nei sistemi agentici, un errore può propagarsi a strumenti, memorie o decisioni successive. Conservare un'informazione scorretta può influenzare interazioni future; attribuire eccessiva autorità agli output può trasformare un errore testuale in un'azione inappropriata.

La progettazione deve quindi mantenere osservabili le informazioni utilizzate e i risultati delle azioni. L'[[Etica, fairness e accountability nell’IA|accountability]] richiede inoltre di poter ricostruire le responsabilità delle decisioni prese sulla base delle risposte, soprattutto quando un'affermazione non supportata produce conseguenze concrete.
