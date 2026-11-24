---
tags:
  - architetture-cognitive
  - scienze-cognitive
  - paradigmi
  - sviluppare
  - apprendere
---

## I Tre Paradigmi della Scienza Cognitiva

Nel panorama della Scienza Cognitiva e dell'Intelligenza Artificiale, le architetture cognitive vengono classificate secondo tre paradigmi fondamentali.

- **Cognitivist Systems** 
  Basato sull'ipotesi dei sistemi di simboli fisici (Newell & Simon, 1976). La cognizione è vista come manipolazione di rappresentazioni simboliche esplicite e disincarnate. Esempi classici sono *SOAR* e *CRAM*.

- **Emergent Systems / Developmental Systems**
  La cognizione non è pre-programmata né basata sulla manipolazione di simboli astratti, ma emerge attraverso l'auto-organizzazione, la maturazione sensoriale-motoria e l'interazione continua dell'agente con il mondo.

- **Hybrid Systems**
  Combina sotto-sistemi emergenti/sensori-motori a basso livello con sotto-sistemi simbolici ad alto livello.
  
### Definizione di Developmental Cognitive Architecture (DCA)

Una **Developmental Cognitive Architecture** (Architettura Cognitiva dello Sviluppo) è un framework software per agenti artificiali che non viene fornito con conoscenze del mondo pre-confezionate o codificate a priori.
Al contrario, sviluppa le proprie abilità percezione-azione, rappresentazione e ragionamento attraverso un *processo continuo di apprendimento autonomo*, esplorazione ed adattamento nel corso del proprio ciclo di vita (*ontogenesi*).
  
## Il Concetto di Sviluppo

### La Visione di Alan Turing (Turing, 1950)

L'idea alla base delle architetture dello sviluppo risale ai fondamenti stessi dell'IA. Nel suo celebre articolo del 1950 *Computing Machinery and Intelligence*, Alan Turing rifletteva sulla difficoltà di programmare a mano una mente adulta:

> *"Instead of trying to produce a programme to simulate the adult mind, why not rather try to produce one which simulates the child's? If this were then subjected to an appropriate course of education one would obtain the adult brain."*

Anziché tentare di codificare la sterminata conoscenza di un adulto in un sistema rigido, Turing suggerì di realizzare una "macchina bambino" dotata di strutture di base e meccanismi di apprendimento, lasciando che l'esperienza e l'educazione sviluppino l'intelligenza finale.
  
### Filogenesi vs. Ontogenesi

Nelle architetture emergenti e dello sviluppo si traccia una distinzione fondamentale tra due dimensioni temporali dell'evoluzione biologica/computazionale:

#### **Filogenesi**
* Corrisponde alla configurazione del sistema **determinata dall'evoluzione**.
* Costituisce la struttura iniziale con cui l'agente nasce e fornisce la base per l'ontogenesi.
* Include *innate skills* e *core knowledge*.

#### **Ontogenesi**

* Riguarda il processo di sviluppo, crescita e maturazione dell'organismo individuale lungo tutto il suo ciclo di vita.
* L'architettura deve contenere meccanismi integrati in grado di sviluppare progressivamente: Percezione, Azione, Adattamento, Anticipazione e Motivazione.

## Le Teorie di Riferimento dello Sviluppo Cognitivo

Le architetture dello sviluppo traggono ispirazione diretta da due prospettive complementari della psicologia dello sviluppo:

### Jean Piaget (1896–1980) – Prospettiva Individuale

* **Teoria:** Costruttivismo Individuale. L'agente sviluppa la propria intelligenza ed abilità cognitive passando attraverso stadi progressivi di maturazione guidati dall'esplorazione autonoma e diretta dell'ambiente fisico inanimato.
* **Assimilazione:** Processo mediante il quale l'agente interpreta ed integra i nuovi oggetti ed eventi fisici all'interno degli schemi sensori-motori o percettivi che già possiede (es. applicare lo schema dell'afferrare ad un nuovo oggetto mai visto prima).
* **Accomodamento:** Processo mediante il quale l'agente modifica e ristruttura i propri schemi interni quando l'oggetto o l'ambiente oppone resistenza fisica o smentisce le aspettative dell'agente (es. ricalibrare la forza sensori-motoria della presa se l'oggetto si rivela troppo pesante o scivoloso).
* **Implicazione per l'IA:** L'agente artificiale deve essere progettato per apprendere ed evolversi interagendo direttamente con gli oggetti fisici del mondo attraverso un ciclo di feedback sensori-motorio continuo, costruendo concetti a partire dalle proprie azioni fisiche.
### Lev Vygotsky (1896–1934) – Prospettiva Sociale

* **Teoria:** Costruttivismo Socio-Culturale. La cognizione non si sviluppa in isolamento solipsistico, ma è un processo essenzialmente sociale e mediato dall'interazione con altri agenti (umani o artificiali), dalla cooperazione, dal linguaggio e dagli artefatti culturali (Concetto di *Zona di Sviluppo Prossimale*).
* **Assimilazione:** Processo mediante il quale l'agente acquisisce ed incorpora nei propri schemi cognitivi le dimostrazioni, i simboli, le convenzioni culturali, il linguaggio e i suggerimenti operativi forniti dagli altri agenti presenti nel contesto (es. assimilare l'uso corretto di uno strumento guidato dall'osservazione di un tutor).
* **Accomodamento:** Processo mediante il quale l'agente ristruttura e rimodella i propri modelli comportamentali e politiche interne in seguito al confronto con i feedback sociali, ai conflitti di intenzione con altri agenti o alla necessità di coordinamento (es. adeguare la propria politica decisionale dopo una correzione/penalità sociale o modulare l'azione per rispettare le aspettative e gli obiettivi dell'altro agente).
* **Implicazione per l'IA:** L'agente artificiale non deve apprendere in isolamento, ma all'interno di un ecosistema sociale popolato da altri agenti (umani o sintetici) con cui interagire, da cui imitare comportamenti, convenzioni e valori, ed adattarsi al coordinamento multi-agente.

## Embodied Cognition

Un pilastro imprescindibile delle architetture dello sviluppo è il concetto di [[Embodied Cognition|Embodied Cognition]].

## I 10 Desiderata per le Architetture Cognitive dello Sviluppo

Formalizzati nel lavoro cardine di *David Vernon, Claes von Hofsten e Luciano Fadiga (2016)* (*"Desiderata for developmental cognitive architectures"*, *BICA*, Elsevier).
#### 1. Value Systems and Motives
* L'agente deve possedere motivazioni intrinseche (es. curiosità, spinta all'esplorazione, ricerca di novità, riduzione dell'incertezza) ed estrinseche (sopravvivenza, ricompense esterne). Tali sistemi guidano lo sviluppo autonomo senza la necessità di un supervisore umano che indichi costantemente cosa imparare.
#### 2. Physical Embodiment
* L'architettura deve risiedere in un corpo fisico (o in una simulazione fisica accurata). La percezione, la cognizione e la pianificazione sono inestricabilmente vincolate dalla forma del corpo, dalla disposizione dei sensori e dalla dinamica degli attuatori.
#### 3. Sensorimotor Contingencies
* L'agente deve imparare le relazioni sistematiche che intercorrono tra le proprie azioni motorie ed i conseguenti cambiamenti nelle sensazioni percettive (*"Se muovo gli occhi a destra, il campo visivo scorre verso sinistra"*). Questo costituisce la base per la percezione dello spazio e dell'oggetto.
#### 4. Perception
* La percezione non è una cattura passiva di fotogrammi o dati sensoriali, ma un **processo attivo e guidato dall'azione** (*Active Perception*). L'agente percepisce l'ambiente per scoprire le **affordance** (ovvero le possibilità d'azione offerte dagli oggetti, come afferrare, spingere o tirare).
#### 5. Attention
* Poiché il flusso sensoriale è enorme, l'agente deve disporre di meccanismi attentivi per selezionare le informazioni rilevanti, orientando i sensori verso stimoli salienti o coerenti con le motivazioni attuali.
#### 6. Prospective Action
* La cognizione orientata allo sviluppo si distingue per la capacità di **anticipare il futuro**. L'agente non si limita a reagire agli stimoli, ma pianifica ed esegue azioni in prospettiva, prevedendo le conseguenze prima che si verifichino nel mondo reale.
#### 7. Declarative and Procedural Memory
* L'architettura deve integrare in modo dinamico:
	* **Memoria Procedurale**: Per la conservazione e l'affinamento delle abilità motorie e pratiche (*know-how*).
	* **Memoria Dichiarativa**:Per la rappresentazione di fatti, concetti ed eventi autobiografici legati al tempo e al luogo di apprendimento.
#### 8. Multiple Modes of Learning
* L'agente non può fare affidamento su un unico algoritmo di ML, ma deve combinare diverse modalità di apprendimento:
	* *Apprendimento per Rinforzo* (basato su premi/punizioni).
	* *Apprendimento Associativo / Ebbiano*.
	* *Apprendimento Sociale / Imitativo* (osservando altri agenti).
	* *Apprendimento Esplorativo / Per Prova ed Errore*.
#### 9. Internal Simulation
* L'agente deve essere in grado di staccarsi dal flusso sensoriale immediato per utilizzare le proprie strutture motorie e percettive al fine di **simulare internamente azioni ipotetiche** (immaginazione, pensiero controfattuale e pianificazione mentale).
#### 10. Constitutive Autonomy
* L'agente deve mantenere la propria integrità organizzativa ed autosufficienza funzionale. L'autonomia non è solo comportamentale (scegliere un'azione), ma costitutiva: il sistema impara a preservare la propria esistenza ed adattamento nell'ambiente.

## Confronto Sintetico: Architetture Cognitiviste vs. Architetture dello Sviluppo

| Dimensione | Architetture Cognitiviste ([[SOAR Cognitive Architecture|SOAR]]) | Architetture dello Sviluppo (DCA) |
| :--- | :--- | :--- |
| Origine della Conoscenza | Programmata o fornita tramite regole e simboli a priori dal progettista | Costruita autonomamente attraverso l'esperienza e l'ontogenesi |
| Natura della Rappresentazione | Simbolica esplicita, disincarnata e manipolabile | Emergente, dinamica, incastonata nei pattern sensori-motori (Embodied) |
| Ruolo del Corpo Fisico | Irrilevante / Secondario (in accordo con il funzionalismo computazionale) | Centrale ed imprescindible (la morfologia corporea struttura la cognizione) |
| Focus dello Sviluppo | Risoluzione di problemi complessi dell'adulto (Newell & Simon) | Maturazione graduale dalla "macchina bambino" (child machine di Turing) |
| Ispiratori Scientifici | Logica matematica, psicologia dei problemi dell'adulto | Alan Turing, Jean Piaget, Lev Vygotsky, David Vernon et al. |
| Meccanismo di Apprendimento | Riorganizzazione di regole simboliche (es. Chunking in SOAR) | 10 Desiderata (Motivazioni intrinseche, simulazione interna, contingenze sensori-motorie) |interna, contingenze sensori-motorie) |
