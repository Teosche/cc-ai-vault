Capitolo: [[Reinforcement learning e processi decisionali]]

Il deep reinforcement learning usa reti neurali per rappresentare componenti di un agente che apprende da ricompense. Una rete può stimare il valore di stati o azioni, produrre una policy oppure descrivere la dinamica dell'ambiente. L'uso del deep learning cambia la rappresentazione e l'ottimizzazione; rimane l'obiettivo di migliorare il ritorno attraverso l'esperienza.

## Perché sostituire la tabella

Una tabella richiede una voce per ogni stato o coppia stato-azione. Per un input visivo, il numero di immagini possibili è troppo grande. Una rete con parametri condivisi può invece riconoscere strutture comuni e produrre valori anche per situazioni non incontrate esattamente in quella forma.

La condivisione comporta generalizzazione, ma anche interferenza: aggiornare la previsione per un'immagine modifica i pesi usati per molte altre. Nel reinforcement learning il target può inoltre dipendere dalle stime dello stesso modello. Se la rete cambia, può cambiare contemporaneamente ciò che predice e ciò che usa come obiettivo.

## Valore, policy e modello dell'ambiente

Nei metodi value-based, come le [[Deep Q-Networks]], la rete produce stime del ritorno e la scelta deriva dal confronto fra azioni. Nei metodi di ottimizzazione della policy, la rete produce direttamente una distribuzione sulle azioni. Nei metodi actor-critic, l'actor sceglie e il critic stima quanto siano promettenti stati o azioni, fornendo un segnale per aggiornare l'actor.

La distinzione fra model-free e model-based riguarda invece il modello dell'ambiente. Un metodo model-based utilizza o apprende transizioni e ricompense per prevedere conseguenze e pianificare. Una rete che stima soltanto il valore non è, per questo, un modello della dinamica. Le due classificazioni descrivono aspetti diversi e possono combinarsi.

## Dati prodotti dal comportamento

Nel supervisionato si parte spesso da un dataset fissato. In RL, una policy che cambia visita situazioni diverse e modifica i dati successivi. Una strategia inizialmente poco esplorativa può non scoprire mai un esito vantaggioso. Ricompense rare rendono difficile capire quali scelte precedenti abbiano aiutato a raggiungerlo.

Gli esempi consecutivi sono anche correlati: immagini vicine nel tempo possono essere quasi identiche. Replay, aggiornamenti controllati e raccolta di nuove esperienze affrontano aspetti di questa instabilità. La valutazione deve ripetere prove con condizioni e semi differenti, perché una singola traiettoria favorevole non misura la qualità attesa della policy.

## L'esempio dei modelli linguistici

Nell'RLHF la policy è il modello che genera la risposta. Il contesto può fungere da stato e la produzione di token da sequenza di azioni; un modello di reward valuta la risposta. Un critic può stimare il ritorno atteso per guidare un aggiornamento actor-critic, per esempio con [[Proximal Policy Optimization]].

Reward model e critic hanno ruoli diversi: il primo definisce il segnale usato per premiare le risposte; il secondo stima il ritorno sotto la policy. Migliorare il reward non garantisce la verità, perché il valutatore può avere difetti sfruttabili. La progettazione del feedback resta quindi parte essenziale del problema, anche quando l'ottimizzazione funziona.
