# Logica fuzzy

Capitolo: [[Logica del primo ordine, programmazione logica e altre logiche]]

La **logica fuzzy** permette di trattare proprietà che si applicano per gradi, come «caldo», «alto» o «vicino». Nella logica classica un enunciato è vero oppure falso; nella logica fuzzy può assumere un grado di verità compreso fra 0 e 1. L'obiettivo è formalizzare concetti dai confini graduali, mantenendo precise le operazioni con cui vengono trattati.

Per esempio, definire «caldo» come «temperatura almeno pari a 25 °C» crea un salto netto fra 24,9 °C e 25 °C. Una rappresentazione fuzzy può descrivere una transizione progressiva: all'aumentare della temperatura cresce il grado con cui la situazione soddisfa il concetto di caldo.

## Insiemi fuzzy e funzione di appartenenza

In un insieme classico un elemento appartiene oppure non appartiene all'insieme. Un **insieme fuzzy** $A$ su un dominio $D$ è invece descritto da una funzione di appartenenza $\mu_A:D\rightarrow[0,1]$. Il valore $\mu_A(x)$ indica quanto l'elemento $x$ soddisfa il concetto rappresentato da $A$.

Per un esempio didattico, definiamo «caldo» sulla temperatura $T$, misurata in gradi Celsius:

$$
\mu_{caldo}(T)=
\begin{cases}
0 & T\leq 20,\\
(T-20)/10 & 20<T<30,\\
1 & T\geq 30.
\end{cases}
$$

A 26 °C il grado di appartenenza è 0,6. Il risultato non è arbitrario una volta fissata la funzione: segue dal calcolo $(26-20)/10$. È invece una scelta del modello stabilire soglie e forma della funzione. Un'altra applicazione potrebbe definire «caldo» diversamente, perché il significato dipende dal contesto.

**Appartenenza fuzzy e probabilità rispondono a domande diverse.** Il valore 0,6 non significa che ci sia il 60% di probabilità che la temperatura sia calda. La temperatura può essere nota esattamente: è il concetto di caldo a essere graduale. Una probabilità descrive invece l'incertezza sul verificarsi di un evento, per esempio che domani la temperatura superi una soglia fissata. Vaghezza e incertezza probabilistica possono coesistere, ma non coincidono.

## Come si combinano condizioni graduali

Per ragionare su più condizioni occorre definire connettivi che operino sui gradi. Una scelta comune usa il minimo per la congiunzione, il massimo per la disgiunzione e il complemento a uno per la negazione:

$$
v(A\land B)=\min(v(A),v(B)),\qquad
v(A\lor B)=\max(v(A),v(B)),\qquad
v(\neg A)=1-v(A).
$$

Se «è caldo» vale 0,6 e «è umido» vale 0,8, «è caldo e umido» vale 0,6, «è caldo oppure umido» vale 0,8 e «non è caldo» vale 0,4. La congiunzione è limitata dalla condizione meno soddisfatta. Questi operatori sono una scelta comune, non l'unica: altre logiche fuzzy usano, per esempio, il prodotto per la congiunzione. Anche le proprietà delle formule dipendono dalla scelta degli operatori.

## Dalle regole a una decisione numerica

Un controllore fuzzy usa regole come «se la temperatura è calda, allora aumenta la ventilazione». Per applicarle a una misura si calcolano prima i gradi di appartenenza: questa operazione è la **fuzzificazione**. Si valuta poi quanto è soddisfatta la condizione di ogni regola e si combinano i suoi effetti con quelli delle altre.

Completiamo l'esempio con un semplice sistema **Sugeno a conseguenti costanti**. Definiamo, solo per questo modello, il grado di «tiepido» come $1-\mu_{caldo}(T)$. Usiamo due regole: «se è tiepido, imposta la ventilazione al 40%» e «se è caldo, imposta la ventilazione all'80%». A 26 °C si attivano rispettivamente con intensità 0,4 e 0,6. Con pesi delle regole uguali a uno, l'uscita è la media dei conseguenti pesata dalle attivazioni:

$$
u=\frac{0{,}4\cdot40+0{,}6\cdot80}{0{,}4+0{,}6}=64\%.
$$

Il 64% è un comando concreto ricavato da condizioni graduali; il grado 0,6 non è stato trasformato direttamente in una ventilazione del 60%. Il risultato dipende sia dalle funzioni di appartenenza sia dalle regole e dai loro conseguenti.

Nei sistemi **Mamdani**, invece, anche i conseguenti sono insiemi fuzzy, come «ventilazione alta». Si modifica l'insieme di uscita di ciascuna regola secondo la sua attivazione e si aggregano i risultati; infine la **defuzzificazione** ricava un singolo valore numerico. Il metodo del baricentro sceglie il punto di equilibrio dell'area sotto la funzione di appartenenza aggregata. La media usata nell'esempio Sugeno e il baricentro Mamdani appartengono quindi a procedure differenti.

## Utilità e limiti

La logica fuzzy è utile quando regole linguistiche comprensibili devono produrre risposte graduali, per esempio nel controllo di temperatura o velocità. La leggibilità delle regole aiuta a capire il comportamento, ma non ne garantisce l'adeguatezza: funzioni e regole devono essere scelte e verificate per il problema. Con molti ingressi il numero di combinazioni da gestire può crescere rapidamente.

Rispetto alla [[Logica del primo ordine]], cambia soprattutto il trattamento della verità delle proprietà vaghe. Non introduce da sola una descrizione dell'evoluzione nel tempo, né sostituisce i modelli del [[Ragionamento in condizioni di incertezza|ragionamento probabilistico]].
