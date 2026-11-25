# Meta-learning

Capitolo: [[Paradigmi di apprendimento]]

Il meta-learning cerca di apprendere come adattarsi rapidamente a un nuovo compito. Supponiamo di dover riconoscere nuove categorie visive disponendo di poche immagini annotate per ciascuna. Addestrare da zero su quei pochi esempi favorirebbe l'overfitting; il meta-learning utilizza l'esperienza su molti compiti precedenti per costruire una buona base di adattamento.

## Apprendere attraverso compiti diversi

Durante il meta-training ogni compito contiene un piccolo insieme di supporto, usato per adattarsi, e un insieme di query, usato per verificare l'adattamento. Il sistema viene premiato se ciò che apprende dal supporto funziona su esempi nuovi dello stesso compito. L'unità dell'esperienza comprende quindi un problema di apprendimento, non soltanto un singolo esempio.

Come esempio didattico, un episodio può chiedere di distinguere tre specie avendo due immagini per specie nel supporto. Le query contengono altre immagini delle stesse specie. Un episodio successivo usa tre specie differenti. In questo modo il sistema deve sviluppare una capacità riutilizzabile, anziché memorizzare soltanto tre etichette fisse.

## Una buona inizializzazione: MAML

MAML apprende parametri iniziali che possano essere adattati con pochi passi di gradiente. Il ciclo interno parte da quei parametri e li modifica usando il supporto di un compito. Il ciclo esterno osserva l'errore sulle query dopo tale modifica e aggiorna l'inizializzazione condivisa. Si ottimizza quindi la qualità del modello dopo l'adattamento.

La distinzione rispetto a un normale fine-tuning riguarda soprattutto come viene preparata la base: nell'addestramento ordinario si cerca una buona prestazione prima dell'adattamento; qui si cerca una base da cui adattarsi bene a più compiti. MAML non esaurisce il meta-learning: si possono apprendere anche rappresentazioni per confrontare esempi o procedure di aggiornamento.

## Verificare l'adattamento

La valutazione richiede compiti tenuti fuori dal meta-training e query diverse dagli esempi usati per adattarsi. Si misura la qualità ottenuta con un numero dichiarato di esempi e aggiornamenti, confrontandola con il riuso di un modello preaddestrato. Se i nuovi compiti sono molto diversi da quelli di preparazione, il vantaggio può ridursi.

Il few-shot descrive la disponibilità di pochi esempi; il meta-learning descrive una strategia per sfruttare l'esperienza fra compiti. Anche il [[Prompting e in-context learning|few-shot prompting]] fornisce dimostrazioni, ma di norma le usa nel contesto senza i passi di aggiornamento dei pesi previsti dall'esempio MAML.
