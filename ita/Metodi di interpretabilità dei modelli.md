# Metodi di interpretabilità dei modelli

Capitolo: [[Explainable AI, etica e bias]]

Un metodo di interpretabilità produce informazioni su un modello rispetto a una domanda definita. Può attribuire importanza a caratteristiche, evidenziare regioni, costruire visualizzazioni o sintetizzare regolarità. La scelta deve rispettare l'architettura e il livello dell'analisi.

## Gradienti e saturazione

Il gradiente misura la sensibilità locale del punteggio rispetto all'input. Un gradiente piccolo non implica necessariamente che una caratteristica sia irrilevante: in una regione di saturazione il punteggio può cambiare poco localmente pur dipendendo da informazione importante.

Gli Integrated Gradients confrontano l'input con una baseline e aggregano le derivate lungo un percorso fra i due. Per una caratteristica $i$, la forma continua è

$$
IG_i(x)=(x_i-x'_i)\int_0^1
\left.\frac{\partial F(z)}{\partial z_i}\right|_{z=x'+\alpha(x-x')}\,d\alpha.
$$

La formula esplicita il procedimento approssimato numericamente: $x'$ è il riferimento e $F$ il punteggio analizzato. Il gradiente è valutato nei punti del percorso, non calcolato rispetto al parametro di interpolazione. La scelta della baseline influenza l'interpretazione; la tecnica non elimina automaticamente tutte le interazioni fra caratteristiche o i limiti delle spiegazioni locali.

## CAM e Grad-CAM

CAM usa una specifica struttura con global average pooling sulle mappe convoluzionali e un livello finale di classificazione. Grad-CAM pesa invece le mappe mediante gradienti associati al punteggio della classe, consentendo di evidenziare regioni rilevanti senza richiedere esattamente la stessa architettura CAM.

La mappa dipende dalla classe, dal livello scelto e dalla rappresentazione interna. Non è una segmentazione garantita dell'oggetto e non dimostra che la regione evidenziata sia la causa del fenomeno reale.

## Attention rollout

Nei [[Vision Transformer e imaging medico|Vision Transformer]], il rollout combina le matrici di attenzione di più teste e livelli per seguire la propagazione delle informazioni. Nell'impostazione descritta, i pesi delle teste vengono aggregati e le relazioni fra livelli combinate. Il risultato è una visualizzazione del flusso di attenzione, non una prova completa del contributo causale di ogni pixel.

Il modello comprende anche trasformazioni diverse dall'attenzione: il rollout descrive quindi solo una parte dell'elaborazione e deve essere interpretato rispetto a questo limite.

## Visualizzazione e spiegazioni testuali

L'activation maximization modifica un input per aumentare l'attivazione di un modello. Le immagini così ottenute aiutano a esplorare quali configurazioni sostengano un punteggio. DiffExplainer e DEXTER impiegano componenti generative per rendere visibili e descrivere dipendenze del classificatore.

Una descrizione testuale può formulare un'ipotesi di bias, come l'uso dell'habitat invece dell'oggetto. Va però controllato che l'ipotesi sia sostenuta dagli esempi e non generalizzata oltre l'analisi svolta. Spiegare e certificare la correttezza sono obiettivi distinti.
