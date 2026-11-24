# Vision Transformer e imaging medico

Capitolo: [[Adattamento, valutazione e applicazioni dei Foundation Models]]

Un Vision Transformer applica l'elaborazione di sequenze alle immagini. L'input viene suddiviso in patch e ogni patch trasformata in un vettore; l'attenzione combina informazioni provenienti da regioni diverse. La rappresentazione appresa può alimentare una classificazione dell'immagine o un altro compito visivo.

## Patch e class token

Le patch vengono disposte in una sequenza, mantenendo informazione sulla posizione. Il class token è un vettore appreso aggiunto all'input e trasformato insieme alle patch. La sua rappresentazione finale viene utilizzata come sintesi per il classificatore: non è l'etichetta corretta inserita anticipatamente nel modello.

La quantità di token cresce con il numero di patch. Perciò la scelta della dimensione delle patch cambia sia la granularità dell'informazione sia il costo dell'elaborazione.

## Video e struttura gerarchica

ViViT estende il trattamento a sequenze video, usando frame o tubelet e organizzando l'attenzione nello spazio e nel tempo. Swin introduce invece finestre locali, spostamenti delle finestre fra livelli e merging delle patch. La struttura gerarchica riduce la risoluzione della rappresentazione e aumenta la ricchezza dei canali.

Lo spostamento delle finestre permette lo scambio di informazione fra regioni che, con finestre sempre identiche, resterebbero separate. Non implica che ogni strato calcoli attenzione globale su tutte le patch.

## Classificazione IPMN da risonanza magnetica

Il caso di studio considera scansioni MRI e un classificatore Transformer che aggrega slice consecutive. Le modalità T1 e T2 vengono combinate in ingresso mediante early fusion, e il class token sostiene la previsione della categoria prevista dal compito.

L'architettura deve trasformare un volume o un insieme di slice in un input compatibile con il modello. Rappresentare più slice permette di utilizzare informazioni che una singola sezione potrebbe non contenere, ma la modalità di aggregazione fa parte delle scelte progettuali e deve essere specificata.

La valutazione riportata riguarda 139 scansioni di pazienti distinti e una cross-validation a dieci fold. Il risultato descrive quell'esperimento: non costituisce una garanzia di efficacia su ogni popolazione o una validazione clinica generale. È essenziale distinguere unità di acquisizione, pazienti e suddivisione dei dati, evitando contaminazioni fra addestramento e valutazione.

## Interpretabilità dell'attenzione

L'attention rollout aggrega pesi fra teste e livelli per visualizzare percorsi di propagazione dell'informazione. Può aiutare a confrontare regioni evidenziate e area d'interesse, ma la mappa rimane il risultato di una procedura interpretativa. Non dimostra automaticamente che il modello utilizzi un criterio clinicamente corretto.

Una previsione corretta può dipendere anche da caratteristiche spurie. Per questo [[Metodi di interpretabilità dei modelli]] e [[Identificazione e mitigazione dei bias]] sono complementari alle metriche di prestazione.
