# Metodi di interpretabilità dei modelli

Capitolo: [[Explainable AI, etica e bias]]

I metodi di interpretabilità rendono osservabili aspetti differenti di un modello: sensibilità alle caratteristiche, regioni coinvolte in una classe, configurazioni che ne aumentano l'attivazione o concetti associati alle previsioni. Il punto di partenza è scegliere il punteggio da analizzare, l'input e il confronto che dia significato alla spiegazione.

## Gradienti e Integrated Gradients

Il gradiente di un punteggio rispetto all'input misura come il punteggio cambi per piccole variazioni delle caratteristiche. Nelle immagini, le derivate rispetto ai pixel possono produrre una mappa di sensibilità. Un limite è la saturazione: il punteggio può aver già raggiunto una zona quasi piatta, con gradienti piccoli anche se l'informazione presente è stata importante per raggiungerlo.

Integrated Gradients considera perciò il percorso da un riferimento $x'$ all'input $x$. Si costruiscono punti intermedi, si calcola il gradiente in ciascuno e si aggregano i risultati. L'attribuzione della caratteristica $i$ è:

$$
IG_i(x)=(x_i-x'_i)\int_0^1
\left.\frac{\partial F(z)}{\partial z_i}\right|_{z=x'+\alpha(x-x')}d\alpha.
$$

$F$ è il punteggio della classe; il percorso varia con $\alpha$ fra zero e uno. Nell'implementazione l'integrale viene approssimato tramite una media di gradienti campionati, poi moltiplicata per la differenza dall'input di riferimento. Si raccolgono così anche le variazioni attraversate prima della saturazione finale.

Come esempio lineare, se $F(x_1,x_2)=2x_1+3x_2$, il riferimento è $(0,0)$ e l'input $(1,2)$, i gradienti sono sempre 2 e 3. Le attribuzioni valgono 2 e 6 e sommano alla differenza di punteggio, 8. Nelle reti non lineari il calcolo è più complesso e il riferimento cambia l'interpretazione: un'immagine nera rappresenta un confronto specifico, non un'assenza universale di informazione. Il risultato è locale e non descrive separatamente tutte le interazioni fra caratteristiche.

## Attribuzioni senza leggere i gradienti

La permutation importance misura quanto peggiora una prestazione quando si mescolano fra esempi i valori di una caratteristica. Si interrompe così il suo collegamento con il target, lasciando invariato il modello. Se il punteggio peggiora molto, il modello si appoggia a quell'informazione nel dataset valutato. Con caratteristiche correlate, un'altra variabile può però sostituire quella mescolata, facendo apparire piccolo il contributo individuale.

I valori di Shapley, usati in SHAP, affrontano l'attribuzione considerando il contributo marginale di una caratteristica quando viene aggiunta a gruppi diversi di altre caratteristiche. Mediare questi contributi distribuisce fra variabili la differenza fra una previsione di riferimento e quella spiegata. La definizione di caratteristica assente e il trattamento delle dipendenze influenzano il risultato; inoltre, esaminare tutti i gruppi diventa rapidamente costoso. L'obiettivo è attribuire una previsione, mentre la permutation importance misura soprattutto la dipendenza della prestazione complessiva da una variabile.

## CAM e Grad-CAM

CAM richiede una struttura con mappe convoluzionali, global average pooling e classificatore lineare finale. Per una classe, combina le mappe con i pesi usati dal classificatore: si ottiene una localizzazione grossolana delle regioni associate al suo punteggio.

Grad-CAM sostituisce quei pesi con coefficienti ricavati dai gradienti del punteggio rispetto alle mappe di un livello scelto. Media i gradienti sulle posizioni spaziali, usa i coefficienti per pesare le mappe e applica una rettificazione per evidenziare il supporto positivo alla classe. Non richiede esattamente la struttura finale di CAM, purché siano disponibili mappe e gradienti appropriati.

Nella stessa immagine con cane e gatto, scegliere il punteggio «cane» o «gatto» può evidenziare regioni diverse. La mappa dipende anche dal livello, la cui risoluzione limita il dettaglio. Non va letta come una segmentazione esatta dell'oggetto.

## Attention rollout

Nei Transformer visivi, il rollout aggrega le matrici delle teste e combina quelle dei livelli per seguire i percorsi attraverso cui le patch contribuiscono alle rappresentazioni successive. Considerando il class token, si può riportare il risultato sulla griglia dell'immagine. La procedura deve dichiarare aggregazione e trattamento dei collegamenti residuali.

Questo mostra un flusso basato sull'attenzione, mentre il modello contiene anche feed-forward e altre trasformazioni. La mappa aiuta quindi a formulare un'ipotesi sulle regioni usate, da confrontare con perturbazioni e altre analisi.

## Generare esempi e descrivere concetti

L'activation maximization ottimizza un input per aumentare un'attivazione scelta. Rivela configurazioni gradite al modello, ma può produrre immagini poco naturali. DiffExplainer usa un generatore di diffusione per esplorare tali preferenze attraverso immagini più riconoscibili, collegando una rappresentazione generativa al classificatore da analizzare.

DEXTER porta l'ottimizzazione anche sul versante linguistico: un prompt appreso orienta la generazione di immagini che aumentano il punteggio del classificatore. La pipeline deve collegare la selezione di parole al percorso differenziabile; usa una rilassazione Gumbel-Softmax e una mappatura fra vocabolari BERT e CLIP. Le immagini vengono poi descritte e analizzate per formulare report. Se «ape» viene sostenuta da immagini di soli fiori, il report può proporre una dipendenza dall'habitat.

Quando i pesi non sono accessibili, occorre invece interrogare il comportamento dall'esterno. UNBOX organizza una ricerca iterativa di prompt, genera immagini, ne valuta i punteggi e usa i risultati per aggiornare la ricerca. OCCAM collega concetti a regioni e analizza la riduzione di confidenza quando vengono rimosse, aggregando spiegazioni locali in una struttura che collega concetti, esempi e classi. Anche qui il risultato dipende dalle immagini generate, dai concetti riconosciuti e dalle perturbazioni: le spiegazioni sono ipotesi controllabili sul modello, non certificazioni.
