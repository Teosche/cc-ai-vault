# Casi di studio su bias e rischi dell’IA

Capitolo: [[Explainable AI, etica e bias]]

Un caso di studio è utile quando permette di seguire il passaggio fra un comportamento osservato, l'ipotesi che lo spiega e il controllo necessario. I casi seguenti mostrano problemi a livelli diversi: caratteristiche visive, rappresentazione di gruppi, interpretazione delle tabelle e poteri di un agente.

## Un fiore classificato come ape

Un classificatore assegna correttamente «ape» a un'immagine con ape e fiore, ma assegna la stessa classe anche a un fiore senza insetto. La prima previsione, osservata da sola, non chiarisce quale indizio abbia sostenuto la decisione. La seconda rende plausibile l'ipotesi che il contesto floreale sia diventato una scorciatoia.

Le immagini sintetiche di DiffExplainer e DEXTER aiutano a esplorare il problema: l'ottimizzazione può trovare fiori che attivano «ape», oppure neve, alberi e cani che sostengono categorie associate a scene più complesse. Il generatore rende visibili combinazioni gradite al classificatore. Per misurare la dipendenza servono poi prove che variino oggetto e contesto, perché anche il generatore ha le proprie preferenze.

## Report sull'età e rappresentazione dei gruppi

In un esempio DEXTER relativo alla previsione dell'età, immagini e descrizioni sostengono un report che segnala una possibile sovrarappresentazione maschile. Il confronto illustrativo presenta una composizione 70/30 e una 50/50; il testo del report cambia fra i due casi.

La procedura trasforma esempi visivi in un'ipotesi espressa in linguaggio naturale. La catena include però più componenti: immagini prodotte, classificazione, caption e analisi testuale. Una distorsione può comparire in ciascuno. Il report può guidare un'indagine, mentre una conclusione sulle differenze di errore fra gruppi richiede dati etichettati e una valutazione quantitativa separata.

## Una risposta sulla tabella guidata dalle parole

Il Table QA richiede di interpretare colonne, righe e condizioni della domanda. Contare tutte le occorrenze di un punteggio può essere sbagliato se alcune descrivono la squadra e altre l'avversaria. Le parole della richiesta possono inoltre attivare un'associazione appresa con «sì», prevalendo sulla verifica dei dati.

Riformulare la domanda e aggregare evidenze cerca di ridurre questa dipendenza. Il controllo decisivo rimane ricostruire quali righe soddisfino effettivamente la condizione. La concordanza fra più risposte è utile soltanto se nasce da letture corrette, non da un errore condiviso.

## Un errore che diventa azione o memoria

Un assistente per clienti può essere autorizzato a consultare ordini ma disporre anche di strumenti capaci di effettuare rimborsi. Se interpreta una richiesta come autorizzazione a usarli, un errore di decisione produce una modifica reale. L'excessive agency riguarda questa distanza fra compito, poteri e controllo dell'esecuzione.

Nel memory poisoning, un'informazione falsa o un'istruzione ingannevole viene conservata e influenza cicli successivi. Il problema persiste anche quando il messaggio originario non è più nel contesto immediato. Nel tool misuse, il sistema usa invece uno strumento in modo incompatibile con il compito o con le autorizzazioni previste.

Questi casi richiedono di ricostruire l'origine delle informazioni, limitare le operazioni disponibili e verificare gli effetti prima di considerarli acquisiti. L'analisi del solo testo finale sarebbe insufficiente: la valutazione riguarda anche ciò che è stato eseguito e ciò che verrà riutilizzato.
