# Explainable AI e interpretabilità

Capitolo: [[Explainable AI, etica e bias]]

La Explainable AI cerca di rendere comprensibili risultati e comportamento dei sistemi artificiali. La domanda non è soltanto quale previsione sia stata prodotta, ma quali informazioni aiutino a capirla, valutarne i limiti o individuare un errore. La forma della spiegazione dipende dal destinatario e dall'oggetto da spiegare.

## Decisione singola e comportamento complessivo

Una spiegazione locale riguarda una previsione o una zona limitata dello spazio degli input. Una spiegazione globale cerca di descrivere regolarità del modello nel suo insieme. Un'analisi accurata di un caso non dimostra necessariamente come il sistema si comporterà in tutti gli altri; un riepilogo aggregato può nascondere differenze importanti fra casi.

Una [[Argomentazione e spiegazione|giustificazione]] deve rendere controllabili le ragioni della conclusione: la plausibilità del testo non dimostra che quelle ragioni siano corrette o abbiano determinato la risposta del sistema.

## Struttura interpretabile e analisi successiva

L'interpretabilità intrinseca riguarda modelli la cui struttura permette un'analisi più diretta. I metodi post-hoc interrogano invece un modello già addestrato. Sono scelte diverse, non categorie di verità: una tecnica post-hoc può essere utile anche per un modello semplice.

Si distingue inoltre fra metodi specifici per un modello e metodi che operano su più famiglie. Usare gradienti richiede accesso a proprietà interne differenti da quelle necessarie per analizzare soltanto input e output.

## Prestazione e spiegazione

Un'accuracy elevata non dice su quali caratteristiche si basi la previsione. Un classificatore può riconoscere correttamente un'ape e assegnare la stessa classe a un fiore, mostrando una dipendenza indesiderata dal contesto. Analizzare le caratteristiche utilizzate è quindi complementare alla misura delle prestazioni.

La complessità della rappresentazione può rendere difficile costruire spiegazioni semplici. Questo non autorizza però a trasformare ogni semplificazione in una descrizione fedele del calcolo: occorre dichiarare che cosa il metodo analizza e che cosa lascia fuori.

## Spiegabilità e controllo dell'agente

Per un agente, la spiegazione può riguardare anche il passaggio da obiettivo a piano e azione. Registrare osservazioni e risultati aiuta a localizzare il problema: una richiesta può essere stata interpretata male, uno strumento può aver restituito dati errati o il piano può non essere adatto al contesto.

Una spiegazione comprensibile non sostituisce tale controllo. Anche il testo prodotto dal sistema per giustificarsi deve essere valutato rispetto al comportamento effettivo. I [[Metodi di interpretabilità dei modelli|metodi di interpretabilità]] analizzano aspetti specifici della previsione, come la sensibilità alle caratteristiche o le regioni rilevanti di un'immagine.
