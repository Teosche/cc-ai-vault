# Etica, fairness e accountability nell’IA

Capitolo: [[Explainable AI, etica e bias]]

Valutare eticamente un sistema significa considerare come le sue decisioni incidano sulle persone e quali responsabilità comportino. La qualità tecnica media è rilevante, ma non descrive da sola chi subisca gli errori, quali informazioni siano usate e se una persona possa ottenere una correzione. Il contesto d'uso trasforma una previsione numerica in una conseguenza concreta.

## Dalla rappresentazione alle opportunità

Un danno rappresentazionale riguarda il modo in cui persone o gruppi vengono descritti: stereotipi, associazioni svalutanti o scarsa visibilità possono essere riprodotti dai dati. Un danno allocativo riguarda invece la distribuzione di risorse e opportunità. Le due forme possono collegarsi: una rappresentazione distorta usata per selezionare candidati può influenzare chi riceve un'opportunità.

La fairness richiede quindi di definire quale differenza sia rilevante nel compito. Un'accuratezza complessiva elevata può nascondere molti più falsi negativi per un gruppo. Il confronto deve considerare numerosità, qualità delle etichette, distribuzione dei casi e costo degli errori. La stessa differenza numerica può assumere significati diversi secondo ciò che la decisione comporta.

Eliminare dall'input una caratteristica sensibile non elimina tutte le dipendenze: altre variabili possono conservarne associazioni. Anche una correzione deve essere valutata sul comportamento risultante, anziché dedurre l'equità dalla sola forma dell'input.

## Rendere la responsabilità esercitabile

Trasparenza significa rendere disponibili informazioni utili su presenza, capacità e limiti del sistema. Accountability riguarda la possibilità di attribuire responsabilità, ricostruire decisioni e intervenire quando si produce un errore. Una spiegazione comprensibile aiuta, ma occorrono anche ruoli e possibilità concrete di revisione.

In un agente, la registrazione di richiesta, dati consultati, strumenti invocati ed esiti permette di capire dove sia nato il problema. Se viene eseguito un rimborso errato, la responsabilità non può essere esaurita dalla frase «lo ha deciso il modello»: vanno esaminate configurazione, autorizzazioni e supervisione che hanno reso possibile l'azione.

## Autonomia proporzionata agli effetti

Un assistente che suggerisce una risposta e uno che modifica direttamente un conto esercitano poteri diversi. Il rischio di excessive agency emerge quando gli strumenti consentono più operazioni di quelle necessarie o manca un controllo adeguato sulle decisioni con conseguenze rilevanti. Definire confini operativi collega l'autonomia al compito assegnato.

La supervisione umana è efficace se chi interviene può vedere le evidenze, comprendere l'incertezza e modificare l'esito. Approvare rapidamente risposte senza informazioni o tempo di verifica può produrre overreliance: una fiducia eccessiva nell'automazione. La presenza formale di una persona non basta a rendere controllato il processo.

## Analizzare il sistema nel suo uso

I controlli devono includere cambiamenti dei dati, errori ripetuti e differenze fra gruppi, oltre alla prestazione iniziale. Una spiegazione del modello, un intervento sui bias e una procedura di contestazione affrontano aspetti complementari. L'obiettivo è collegare ciò che il sistema fa a conseguenze osservabili e possibilità effettive di correzione, come mostrano i [[Casi di studio su bias e rischi dell’IA]].
