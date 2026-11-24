# Agenti autonomi interattivi

Capitolo: [[Percezione, interazione e robotica]]

Un agente interattivo riceve osservazioni, sceglie azioni e modifica l'ambiente o la comunicazione con altri soggetti. L'autonomia riguarda il modo in cui decide e si adatta entro le capacità e i vincoli disponibili. Non implica assenza di obiettivi definiti, supervisione o responsabilità del sistema.

## Azioni e feedback

Un agente linguistico può consultare documenti, chiamare strumenti, comunicare con utenti o operare su interfacce. I risultati ritornano come osservazioni che aggiornano il contesto. Il successo di una chiamata deve essere verificato dal feedback, non dedotto dal solo fatto che il comando sia stato prodotto.

Il ciclo permette di rivedere la strategia dopo errori o informazioni nuove. La memoria rende disponibili esperienze precedenti, ma può anche conservare dati scorretti se l'aggiornamento non viene controllato.

## Azioni interne ed esterne

[[CoALA]] distingue azioni di grounding, che coinvolgono l'ambiente, e azioni interne come retrieval, ragionamento e apprendimento. Recuperare un fatto modifica il contesto attivo; eseguire un comando su un dispositivo modifica un sistema esterno. La distinzione aiuta a localizzare gli effetti e i rischi.

Nel dominio fisico, segnali visivi, sonori o tattili possono essere trasformati in osservazioni utilizzabili da un agente linguistico; i comandi devono poi essere tradotti da componenti adatti all'esecuzione. Un comando testuale non realizza automaticamente un movimento affidabile.

## Coordinamento e controllo

Più agenti possono specializzarsi e cooperare tramite un'organizzazione centralizzata, distribuita o ibrida. La condivisione di un obiettivo non garantisce coordinamento: gli agenti possono produrre azioni incompatibili o non usare correttamente le informazioni reciproche.

La supervisione riguarda il passaggio da obiettivo a piano e azione. Osservabilità e valutazione del sistema complessivo sono necessarie per comprendere un errore, come discusso in [[Etica, fairness e accountability nell’IA]].
