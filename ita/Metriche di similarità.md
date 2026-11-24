# Metriche di similarità

Capitolo: [[Problem-solving, ricerca e similarità]]

Confrontare elementi in base alla somiglianza richiede di scegliere quali caratteristiche rappresentarli e come misurarne lo scostamento. **Due elementi non sono simili in assoluto**: possono esserlo rispetto a una proprietà e differire rispetto a un'altra. **Una misura utile deve riflettere ciò che conta per il compito**, come accade nel [[Ragionamento basato su casi]].

## Distanza e rappresentazione

Quando gli oggetti sono vettori numerici, ogni coordinata rappresenta una caratteristica. Una distanza assegna un valore allo scostamento fra due vettori; valori piccoli indicano vicinanza rispetto alla rappresentazione scelta.

Una **similarità** adotta il verso opposto: valori più grandi indicano maggiore somiglianza. Una distanza pari a zero indica oggetti coincidenti rispetto alla rappresentazione e alla misura usate. **Non tutte le distanze sono comprese fra 0 e 1**: alcune sono normalizzate, altre possono crescere senza un limite superiore. Inoltre, una similarità di $0{,}8$ non significa automaticamente «80% di probabilità che i due oggetti abbiano lo stesso significato».

Una distanza è una **metrica matematica** se è non negativa, vale zero se e solo se gli oggetti sono uguali, è simmetrica e rispetta la disuguaglianza triangolare:

$$
d(x,z)\leq d(x,y)+d(y,z).
$$

Quest'ultima proprietà dice che passare attraverso un terzo punto non può rendere il percorso più corto della distanza diretta. Alcune misure chiamate «distanze», come $1-\text{similarità del coseno}$, non soddisfano tutte queste condizioni.

Anche le scale contano. Se una coordinata assume valori molto più grandi delle altre, può dominare il confronto numerico. La standardizzazione trasforma una caratteristica sottraendo la media e dividendo per la deviazione standard:

$$
z_j=\frac{x_j-\mu_j}{\sigma_j},\qquad \sigma_j>0.
$$

La trasformazione esprime lo scostamento dalla media in unità di deviazione standard. Per esempio, confrontando persone mediante altezza e reddito, i valori del reddito in euro possono dominare numericamente quelli dell'altezza in metri. Standardizzare rende le scale più confrontabili, ma non stabilisce quali caratteristiche siano semanticamente rilevanti: correggere le scale e scegliere le proprietà utili sono due decisioni distinte.

## Minkowski, Euclidea e Manhattan

La **distanza di Minkowski** è una famiglia di distanze per vettori reali $x,y$ con lo stesso numero di coordinate:

$$
d_p(x,y)=\left(\sum_j|x_j-y_j|^p\right)^{1/p},\qquad p\geq1.
$$

L'indice $j$ scorre le coordinate; $|x_j-y_j|$ è lo scarto assoluto sulla caratteristica $j$. Si elevano gli scarti alla potenza $p$, si sommano e si applica la radice di ordine $p$. Il parametro $p$ determina come vengono aggregati gli scarti: aumentando $p$, gli scarti maggiori acquistano un peso relativo più forte.

### Distanza Manhattan: sommare gli scarti

Con $p=1$ si ottiene la **distanza Manhattan**, o $L_1$:

$$
d_1(x,y)=\sum_j|x_j-y_j|.
$$

Per $x=(1,2)$ e $y=(4,6)$, lo scarto sulla prima coordinata è $|1-4|=3$; sulla seconda è $|2-6|=4$. Pertanto:

$$
d_1(x,y)=3+4=7.
$$

L'immagine intuitiva è uno spostamento lungo una griglia: si percorrono tre unità in una direzione e quattro nell'altra. Ogni scarto contribuisce linearmente alla somma.

### Distanza euclidea: la linea retta

Con $p=2$ si ottiene la **distanza euclidea**, o $L_2$:

$$
d_2(x,y)=\sqrt{\sum_j(x_j-y_j)^2}.
$$

Sugli stessi punti si applica il teorema di Pitagora:

$$
d_2(x,y)=\sqrt{3^2+4^2}=\sqrt{9+16}=\sqrt{25}=5.
$$

È la lunghezza del segmento che collega i due punti. Per capire la differenza fra le due misure, confrontiamo anche gli scarti $(4,0)$ e $(2,2)$: Manhattan assegna a entrambi distanza 4, mentre l'euclidea dà rispettivamente $4$ e $\sqrt8\approx2{,}83$. A parità di somma degli scarti assoluti, l'euclidea attribuisce quindi un valore maggiore allo scarto concentrato su una sola coordinata.

**I valori 5 e 7 non sono in contraddizione**: rispondono a definizioni differenti. Entrambe le misure valgono zero per vettori identici e, senza ulteriori vincoli sui dati, non hanno un limite superiore pari a 1.

## Jaccard per insiemi di caratteristiche

Quando un oggetto è descritto come un **insieme di caratteristiche presenti**, l'indice di Jaccard confronta quelle condivise con tutte quelle presenti almeno una volta. Per esempio, un documento può essere rappresentato come l'insieme delle parole distinte che contiene: in questa rappresentazione ordine e ripetizioni non vengono conservati.

Per due insiemi finiti $A$ e $B$:

- $A\cap B$, l'**intersezione**, contiene gli elementi presenti in entrambi;
- $A\cup B$, l'**unione**, contiene gli elementi presenti in almeno uno, contando una sola volta quelli condivisi;
- $|A|$ indica il numero di elementi dell'insieme $A$, detto **cardinalità**.

La **similarità di Jaccard** è:

$$
J(A,B)=\frac{|A\cap B|}{|A\cup B|},\qquad |A\cup B|>0.
$$

Il rapporto risponde alla domanda: **fra tutte le caratteristiche presenti, quale frazione è condivisa?** La **distanza di Jaccard** considera la frazione complementare, quella non condivisa:

$$
d_J(A,B)=1-J(A,B)
=\frac{|A\cup B|-|A\cap B|}{|A\cup B|}.
$$

### Perché nell'esempio la distanza è $2/3$

Con $A=\{a,b\}$ e $B=\{b,c\}$ si ha:

$$
A\cap B=\{b\},\qquad A\cup B=\{a,b,c\}.
$$

L'intersezione contiene un elemento e l'unione ne contiene tre. Quindi:

$$
J(A,B)=\frac{1}{3}.
$$

Per ottenere la distanza si sottrae la similarità da 1, scrivendo 1 come $3/3$:

$$
d_J(A,B)=1-\frac13=\frac33-\frac13=\frac23.
$$

Il risultato si legge anche direttamente: dei tre elementi dell'unione, $b$ è condiviso, mentre $a$ e $c$ appartengono ciascuno a un solo insieme. **Due elementi su tre non sono condivisi**, perciò la distanza è $2/3$.

Formalmente, gli elementi presenti in un solo insieme costituiscono la **differenza simmetrica** $A\triangle B=(A\setminus B)\cup(B\setminus A)$. Nell'esempio è $\{a,c\}$; perciò si può anche scrivere $d_J(A,B)=|A\triangle B|/|A\cup B|$.

### Come interpretare i valori

| Situazione | Similarità $J$ | Distanza $d_J$ |
| :--- | :--- | :--- |
| Insiemi identici e non vuoti | $1$ | $0$ |
| Alcuni elementi condivisi | Fra $0$ e $1$ | Fra $0$ e $1$ |
| Nessun elemento condiviso, unione non vuota | $0$ | $1$ |

Jaccard **non premia l'assenza comune**: se due documenti non contengono entrambi una parola, questa assenza non aumenta la similarità. Questo è utile quando interessa ciò che gli oggetti possiedono in comune e l'universo delle caratteristiche assenti è molto ampio.

Se uno solo degli insiemi è vuoto, la similarità è 0 e la distanza è 1. Se entrambi sono vuoti, il rapporto scritto diventa $0/0$ e richiede una convenzione: una scelta usuale è $J(\varnothing,\varnothing)=1$ e $d_J(\varnothing,\varnothing)=0$, trattandoli come identici.

La relazione **«distanza = 1 − similarità» è una definizione specifica di questa coppia di misure**, non una regola universale. Non avrebbe senso applicarla direttamente, per esempio, a una distanza euclidea di 5 per ricavare una similarità di $-4$ senza definire e giustificare una trasformazione appropriata.

## Similarità del coseno

La **similarità del coseno** confronta la direzione di due vettori non nulli. È il coseno dell'angolo $\theta$ fra essi:

$$
s_{\cos}(x,y)=\cos\theta
=\frac{x^\top y}{\|x\|_2\|y\|_2}
=\frac{\sum_j x_jy_j}{\sqrt{\sum_j x_j^2}\sqrt{\sum_j y_j^2}}.
$$

Il numeratore è il **prodotto scalare**: si moltiplicano le coordinate corrispondenti e si sommano i risultati. Al denominatore compaiono le **norme euclidee**, cioè le lunghezze dei vettori. Dividere per queste lunghezze permette di confrontare l'orientamento senza far dipendere il risultato dalla sola grandezza dei vettori.

### Esempio con conteggi di parole

Riprendendo l'esempio delle slide, due documenti sono rappresentati dai conteggi delle stesse tre parole: $x=(7,3,2)$ e $y=(2,3,0)$. Si calcolano:

$$
x^\top y=7\cdot2+3\cdot3+2\cdot0=23,
$$

$$
\|x\|_2=\sqrt{49+9+4}=\sqrt{62},\qquad
\|y\|_2=\sqrt{4+9+0}=\sqrt{13}.
$$

Quindi la similarità è:

$$
s_{\cos}(x,y)=\frac{23}{\sqrt{62}\sqrt{13}}\approx0{,}810.
$$

Definendo la **cosine distance** come $d_{\cos}=1-s_{\cos}$, si ottiene:

$$
d_{\cos}(x,y)\approx1-0{,}810=0{,}190.
$$

I documenti presentano un orientamento abbastanza vicino nello spazio dei conteggi. Il valore non esprime però una probabilità di equivalenza fra i testi.

### Direzione, lunghezza e intervallo dei valori

Se $y=2x$, la direzione è identica e la similarità del coseno vale 1, anche se i vettori hanno lunghezze differenti e la loro distanza euclidea è positiva. Per esempio, $(1,2)$ e $(2,4)$ hanno similarità del coseno 1 e distanza euclidea $\sqrt5$. Questo chiarisce perché le due misure possono dare giudizi differenti sullo stesso confronto.

Per vettori reali generici, $s_{\cos}$ varia fra $-1$ e $1$:

- $1$: stessa direzione;
- $0$: vettori ortogonali, cioè angolo di $90^\circ$;
- $-1$: direzioni opposte.

La distanza $1-s_{\cos}$ varia quindi fra 0 e 2. Quando tutte le coordinate sono non negative, come nei conteggi di parole, la similarità varia invece fra 0 e 1 e anche la distanza è compresa fra 0 e 1. Se uno dei vettori è nullo, il denominatore è zero: la formula matematica non è definita e occorre gestire separatamente il caso.

La cosine distance **non è in generale una metrica**: può violare la disuguaglianza triangolare. Come verifica matematica, con $u=(1,0)$, $v=(1/\sqrt2,1/\sqrt2)$ e $w=(0,1)$ si ottiene $d(u,w)=1$, mentre $d(u,v)+d(v,w)=2-\sqrt2<1$. Inoltre, vettori distinti ma con la stessa direzione hanno cosine distance zero. Chiamarla distanza operativa non autorizza quindi ad attribuirle tutte le proprietà delle distanze di Minkowski.

Nel recupero semantico, query e documenti vengono rappresentati in uno spazio di [[Metodi di embedding|embedding]] e confrontati per ottenere candidati pertinenti. Una forte similarità non garantisce tuttavia che un documento risponda alla domanda: la rappresentazione deve essere adatta al compito e può essere necessario scartare risultati poco pertinenti.

## Distanza di Levenshtein per stringhe

La **distanza di Levenshtein** misura il numero **minimo** di modifiche necessarie per trasformare una stringa nell'altra. Nella versione standard ogni operazione costa 1 e può essere un inserimento, una cancellazione oppure una sostituzione di un carattere.

Per esempio:

- `casa` → `cassa`: si inserisce una `s`, quindi la distanza è 1;
- `cassa` → `casa`: si cancella una `s`, quindi la distanza è 1;
- `casa` → `cosa`: si sostituisce `a` con `o`, quindi la distanza è 1;
- `casa` → `cane`: si sostituiscono `s` con `n` e l'ultima `a` con `e`, quindi la distanza è 2.

Conta il percorso più economico fra tutte le trasformazioni possibili. Stringhe identiche hanno distanza 0; la distanza grezza è un conteggio intero e può superare 1. Per ottenere una similarità normalizzata occorre specificare un'ulteriore definizione: non basta calcolare $1-d$.

È utile per varianti ortografiche e possibili errori di battitura. **La vicinanza nella scrittura non garantisce vicinanza di significato**: `cane` e `pane` differiscono per una sola lettera, ma indicano concetti diversi.

## Come scegliere la misura

| Misura | Rappresentazione | Che cosa confronta | Interpretazione |
| :--- | :--- | :--- | :--- |
| Manhattan | Vettori numerici | Somma degli scarti assoluti | Più piccola = più vicini; minimo 0. |
| Euclidea | Vettori numerici | Lunghezza del segmento fra i punti | Più piccola = più vicini; minimo 0. |
| Minkowski | Vettori numerici | Scarti aggregati secondo $p\geq1$ | Comprende Manhattan ed euclidea. |
| Jaccard | Insiemi finiti di caratteristiche | Quota condivisa rispetto all'unione | Similarità da 0 a 1; distanza complementare da 0 a 1. |
| Coseno | Vettori non nulli | Direzione e angolo | Similarità da $-1$ a 1; cosine distance da 0 a 2. |
| Levenshtein | Stringhe | Minimo numero di modifiche ai caratteri | Più piccola = grafie più vicine; minimo 0. |

La scelta parte da ciò che si vuole conservare: i valori delle caratteristiche, la loro presenza, le proporzioni fra le componenti oppure la sequenza dei caratteri. Per esempio, trasformando i conteggi $(7,3,2)$ in un insieme di parole presenti, si perde l'informazione su quante volte compaia ciascuna parola; passando al coseno si conserva invece il rapporto fra i conteggi, ignorando un loro riscalamento positivo comune.

In alta dimensionalità il solo ricorso ai vicini può diventare problematico: gli esempi più vicini possono essere comunque troppo lontani per sostenere una previsione affidabile. Conoscenza del dominio e selezione delle caratteristiche possono rendere il confronto più informativo, senza che esista una misura universalmente migliore.

Riferimenti e pagine delle slide: [[Fonti#Metriche di similarità|Fonti]]. Gli esempi numerici aggiuntivi sono rielaborazioni didattiche; quello del coseno con $(7,3,2)$ e $(2,3,0)$ riprende la pagina PDF 28 delle slide.
