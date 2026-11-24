---
tags:
  - ragionamento
  - deduzione
  - logica
---
Il **ragionamento deduttivo** permette di ricavare conclusioni che seguono necessariamente dalle premesse assunte. La necessità riguarda il rapporto logico tra **premesse** e **conclusione**: se le premesse sono vere e l'inferenza è valida, la conclusione non può essere falsa. Non basta, invece, che una frase abbia l'aspetto di una conclusione logica per considerarla una descrizione vera del mondo.

## Il ragionamento come capacità cognitiva

Nelle architetture cognitive il ragionamento è la capacità di elaborare la conoscenza in modo logico e sistematico per ricavarne conclusioni. Non opera isolatamente: utilizza informazioni rese disponibili dalla percezione e dalla memoria, può orientare la scelta dell'azione e può contribuire all'apprendimento. Deduzione, induzione ed abduzione sono tre forme classiche di inferenza, collocando il ragionamento anche rispetto al problema pratico di scegliere la successiva azione migliore.

Il punto di partenza della deduzione è quindi una conoscenza già rappresentata. Le premesse non devono necessariamente essere leggi universali: possono includere fatti relativi a un caso specifico, regole generali e altre conclusioni precedentemente ottenute. L'elemento caratterizzante è che il passaggio alla conclusione conserva la verità, quando essa è presente nelle premesse. Il [[Ragionamento induttivo]], al contrario, estende le osservazioni a casi non ancora osservati e non offre la stessa garanzia.

## La struttura dell'inferenza e l'esempio del venerdì

 Se è venerdì, una certa persona andrà al lavoro. Poichè è venerdì si conclude che la persona andrà al lavoro. La conclusione è necessaria rispetto alle due premesse prese insieme; il solo fatto che sia venerdì, senza la regola, non basterebbe a giustificarla.

Come **formalizzazione didattica** dell'esempio, indichiamo con $P$ la proposizione «è venerdì» e con $Q$ la proposizione «andrà al lavoro». Lo schema, comunemente chiamato *modus ponens*, è:

$$
P\rightarrow Q,\qquad P\qquad\therefore Q.
$$

L'implicazione $P\rightarrow Q$ e l'affermazione $P$ svolgono ruoli differenti. La prima esprime il collegamento assunto fra due proposizioni; la seconda stabilisce che, nel caso in esame, l'antecedente è soddisfatto. L'inferenza utilizza entrambe. Non sarebbe corretto leggere lo stesso schema al contrario e considerare la sola verità di $Q$ una prova di $P$: la regola non esclude che la persona vada al lavoro anche in altri giorni. Questa precisazione esplicita la direzione del condizionale dell'esempio.

Una struttura analoga è quella del sillogismo su Socrate: tutti gli uomini sono mortali; Socrate è un uomo; dunque Socrate è mortale. La premessa generale e quella relativa all'individuo consentono di attribuire all'individuo una proprietà prevista dalla regola. La codifica di forme corrette di ragionamento è alla base dell'approccio delle «leggi del pensiero».

## Validità dell'inferenza e verità delle premesse

Una distinzione indispensabile è quella tra **validità logica** e **verità fattuale**. Un'inferenza valida garantisce che non si passi da premesse tutte vere a una conclusione falsa. *Non garantisce che le premesse adottate descrivano effettivamente il mondo*. La verità delle premesse richiede quindi una verifica distinta dalla correttezza del passaggio inferenziale.

Se piove, il terreno è asciutto; piove; dunque il terreno è asciutto. Lo schema logico è lo stesso dell'esempio precedente. Il problema è il contenuto della regola assunta, non la forma del passaggio alla conclusione. La deduzione non corregge automaticamente una conoscenza iniziale errata.

In termini di studio, bisogna quindi chiedersi separatamente se la conclusione segue dalle premesse e se le premesse sono accettabili. È questo il senso rigoroso dell'osservazione secondo cui ***una conclusione ottenuta logicamente non è necessariamente vera***: *la garanzia è condizionale alla verità delle premesse, non indipendente da essa.*

## Dalla logica ai sistemi artificiali

L'approccio logicista cerca di rappresentare la conoscenza mediante un linguaggio formale e di applicare procedure capaci di derivarne conseguenze. Questo approccio incontra due difficoltà principali. La prima riguarda la traduzione della conoscenza informale in enunciati precisi, soprattutto quando tale conoscenza non è completamente certa. La seconda è computazionale: poter risolvere un problema in linea di principio non significa poterlo risolvere entro il tempo e la memoria disponibili.

Questi limiti collegano la deduzione al [[The problem of knowledge representation|problema della rappresentazione della conoscenza]]. Il ragionamento può lavorare soltanto sulle distinzioni e sulle relazioni che il sistema riesce a rappresentare. I Knowledge Graphs mostrano, per esempio, che semplici etichette di categoria non bastano a stabilire che una categoria sia più generale di un'altra: occorre rappresentare una gerarchia di specializzazione. Riconoscere che una mela appartiene alla categoria della frutta consente di sostenere una sostituzione a quel livello di generalità.

Non ogni arco di un grafo è però una regola di deduzione. Il punto è la necessità di un principio organizzativo che attribuisca significato alle relazioni. Una tassonomia e un'ontologia possono fornire conoscenza utile al ragionamento; il semplice fatto di aver memorizzato collegamenti non specifica, da solo, tutte le inferenze lecite.

## Deduzione, azione razionale e modelli di linguaggio

L'[[Agente Razionale|agente razionale]] può dedurre che una certa azione raggiungerà un obiettivo e scegliere di eseguirla. La razionalità è tuttavia più generale della deduzione: in alcune situazioni non esiste un'azione dimostrabilmente corretta, ma l'agente deve comunque agire. Inoltre una risposta riflessa, come allontanarsi da una superficie bollente, può risultare più efficace di una lunga deliberazione.

La deduzione va perciò distinta dal [[Ragionamento in condizioni di incertezza|ragionamento sotto incertezza]] e dalla decisione. Una derivazione stabilisce che cosa segue da certe premesse; la decisione richiede anche obiettivi, preferenze e, quando gli esiti non sono certi, una valutazione delle conseguenze possibili.

Il ragionamento deliberativo nei modelli di linguaggio mira a migliorare la coerenza e la manipolazione di concetti astratti, ma comporta costi computazionali e può risentire di bias e difficoltà di spiegazione. Nel benchmark LogiCAM emergono problemi di allineamento logico fra modalità e ricorso a scorciatoie euristiche. Questi limiti riguardano i compiti e le condizioni del benchmark: una risposta articolata di un LLM richiede comunque una verifica della validità dei passaggi, prima di poter essere considerata una dimostrazione formale.
