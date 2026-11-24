# Sfide dell’interazione multimodale

Capitolo: [[Percezione, interazione e robotica]]

Un sistema multimodale utilizza informazioni di natura diversa, come testo e immagini, per rappresentare una situazione o scegliere una risposta. Il problema non è soltanto raccogliere più dati, ma stabilire come le informazioni si riferiscano fra loro e quale ruolo abbiano nel compito.

## Allineare e fondere

L'allineamento di CLIP rende confrontabili rappresentazioni visive e linguistiche in uno spazio comune. Altre architetture inseriscono caratteristiche visive nel contesto di un modello linguistico o calcolano attenzione congiunta. La modalità di fusione determina quali interazioni fra informazioni siano possibili.

Un'immagine e la sua descrizione possono fornire informazioni complementari senza essere intercambiabili. Il testo può omettere dettagli visivi; l'immagine può non rendere accessibili intenzioni o fatti esterni. Aggiungere una modalità non elimina automaticamente l'incertezza delle altre.

## Coerenza fra evidenze

Un modello può privilegiare una scorciatoia linguistica e trascurare dettagli visivi pertinenti. I problemi di allineamento logico fra modalità mostrano che riconoscere gli elementi e utilizzarli correttamente in un'inferenza sono passaggi differenti.

La verifica deve quindi seguire il collegamento fra osservazione, rappresentazione e conclusione. Una risposta plausibile non basta a dimostrare che tutte le evidenze rilevanti siano state considerate.

## Interazione ed esecuzione

Negli agenti che agiscono, il problema si estende al passaggio dai segnali ai comandi. Le percezioni fisiche possono essere rese disponibili al modello in forma testuale o vettoriale, ma l'esecuzione richiede componenti capaci di operare sull'ambiente e restituire feedback.

Nell'[[Interazione uomo-robot|interazione uomo-robot]], comprendere una richiesta deve tradursi in un'azione coerente con la situazione percepita. La valutazione deve quindi controllare sia l'interpretazione del messaggio sia gli effetti prodotti, oltre alla qualità del dialogo.
