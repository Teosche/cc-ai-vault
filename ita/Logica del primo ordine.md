# Logica del primo ordine

Capitolo: [[Logica del primo ordine, programmazione logica e altre logiche]]

La **logica del primo ordine** è un linguaggio formale con cui descrivere oggetti, le loro proprietà e le relazioni che li collegano, ed esprimere regole generali su di essi. La sintassi stabilisce come costruire le formule; al linguaggio si associano una semantica, che stabilisce quando gli enunciati sono veri, e regole di inferenza, che permettono di ricavare conclusioni dalle premesse. In intelligenza artificiale serve a rappresentare conoscenza esplicita e a svolgere [[Ragionamento deduttivo|ragionamento deduttivo]].

Per capirne l'utilità, consideriamo due informazioni: «Socrate è un uomo» e «tutti gli uomini sono mortali». Un sistema dovrebbe poterle collegare e concludere che Socrate è mortale. Il punto non è soltanto memorizzare tre frasi, ma rappresentare la struttura che rende la terza una conseguenza delle prime due.

## Dalle proposizioni agli oggetti

La logica proposizionale tratta enunciati interi come unità a cui assegnare vero o falso. Possiamo chiamare $P$ la frase «Socrate è un uomo» e $Q$ la frase «Socrate è mortale», quindi scrivere $P\rightarrow Q$. La relazione fra i due enunciati è rappresentata, ma i simboli $P$ e $Q$ non mostrano che si riferiscono alla stessa persona. Per parlare anche di Platone occorrerebbero altri simboli e altre regole riferite al suo caso.

La logica del primo ordine rende invece visibile questa struttura: scrive $Uomo(Socrate)$ e $Mortale(Socrate)$, distinguendo l'individuo dalle proprietà che gli vengono attribuite. Introducendo una variabile $x$ può poi esprimere una sola regola valida per qualunque individuo sia un uomo. È questa combinazione di **oggetti, predicati e quantificatori** a renderla più espressiva della logica proposizionale.

Si chiama «del primo ordine» perché i quantificatori variano sugli individui del dominio. Non quantificano direttamente su predicati o proprietà, come avviene nella logica del secondo ordine. «Primo ordine» non significa che si possa usare una sola variabile o descrivere soltanto relazioni semplici.

## Come si costruiscono le formule

Prima di formalizzare occorre scegliere il **dominio**, cioè l'insieme degli oggetti di cui si parla. In un esempio universitario può comprendere persone e corsi; in un problema aritmetico può essere l'insieme dei numeri naturali. I quantificatori si riferiscono sempre a quel dominio, non indistintamente a ogni cosa esistente.

Una **costante** è un simbolo che designa un oggetto, come $Luca$ o $Cognitive$. Una **variabile**, come $x$, può riferirsi a un oggetto del dominio senza nominarlo in anticipo. Un **predicato** esprime una proprietà o una relazione: $Studente(Luca)$ attribuisce una proprietà a Luca; $Frequenta(Luca,Cognitive)$ mette in relazione una persona e un corso. Il numero di argomenti è detto *arità*: i due predicati hanno rispettivamente arità uno e due. L'ordine degli argomenti conta: chi frequenta e ciò che viene frequentato hanno ruoli diversi.

Le funzioni servono invece a designare oggetti a partire da altri oggetti. Nel dominio dei naturali, $successore(2)$ designa il numero 3. **Un termine designa un oggetto; una formula afferma qualcosa che può essere vero o falso**. Costanti, variabili e applicazioni di funzioni sono termini; $Studente(Luca)$ è una formula atomica. Nella semantica standard una funzione assegna esattamente un risultato a ogni combinazione di argomenti del dominio. Il simbolo di uguaglianza, quando presente, permette di affermare che due termini designano lo stesso oggetto.

Le formule atomiche si combinano usando i connettivi: $\neg P$ significa «non $P$», $P\land Q$ «$P$ e $Q$», $P\lor Q$ «almeno uno fra $P$ e $Q$», $P\rightarrow Q$ «se $P$, allora $Q$» e $P\leftrightarrow Q$ «$P$ se e solo se $Q$». Nell'implicazione, $P$ è l'antecedente e $Q$ il conseguente: la formula esclude il caso in cui $P$ sia vero e $Q$ falso. Non afferma da sola né che $P$ sia vero né che $P$ causi $Q$.

## Quantificatori: tutti e almeno uno

Il **quantificatore universale** $\forall$ permette di affermare che una condizione vale per ogni oggetto del dominio. «Tutti gli uomini sono mortali» diventa:

$$
\forall x\,\bigl(Uomo(x)\rightarrow Mortale(x)\bigr).
$$

Si legge: «per ogni $x$, se $x$ è un uomo, allora $x$ è mortale». L'implicazione restringe l'obbligo agli uomini: la formula non richiede che ogni oggetto del dominio sia un uomo. Scrivere invece $\forall x\,(Uomo(x)\land Mortale(x))$ affermerebbe che tutti gli oggetti sono sia uomini sia mortali.

Il **quantificatore esistenziale** $\exists$ afferma che esiste almeno un oggetto con una certa proprietà. «Esiste uno studente che frequenta Cognitive» si scrive:

$$
\exists x\,\bigl(Studente(x)\land Frequenta(x,Cognitive)\bigr).
$$

Qui serve la congiunzione: lo stesso oggetto deve soddisfare entrambe le condizioni. «Almeno uno» non significa «esattamente uno» e non richiede di conoscere il nome dello studente. Usare l'implicazione al posto della congiunzione sarebbe inadeguato: basterebbe trovare un oggetto che non è studente perché l'implicazione risulti vera.

Anche l'ordine dei quantificatori cambia il significato. Confrontiamo:

$$
\forall x\,\bigl(Studente(x)\rightarrow
\exists y\,(Corso(y)\land Frequenta(x,y))\bigr)
$$

con:

$$
\exists y\,\bigl(Corso(y)\land
\forall x\,(Studente(x)\rightarrow Frequenta(x,y))\bigr).
$$

La prima formula dice che ogni studente frequenta almeno un corso, eventualmente diverso per ciascuno. La seconda richiede che esista uno stesso corso frequentato da tutti gli studenti, senza affermare che sia l’unico. Se Luca frequenta soltanto Cognitive e Anna soltanto Robotica, la prima è vera e la seconda falsa.

Le parentesi delimitano l'ambito dei quantificatori. In $Studente(x)$ la variabile è libera: per valutare la formula bisogna sapere a quale oggetto si riferisce $x$. In $\exists x\,Studente(x)$ è legata dal quantificatore. Una formula senza variabili libere è detta **chiusa**, o enunciato, e può essere usata come affermazione completa nella base di conoscenza.

## Dare significato ai simboli: interpretazioni e modelli

Scrivere una formula corretta non basta a stabilire se sia vera. Un'**interpretazione** specifica il dominio, gli oggetti designati dalle costanti, le funzioni associate ai simboli funzionali e le proprietà o relazioni associate ai predicati. Non è il nome $Studente$ a conferirgli automaticamente il significato desiderato: è l'interpretazione adottata.

Per esempio, scegliamo il dominio $\{Luca,Anna,Cognitive\}$, interpretiamo $Studente$ come la proprietà posseduta da Luca e Anna, e $Frequenta$ come la relazione contenente soltanto la coppia $(Luca,Cognitive)$. In questa interpretazione $Frequenta(Luca,Cognitive)$ è vera e $Frequenta(Anna,Cognitive)$ è falsa. È vero che esiste uno studente che frequenta Cognitive, ma è falso che tutti gli studenti lo frequentino.

Un'interpretazione che rende vere tutte le formule di una base di conoscenza è un suo **modello**. Dire $KB\models\varphi$ significa che $\varphi$ è vera in tutti i modelli della base $KB$: non esiste una situazione compatibile con le premesse in cui la conclusione sia falsa. Questa è la nozione di *conseguenza logica*.

## Come si ricava una conclusione

Riprendiamo la base di conoscenza con $\forall x\,(Uomo(x)\rightarrow Mortale(x))$ e $Uomo(Socrate)$. La regola universale vale anche per Socrate: sostituendo $x$ con quella costante otteniamo $Uomo(Socrate)\rightarrow Mortale(Socrate)$. Questo passaggio è l'**istanziazione universale**. Avendo anche l'antecedente $Uomo(Socrate)$, possiamo applicare il **modus ponens**, che da $P$ e $P\rightarrow Q$ ricava $Q$, e concludere $Mortale(Socrate)$.

La conclusione non era necessariamente memorizzata: è stata dedotta. Il procedimento non permette però di invertire la regola: sapere che qualcuno è mortale non basta a concludere che sia un uomo. Inoltre la validità del ragionamento non verifica le premesse rispetto al mondo reale: se la conoscenza iniziale è sbagliata, la deduzione non la corregge.

I sistemi automatici possono cercare dimostrazioni con metodi come la **risoluzione**. L'idea della dimostrazione per confutazione è aggiungere la negazione della conclusione e cercare una contraddizione. Nell'esempio, l'istanza della regola equivale a $\neg Uomo(Socrate)\lor Mortale(Socrate)$; combinandola con $Uomo(Socrate)$ si ottiene $Mortale(Socrate)$, incompatibile con la negazione aggiunta. La risoluzione del primo ordine usa anche l'unificazione, spiegata operativamente in [[Programmazione logica]].

Occorre distinguere una descrizione completa di un'interpretazione da una base di conoscenza incompleta. Se nella base manca $Studente(Giulia)$, non possiamo per questo dedurre $\neg Studente(Giulia)$: potrebbero esistere modelli compatibili in cui è vera e altri in cui è falsa. La logica resta a due valori in ogni interpretazione; è la nostra conoscenza a non determinare quale caso valga.
