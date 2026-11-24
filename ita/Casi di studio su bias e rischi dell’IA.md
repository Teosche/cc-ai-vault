# Casi di studio su bias e rischi dell’IA

Capitolo: [[Explainable AI, etica e bias]]

I casi di studio permettono di distinguere errori di rappresentazione, ragionamento e controllo. Un risultato corretto in una singola prova non esclude dipendenze indesiderate; una capacità elevata non impedisce un uso inappropriato se il sistema riceve accessi o responsabilità eccessivi.

## Ape, fiore e caratteristiche spurie

Un classificatore può associare la classe «ape» a un fiore anche quando l'ape non è presente. Le visualizzazioni generative mostrano inoltre esempi nei quali neve, alberi o altri elementi di contesto sostengono una categoria. Il problema è capire quali regolarità il modello stia effettivamente utilizzando, anziché inferirlo dal solo nome della classe.

Questo non autorizza a concludere che ogni modello per quella categoria abbia lo stesso difetto. L'analisi riguarda un sistema, esempi e procedura specifici.

## Report DEXTER e rappresentazione dei gruppi

DEXTER combina generazione, classificazione e descrizione testuale per formulare ipotesi sul comportamento di classificatori visivi. Negli esempi di previsione dell'età, un report segnala una possibile sovrarappresentazione maschile e un altro descrive un comportamento differente su un caso bilanciato.

Il report è un elemento di analisi, non una certificazione generale di equità. Un confronto illustrativo fra campioni non documenta da solo tutte le condizioni sperimentali, la significatività statistica o gli effetti su ogni gruppo.

## Table QA e verifica delle evidenze

Una domanda può chiedere quante squadre abbiano ottenuto un certo punteggio contro un'avversaria. Contare valori uguali nella tabella non basta se alcune righe descrivono ruoli differenti. L'esempio mostra come una formulazione linguistica o un conteggio superficiale possano prevalere sulla relazione effettivamente richiesta.

La strategia di debiasing genera varianti equivalenti della domanda e aggrega le evidenze per ridurre la dipendenza da una singola formulazione. L'[[Inferenza causale|interpretazione causale]] del procedimento richiede comunque assunzioni ulteriori: la concordanza delle risposte non dimostra da sola l'eliminazione del confondimento.

## Rimborsi, memoria e strumenti

Un assistente con autonomia eccessiva può effettuare rimborsi o modifiche non autorizzate. Il rischio non riguarda soltanto la verità della frase generata, ma il potere operativo che le è associato.

Una memoria può inoltre essere alterata con informazioni ingannevoli; uno strumento può essere invocato in un contesto non appropriato. Questi casi richiedono di controllare confini dell'azione, origine dei dati e possibilità di ricostruire gli eventi. Nessun particolare prodotto di sicurezza viene assunto come garanzia universale di prevenzione.
