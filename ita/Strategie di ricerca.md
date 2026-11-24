# Strategie di ricerca

Capitolo: [[Problem-solving, ricerca e similarità]]

Risolvere un problema mediante ricerca significa *individuare una sequenza di azioni che porti dalla situazione corrente a una situazione desiderata*. La scelta di un'azione dipende così non soltanto da ciò che accade adesso, ma anche dagli effetti che essa può produrre e dalle azioni che diventeranno possibili in seguito. Questa è la prospettiva degli [[Goal-Based Agents|agenti basati su obiettivi]].

## Stati, azioni e obiettivi

L'esempio di un taxi chiarisce il problema. A un incrocio sono disponibili più direzioni, ma la posizione attuale non basta a stabilire quale prendere: occorre conoscere la destinazione e le conseguenze previste delle alternative. Un'azione può avvicinare all'obiettivo senza raggiungerlo immediatamente; una soluzione può quindi richiedere più passaggi coordinati.

Lo spazio da esplorare dipende dalla rappresentazione. Per cercare un percorso fra città può bastare rappresentare uno stato con il nome della città corrente. È una rappresentazione atomica: lo stato viene trattato come un'unità indivisibile e le azioni collegano stati diversi. Se contano anche carburante, posizione precisa e disponibilità economica, la descrizione deve includere altre variabili. Una scelta di rappresentazione troppo povera può nascondere condizioni necessarie alla soluzione.

La ricerca costruisce e confronta possibilità all'interno di questo modello. Non coincide con l'esecuzione fisica di tutte le alternative: il modello permette di *anticiparne gli effetti* prima di agire. La distinzione fra previsione e azione è importante, perché *una sequenza corretta nel modello può risultare inadeguata se il modello non descrive bene l'ambiente*.

## Guidare l'esplorazione con la conoscenza

Una **strategia di ricerca** stabilisce come organizzare l'esplorazione delle alternative. La difficoltà fondamentale è che combinare molte azioni possibili può produrre un numero enorme di sequenze. Un meccanismo generale che concatena passi elementari può essere applicabile a molti problemi e tuttavia diventare poco efficace su quelli più grandi.

La **conoscenza del dominio** può ridurre questa difficoltà. Nel caso DENDRAL, il compito è individuare strutture molecolari compatibili con dati sperimentali. Generare indiscriminatamente le strutture possibili diventa rapidamente impraticabile; riconoscere configurazioni caratteristiche nello spettro consente invece di restringere le ipotesi. Il vantaggio non deriva soltanto da un computer più veloce, ma dall'uso di conoscenze che rendono l'esplorazione più selettiva.

Conoscenza e ricerca non sono quindi alternative assolute. **La conoscenza aiuta a decidere quali possibilità generare o esaminare, mentre la ricerca utilizza questa guida per costruire una soluzione**.

## Trovare una soluzione e sceglierne una migliore

Raggiungere l'obiettivo non significa necessariamente ottenere la soluzione più conveniente. Diversi percorsi possono portare alla stessa destinazione, ma differire per durata, costo o sicurezza. L'[[Ottimizzazione]] introduce un criterio per confrontare soluzioni ammissibili, mentre un obiettivo può limitarsi a distinguere quelle che soddisfano la richiesta da quelle che non la soddisfano.

Anche i mezzi necessari alla ricerca fanno parte del problema pratico. Una procedura che richiede troppe risorse può essere inutilizzabile pur descrivendo correttamente una soluzione possibile. La capacità di risolvere un problema in linea di principio va distinta dalla capacità di farlo entro i vincoli di tempo e memoria disponibili.
