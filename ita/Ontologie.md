# Ontologie

Capitolo: [[Tecnologie semantiche, Knowledge Graphs e ontologie]]

Un'ontologia esplicita categorie e relazioni rilevanti per un dominio, organizzandone il significato in modo che persone e sistemi possano utilizzarlo coerentemente. Il suo ruolo non è soltanto elencare termini, ma rendere disponibili distinzioni e collegamenti necessari per interpretare la conoscenza.

## Dalle etichette alle tassonomie

Un'etichetta come «mammifero» identifica una categoria, ma non descrive da sola quali categorie siano più specifiche. Una tassonomia costruisce una gerarchia di generalizzazione e specializzazione: una categoria più specifica viene collegata a una più generale mediante una relazione di sottoclasse.

Occorre distinguere il rapporto fra categorie dall'appartenenza di un individuo a una categoria. Un singolo animale può essere una giraffa; la categoria giraffa può essere una specializzazione di mammifero. Nel primo caso si classifica un individuo, nel secondo si organizza una gerarchia di concetti. Confondere i due livelli rende ambigua la rappresentazione.

Nel caso dei prodotti, riconoscere mela e banana come tipi di frutta permette di confrontarli al livello della categoria generale. Non dimostra però che siano intercambiabili per ogni scopo: una proprietà comune non elimina tutte le differenze rilevanti. L'informazione tassonomica deve essere letta rispetto alla domanda che si sta affrontando.

## Relazioni più articolate

Un'ontologia può esprimere anche rapporti diversi dalla specializzazione, come `PART_OF`, `COMPATIBLE_WITH` e `DEPENDS_ON`. Può inoltre caratterizzare relazioni mediante proprietà come simmetria o transitività. Queste proprietà devono essere appropriate al significato scelto, non attribuite indistintamente a ogni arco.

Una relazione simmetrica permette di scambiare i ruoli dei due elementi mantenendo il rapporto; una relazione transitiva permette di comporre due rapporti dello stesso tipo. Sono chiarimenti del significato matematico dei termini: non affermano che ogni compatibilità sia transitiva o che ogni relazione parte-tutto abbia le stesse condizioni in tutti i domini.

La presenza di relazioni esplicite rende possibile formulare domande più precise, ma non sostituisce il procedimento che applica regole o esegue interrogazioni. Modellare una relazione e disporre di un motore capace di usarne le proprietà sono aspetti distinti.

## Ontologie ponte e interoperabilità

Quando più sistemi descrivono domini collegati, un'ontologia può fungere da ponte semantico. Il collegamento richiede corrispondenze fra concetti, attributi e relazioni; non si riduce a rinominare campi con parole uguali.

Un esempio mette in relazione un archivio sanitario e un archivio di fitness. Nel primo compaiono pazienti, medici e appuntamenti; nel secondo utenti, allenatori e sessioni di allenamento. Una categoria generale `Person` raccoglie gli aspetti condivisi degli individui, mentre `Session` generalizza gli eventi con partecipanti e professionisti.

Le informazioni specifiche rimangono distinguibili: storia clinica e obiettivi di fitness non diventano lo stesso attributo. Il ponte rende possibile muoversi fra descrizioni diverse senza cancellare le differenze del dominio. Generalizzare due categorie sotto una categoria comune non significa dichiararle equivalenti.

## Uso e limiti

Un vocabolario condiviso facilita l'integrazione e il riuso della conoscenza nei [[Knowledge Graphs]]. La qualità del risultato dipende dalla scelta dei concetti e dall'adeguatezza delle relazioni: un'ontologia coerente sul piano formale può comunque rappresentare male un particolare compito o una parte del mondo.

Va inoltre considerato il costo del ragionamento. Aumentare l'espressività permette di descrivere più distinzioni, ma può rendere più complessa l'elaborazione. La progettazione richiede quindi un equilibrio fra ricchezza della descrizione, esigenze applicative e procedure disponibili, come discusso in [[Potenzialità e limiti dei formalismi logici]].
