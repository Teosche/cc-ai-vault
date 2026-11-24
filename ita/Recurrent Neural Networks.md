# Recurrent Neural Networks

Capitolo: [[Modelli neurali profondi]]

Le **Recurrent Neural Networks**, abbreviate **RNN**, sono reti neurali progettate per elaborare sequenze mantenendo uno **stato nascosto** che viene aggiornato a ogni passo. Un elemento viene quindi interpretato insieme all'informazione conservata dai passi precedenti. Questa struttura è utile per testo, segnali e serie temporali, dove l'ordine delle osservazioni può cambiare il significato del dato.

Per esempio, «il cane insegue il gatto» e «il gatto insegue il cane» contengono le stesse parole, ma descrivono situazioni diverse. Un modello che conservasse soltanto l'elenco delle parole perderebbe questa differenza. Una RNN costruisce invece la rappresentazione progressivamente, seguendo la sequenza.

## Come si aggiorna lo stato

Al passo $t$ la rete riceve il vettore di input $x_t$ e lo stato precedente $h_{t-1}$. Una RNN semplice può aggiornarlo secondo:

$$
h_t=\tanh(W_xx_t+W_hh_{t-1}+b).
$$

$W_x$ trasforma l'input, $W_h$ trasforma lo stato precedente e $b$ è un termine di bias. Sono parametri appresi. La funzione non lineare $\tanh$ agisce componente per componente e produce valori fra −1 e 1. Lo stato iniziale $h_0$ viene spesso posto a zero. Se l'input è testo, i vettori $x_t$ possono essere [[Metodi di embedding|embedding]] dei token.

**Gli stessi parametri vengono riutilizzati a ogni passo.** Non si costruisce una rete indipendente per ciascuna posizione: è lo stato a cambiare mentre la stessa trasformazione viene applicata ai nuovi input. Rappresentare questi passaggi uno accanto all'altro è detto *srotolamento nel tempo*. Una RNN può anche essere profonda, impilando più strati ricorrenti; profondità degli strati e numero di passi temporali sono due aspetti diversi.

Lo stato corrente può riassumere la storia precedente, ma non è un archivio che conserva ogni elemento in forma esplicita. Quali informazioni sopravvivano dipende dai parametri e dal compito di addestramento. È quindi diverso dalla memoria simbolica o episodica di un'architettura come [[SOAR Cognitive Architecture]].

## Un esempio dell'effetto dell'ordine

Consideriamo una versione scalare, con un solo numero come stato, definita da $h_t=\tanh(x_t+0{,}5h_{t-1})$ e $h_0=0$. Per la sequenza di input $(1,0)$ si ottiene:

$$
h_1=\tanh(1)\approx0{,}762,\qquad
h_2=\tanh(0{,}381)\approx0{,}363.
$$

Il secondo stato resta diverso da zero anche se il secondo input è zero: contiene un effetto dell'input precedente. Se invertiamo la sequenza e usiamo $(0,1)$, otteniamo prima $h_1=0$ e poi $h_2\approx0{,}762$. Gli stessi valori, presentati in un ordine diverso, portano quindi a stati finali diversi. È un esempio numerico del meccanismo, non una rete già addestrata a comprendere frasi.

## Dallo stato alla risposta

Il modo di utilizzare lo stato dipende dal compito. Per classificare un'intera frase, si può trasformare lo stato finale in una previsione, per esempio positiva o negativa. Per assegnare un'etichetta a ogni elemento, si produce invece un'uscita a ogni passo. La ricorrenza rimane la stessa; cambia il punto in cui si richiede una risposta.

In un modello linguistico, lo stato dopo i token osservati può essere trasformato in punteggi sui possibili token successivi. Una funzione **softmax** converte questi punteggi in probabilità che sommano a uno. La rete viene addestrata a dare alta probabilità al token che segue davvero nel testo; durante la generazione, un token scelto dalla distribuzione viene reinserito come nuovo input e lo stato viene aggiornato.

I parametri memorizzano regolarità apprese attraverso molti esempi, mentre lo stato evolve durante l'elaborazione della singola sequenza. Continuare una frase aggiornando lo stato non equivale, di per sé, a riaddestrare i pesi.

## Addestramento attraverso il tempo

Per addestrare una RNN si calcolano le perdite delle uscite richieste e si propaga il gradiente attraverso la rete srotolata. Questo procedimento è la **backpropagation through time**, o **BPTT**. Poiché gli stessi parametri compaiono in più passi, il loro gradiente raccoglie contributi dalle diverse posizioni.

Una previsione finale sbagliata può dipendere da come è stato elaborato un elemento molto precedente. Per attribuirgli parte dell'errore, il gradiente deve attraversare molti aggiornamenti ricorrenti. Le ripetute moltiplicazioni possono farlo diventare molto piccolo, causando **vanishing gradient**, oppure molto grande, causando **exploding gradient**. Nel primo caso diventa difficile apprendere dipendenze lontane; nel secondo gli aggiornamenti possono diventare instabili.

Il **gradient clipping** limita la norma del gradiente quando è eccessiva, riducendo il problema dell'esplosione. Non risolve da solo la scomparsa del gradiente. Con sequenze lunghe si usa anche la BPTT troncata: si propaga il gradiente soltanto attraverso una finestra di passi. Questo limita memoria e costo del calcolo, ma limita anche quanto lontano venga attribuito direttamente l'errore, pur potendo trasportare lo stato fra finestre successive.

## LSTM: controllare che cosa conservare

Le **Long Short-Term Memory**, o **LSTM**, sono reti ricorrenti che aggiungono uno stato di cella $c_t$ e meccanismi appresi per controllarne gli aggiornamenti. Questi meccanismi sono detti **gate**: producono valori fra 0 e 1, con cui modulano componente per componente il passaggio delle informazioni.

Il *forget gate* $f_t$ regola quanto conservare dello stato di cella precedente; l'*input gate* $i_t$ regola quanta nuova informazione candidata $\tilde{c}_t$ aggiungere; l'*output gate* $o_t$ regola quanto dello stato di cella usare per costruire lo stato nascosto. L'aggiornamento centrale è:

$$
c_t=f_t\odot c_{t-1}+i_t\odot\tilde{c}_t,\qquad
h_t=o_t\odot\tanh(c_t).
$$

$\odot$ indica il prodotto componente per componente. I gate dipendono dall'input corrente e dallo stato nascosto precedente attraverso trasformazioni apprese; una funzione sigmoid ne porta i valori fra 0 e 1. Il candidato è calcolato tramite una trasformazione non lineare degli stessi ingressi.

Se una componente di $f_t$ è vicina a uno e la corrispondente componente di $i_t$ vicina a zero, la memoria precedente viene quasi conservata. Se occorre cambiare contesto, la rete può ridurre il contributo passato e inserire nuova informazione. Il percorso additivo nello stato di cella facilita il mantenimento dell'informazione e del gradiente lungo molti passi, senza garantire una memoria illimitata o l'eliminazione di ogni difficoltà di addestramento.

## GRU: una memoria con meno componenti

Le **Gated Recurrent Units**, o **GRU**, mantengono il controllo tramite gate ma non separano uno stato di cella da quello nascosto. Il *reset gate* regola quanto dello stato precedente usare nel calcolo del candidato $\tilde{h}_t$; l'*update gate* combina poi quel candidato con lo stato precedente. In una convenzione comune:

$$
h_t=u_t\odot h_{t-1}+(1-u_t)\odot\tilde{h}_t.
$$

Con questa convenzione, valori di $u_t$ vicini a uno favoriscono la conservazione dello stato precedente; valori vicini a zero favoriscono il candidato. Il controllo è appreso dai dati. A parità di dimensione dello stato, una GRU standard usa meno parametri di una LSTM standard, ma quale delle due funzioni meglio dipende dal problema.

## Encoder-decoder, attenzione e limiti

Una RNN può essere usata come encoder per rappresentare una sequenza e un'altra come decoder per generarne una seconda, per esempio una traduzione. Nella forma più semplice, l'encoder condensa l'intera frase in un solo vettore: questo crea un collo di bottiglia quando la frase è lunga. Un meccanismo di **attenzione** permette al decoder di combinare gli stati delle diverse posizioni dell'input, scegliendo a ogni passo quali informazioni usare.

L'attenzione può dunque essere aggiunta a una rete ricorrente. Il [[Architettura Transformer e meccanismo di attenzione|Transformer]] originario costruisce invece il contesto senza questa ricorrenza, favorendo l'elaborazione parallela delle posizioni durante l'addestramento. La generazione autoregressiva continua comunque a produrre i token in sequenza.

Le RNN permettono di aggiornare uno stato man mano che arrivano osservazioni e di trattare sequenze di lunghezza variabile. Restano però la dipendenza fra passi successivi, che limita la parallelizzazione lungo il tempo, e la difficoltà di usare informazioni molto lontane. LSTM e GRU attenuano questi problemi; non trasformano lo stato interno in una conservazione completa e garantita del passato.
