# Allucinazioni dei modelli linguistici

Capitolo: [[Comprensione del linguaggio, chatbot e integrazione della conoscenza]]

Un'allucinazione è un contenuto generato che appare plausibile ma risulta errato, contraddice le evidenze oppure non è sostenuto dal materiale su cui la risposta dovrebbe basarsi. Un modello può inventare un nome, attribuire a un documento un'affermazione assente o aggiungere una spiegazione non documentata. La forma linguistica corretta può rendere questi errori difficili da riconoscere.

## Perché la plausibilità non basta

L'addestramento alla previsione dei token favorisce continuazioni compatibili con il testo osservato. Una frase ben costruita con un nome e una data può quindi essere probabile nella forma senza che quel nome e quella data siano corretti. La generazione non include automaticamente, per ogni affermazione, una consultazione di dati verificati.

Questo non significa che il modello non possa rappresentare fatti o rispondere correttamente. Significa che **il meccanismo che produce una continuazione non costituisce da solo una prova della sua verità**. Anche il tono sicuro è una proprietà del testo, non una misura affidabile della conoscenza disponibile.

## Errori fattuali e infedeltà al contesto

Supponiamo che un documento riporti un ricavo di 10 milioni e che la risposta ne dichiari 12: l'affermazione contraddice il contesto. Se invece il modello aggiunge «l'aumento dipende dall'espansione internazionale» quando il documento non indica cause, *introduce un dettaglio senza supporto*. Quest'ultimo potrebbe anche essere vero nel mondo, ma non è giustificato se il compito richiede di usare soltanto quel documento.

La distinzione chiarisce che cosa controllare. La correttezza fattuale riguarda la corrispondenza al mondo; la fedeltà riguarda il rapporto con le evidenze ammesse. Un riassunto può fallire la fedeltà aggiungendo informazioni plausibili, anche senza inventare una data o un numero.

## Come il recupero può aiutare e fallire

Il [[RAG]] rende disponibili documenti pertinenti, riducendo la necessità di ricostruire tutto dai pesi. Se il problema è la mancanza di un'informazione specifica, recuperare il passaggio corretto può risolverlo. Se però il retriever seleziona una vecchia versione o perde una condizione importante durante il chunking, il generatore riceve una base insufficiente.

Anche con dati corretti, la generazione può confondere soggetti, unire numeri di periodi diversi o aggiungere conseguenze non dichiarate. Occorre quindi separare *errore di recupero, errore di interpretazione ed espansione non supportata*. Limitare la risposta alle evidenze e prevedere la possibilità di dichiarare un'informazione mancante riduce la pressione a completare ogni dettaglio.

## Verificare affermazioni e riferimenti

Una citazione sostiene una frase soltanto se il passaggio contiene l'informazione necessaria, con le stesse condizioni e la stessa portata. Un riferimento a una pagina sui ricavi non dimostra una spiegazione delle loro cause. La verifica richiede di scomporre la risposta in affermazioni controllabili e confrontarle con i passaggi pertinenti.

Un altro LLM può aiutare a svolgere questo controllo, ma può ripetere lo stesso errore o approvare un'inferenza ingiustificata. Nei sistemi agentici, un fatto non verificato può inoltre essere scritto in memoria e riutilizzato più avanti, oppure guidare uno strumento. ***Rendere ricostruibili osservazioni e azioni serve a impedire che la fluidità di una risposta nasconda l'origine dell'errore***.
