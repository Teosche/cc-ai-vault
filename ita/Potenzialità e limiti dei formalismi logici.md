# Potenzialità e limiti dei formalismi logici

Capitolo: [[Logica del primo ordine, programmazione logica e altre logiche]]

Un formalismo logico rende espliciti ciò che si assume, ciò che si vuole esprimere e le condizioni che autorizzano una conclusione. Il suo valore sta nella possibilità di controllare il ragionamento e riutilizzare la conoscenza. Per valutarlo bisogna però distinguere **espressività, correttezza delle premesse e costo del calcolo**.

## Formalismi diversi per esigenze diverse

La [[Logica del primo ordine]] descrive individui, proprietà e relazioni e consente di formulare regole quantificate. La [[Logica fuzzy]] tratta proprietà applicabili per gradi; la [[Logica temporale]] esprime vincoli sull'evoluzione degli stati. Queste esigenze sono diverse e possono presentarsi insieme: un robot può dover rappresentare gli oggetti, valutare quanto un ostacolo sia vicino e rispettare un requisito temporale sull'arresto.

| Formalismo | Domanda caratteristica | Esempio |
| :--- | :--- | :--- |
| Primo ordine | Quali proprietà e relazioni valgono per gli individui? | Ogni studente frequenta almeno un corso. |
| Fuzzy | In quale grado una situazione soddisfa un concetto? | La temperatura soddisfa «caldo» con grado 0,6. |
| Temporale | Quando e lungo quali evoluzioni vale una condizione? | Ogni richiesta sarà prima o poi seguita da un completamento. |

La [[Programmazione logica]] riguarda invece un modo di calcolare risposte da fatti e regole, con scelte operative come la ricerca a ritroso e il backtracking. Non è semplicemente un altro grado di verità o un operatore temporale.

## Una dimostrazione non verifica il mondo

Un'inferenza corretta preserva la verità delle premesse, ma non dimostra che queste descrivano adeguatamente la realtà. Se si assume che tutti gli studenti abbiano superato un esame, il sistema può dedurre che Luca lo abbia superato; il problema è nell'assunzione, anche se il passaggio deduttivo è corretto.

Anche l'informazione mancante va distinta da quella falsa. Nella logica classica del primo ordine, l'assenza di un fatto dalla base non implica la sua negazione. In Prolog la negazione come fallimento richiede attenzione alla completezza dei dati e alla terminazione della ricerca. Nei sistemi fuzzy, un grado di appartenenza non diventa automaticamente una probabilità; nella verifica temporale, un requisito dimostrato sul modello dipende dall'adeguatezza di quel modello.

## Completezza, decidibilità e tempi di calcolo

Un sistema di prova è **corretto** se dimostra soltanto conseguenze logiche delle premesse; è **completo** se permette di dimostrare tutte le conseguenze logiche. La completezza non garantisce che un algoritmo risponda sempre a ogni domanda. Per la logica del primo ordine generale esistono sistemi di prova corretti e completi, ma il problema della validità è **indecidibile**: non esiste un algoritmo che termini su ogni formula e decida sempre se sia valida oppure no.

La validità è **semidecidibile**: una ricerca sistematica appropriata può trovare una dimostrazione quando la formula è valida, ma può continuare indefinitamente quando non lo è. Questo limite teorico non va confuso con la lentezza: un problema decidibile può comunque richiedere risorse impraticabili. Viceversa, l'indecidibilità generale non impedisce di risolvere molti casi concreti o di usare frammenti del linguaggio con proprietà migliori.

La scelta del formalismo deve quindi partire dal compito. Una maggiore espressività può rendere la descrizione più naturale e compatta, ma può complicare l'inferenza. Analogamente, un modello temporale finito può essere verificabile e tuttavia troppo grande da esplorare agevolmente. Il criterio utile è rappresentare le distinzioni necessarie e scegliere procedure compatibili con le risorse disponibili, senza confondere precisione della notazione e garanzia di successo.
