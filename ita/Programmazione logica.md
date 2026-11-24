# Programmazione logica

Capitolo: [[Logica del primo ordine, programmazione logica e altre logiche]]

La **programmazione logica** consiste nel descrivere un problema mediante fatti e regole e nel chiedere a un sistema di trovare risposte che seguano da quella descrizione. In un programma imperativo si specifica soprattutto una sequenza di operazioni; qui si dichiarano relazioni e condizioni, mentre un meccanismo di ricerca cerca come soddisfare la domanda. **Prolog** è un linguaggio che realizza questo approccio.

Il collegamento con la [[Logica del primo ordine]] è diretto, ma Prolog non esegue qualsiasi formula del primo ordine. Il suo nucleo usa una forma ristretta di regole; il comportamento dipende anche dalla strategia con cui vengono cercate le dimostrazioni.

## Fatti, regole e interrogazioni

Consideriamo questo piccolo programma:

```prolog
studente(luca).
studente(anna).
frequenta(luca, cognitive).
frequenta(anna, robotica).

iscritto_cognitive(X) :- studente(X), frequenta(X, cognitive).
```

Le prime quattro righe sono **fatti**, cioè informazioni assunte nella base di conoscenza. L'ultima è una **regola**: $X$ è iscritto a Cognitive se è uno studente e frequenta quel corso. `:-` separa la testa, `iscritto_cognitive(X)`, dal corpo, che contiene le condizioni da soddisfare; la virgola fra condizioni esprime una congiunzione. I nomi come `luca` sono atomi, le variabili iniziano con una maiuscola o un trattino basso, e il punto termina ogni clausola.

La lettura logica della regola è:

$$
\forall x\,\bigl((Studente(x)\land Frequenta(x,Cognitive))
\rightarrow IscrittoCognitive(x)\bigr).
$$

Una clausola è una disgiunzione di letterali, cioè formule atomiche o loro negazioni. Una clausola di Horn contiene al massimo un letterale positivo. Le regole dell'esempio appartengono alle **clausole definite**, che hanno esattamente una conclusione positiva. Questa restrizione consente un procedimento uniforme: per provare la testa si cerca di provare il corpo.

L'interrogazione `?- iscritto_cognitive(luca).` chiede se il fatto sia dimostrabile e ha esito positivo. La domanda `?- iscritto_cognitive(X).` chiede invece per quali valori della variabile valga la relazione: con questa base la risposta è `X = luca`. Non si restituisce soltanto vero o falso, ma anche una sostituzione che soddisfa la query.

## Unificazione e ricerca a ritroso

L'**unificazione** cerca sostituzioni delle variabili che rendano identiche due espressioni. Per esempio, `frequenta(X, cognitive)` e `frequenta(luca, cognitive)` si unificano sostituendo `X` con `luca`. La sostituzione deve restare coerente in tutte le occorrenze della stessa variabile.

Per rispondere a `iscritto_cognitive(X)`, Prolog parte dalla domanda e trova la regola con quella testa. La sostituisce con due sotto-obiettivi: `studente(X)` e `frequenta(X, cognitive)`. Il primo fatto propone `X = luca`; con la stessa sostituzione anche il secondo sotto-obiettivo riesce. La dimostrazione è completata. Questo è **ragionamento a ritroso**: dall'obiettivo si risale alle condizioni sufficienti per provarlo.

Se viene richiesta un'altra soluzione, Prolog torna al punto in cui aveva scelto Luca e prova Anna. `studente(anna)` riesce, ma `frequenta(anna, cognitive)` fallisce. Il ritorno alle scelte precedenti per provare alternative è detto **backtracking**. Nella ricerca standard le clausole vengono considerate dall'alto verso il basso e i sotto-obiettivi da sinistra a destra, esplorando una possibilità in profondità prima di tornare alle alternative.

## Significato logico e comportamento del programma

L'ordine di fatti e regole può cambiare il comportamento della ricerca anche quando non cambia il contenuto logico delle clausole definite. Una regola ricorsiva che richiama subito lo stesso obiettivo può far proseguire la ricerca indefinitamente prima che venga provata una clausola capace di risolverlo. Avere una conseguenza logica non garantisce quindi che una particolare esecuzione standard la trovi.

Va inoltre distinta la negazione classica dalla **negazione come fallimento**. In Prolog `\+ Goal` riesce quando il tentativo di dimostrare `Goal` fallisce in tempo finito. Con la base dell'esempio, `\+ frequenta(anna, cognitive)` riesce, ma questo significa che l'iscrizione non è dimostrabile dai fatti disponibili. Per interpretarlo come un fatto negativo sul mondo occorre assumere che le informazioni pertinenti siano complete. Se invece la ricerca di `Goal` non termina, neppure la sua negazione come fallimento fornisce una risposta.

La programmazione logica rende così operativa la distinzione fra conoscenza e inferenza, ma richiede di considerare insieme ciò che il programma afferma e il modo in cui cerca le risposte. Le regole di un agente che comandano direttamente un'azione hanno un ruolo diverso: una regola logica stabilisce anzitutto che cosa sia deducibile.
