# Sfide dell’interazione multimodale

Capitolo: [[Percezione, interazione e robotica]]

Un sistema multimodale usa canali diversi, come testo, immagini e suoni, per costruire una rappresentazione o scegliere un'azione. Avere più informazioni può ridurre alcune ambiguità, ma richiede di stabilire quali elementi corrispondano, come combinarli e quale evidenza debba sostenere una conclusione.

## Allineare rappresentazioni diverse

CLIP addestra un encoder visivo e uno testuale a produrre vettori confrontabili. Una coppia immagine-descrizione viene avvicinata rispetto ad associazioni non corrispondenti. Si può così confrontare una fotografia con descrizioni candidate, anche per categorie espresse soltanto in parole.

L'allineamento non rende intercambiabili i canali. «Un cane nel parco» omette colore, posizione e molti dettagli della fotografia; l'immagine può non mostrare il nome del cane o il motivo per cui si trovi lì. La similarità utile al recupero non implica una corrispondenza completa di tutte le informazioni.

## Fondere per elaborare insieme

Le architetture possono mettere token visivi e testuali in un'elaborazione con attenzione congiunta, oppure proiettare le caratteristiche di un encoder visivo nello spazio accettato da un modello linguistico. Nel primo caso le rappresentazioni interagiscono durante una parte condivisa dell'elaborazione; nel secondo un collegamento appreso rende l'uscita visiva utilizzabile dal backbone linguistico.

La scelta del punto di fusione cambia dove le modalità possono influenzarsi. Un buon riconoscitore di oggetti non diventa automaticamente un buon risolutore di domande sulle relazioni: deve preservare anche numero, posizione e legami fra elementi. Le etichette di fusione vanno quindi collegate al percorso effettivo dei dati.

## Usare le evidenze anziché una scorciatoia

Se una domanda chiede se vi siano più oggetti rossi che blu, una risposta plausibile dal solo testo può trascurare un elemento dell'immagine. L'errore può nascere dal conteggio visivo, dall'associazione fra colori e oggetti o dal confronto finale. Separare questi passaggi rende il problema verificabile.

Nei sistemi neuro-simbolici come LogiCAM, l'estrazione di premesse cerca di rendere esplicito il contenuto da usare nel ragionamento. Se il dettaglio visivo viene perso in questa fase, la logica successiva non può recuperarlo. L'allineamento fra modalità deve quindi riguardare anche ciò che le premesse affermano, non soltanto la vicinanza dei vettori.

## Collegare i canali a una situazione che cambia

Nell'interazione, una parola come «questo» può riferirsi a un oggetto indicato in un certo momento. Testo, gesto e immagine devono descrivere la stessa situazione; se la scena cambia, una rappresentazione precedente può diventare inadeguata. Il ciclo percezione-azione deve mantenere riconoscibile quale osservazione sostenga il comando corrente.

Per un robot, le caratteristiche sensoriali devono infine diventare posizioni, obiettivi e operazioni eseguibili. La verifica comprende quindi coerenza della rappresentazione, correttezza della conclusione e risultato dell'azione. Nei casi ambigui, acquisire una nuova osservazione o chiedere una precisazione è parte della soluzione, non un fallimento della sola capacità linguistica.
