# 5.1 Principi hardware dell’I/O

*Dispositivi di I/O*
====

I dispositivi di I/O possono essere sommariamente suddivisi in due categorie: **dispositivi a blocchi e dispositivi a caratteri**. 

-   `Dispositivi a blocchi`:
    -   Un dispositivo a blocchi è quello che **archivia informazioni in blocchi di dimensioni fisse, ognuno con il proprio indirizzo.** 

    -   L’intervallo comune delle **dimensioni dei blocchi va da `512` a `32.768 byte`.** 
    **Tutti i trasferimenti sono in unità di uno o più blocchi (consecutivi) interi.**

    -   I dispositivi che ricadono i questa categoria sono: **dischi fissi, dischi Blu-ray e penne USB.**

    -   >**NOTA:**  La caratteristica essenziale di un dispositivo a blocchi è che **ciascun blocco può essere letto o scritto indipendentemente da tutti gli altri**.

-   `Dispositivi a caratteri`:
    -   Un dispositivo a caratteri **rilascia o accetta un flusso di caratteri, senza alcuna struttura a blocchi.**

    -   I dispositivi che ricadono i questa categoria sono: **stampanti, interfacce di rete, tastiere, mouse, ect..**

>**NOTA:** Alcuni dispositivi però non ricadono in questo schema.

Tipo **`i clock`**, **non sono indirizzabili a blocchi e non generano né accettano flussi di caratteri: tutto ciò che fanno è produrre interrupt a intervalli ben definiti.**

&nbsp;
&nbsp;
&nbsp;

**I dispositivi di I/O possono avere velocità molto diverse.**

### **`Velocità trasferimento dati di alcuni dispositivi tipici e bus.`**

| **Dispositivo**      	| **Velocità trasferimento dati** 	|
|----------------------	|---------------------------------	|
| Tastiera             	| 10 byte/s                       	|
| Mouse                	| 100 byte/s                      	|
| Modem a 56k          	| 7 KB/s                          	|
| Scanner              	| 1 MB/s                          	|
| Videocamera Digitale 	| 3,5 MB/s                        	|
| Disco Blu-Ray 4x     	| 18 MB/s                         	|
| 801.11n Wireless     	| 37,5 MB/s                       	|
| USB 2.0              	| 60 MB/s                         	|
| Firewire 800         	| 100 MB/s                        	|
| Gigabit Ethernet     	| 125 MB/s                        	|
| Disco SATA 3         	| 600 MB/s                        	|
| USB 3.0              	| 625 MB/s                        	|
| Bus SCSI Ultra 5     	| 640 MB/s                        	|
| Bus PCIe 3.0 1x      	| 985 MB/s                        	|
| Bus ThunderBolt 2    	| 2,5 GB/s                        	|
| Bus PCIe 3.0 16x     	| 32 GB/s                         	|

&nbsp;
&nbsp;
&nbsp;

*Controller dei dispositivi*
======

**I dispositivi di I/O consistono tipicamente di una componente meccanica e di una elettronica**.
Spesso è possibile separare queste due parti per **fornire una progettazione più mo­dulare e generale.** 

La componente elettronica è detta **controller del dispositivo (`device controller`)**.
 
La parte meccanica è il **dispositivo stesso** (Es: Un Disco Meccanico).

La scheda del controller presenta spesso un connettore in cui inserire un cavo che si collega al dispositivo stesso.

Molte industrie producono unità disco che **si connettono alle interfacce `SATA, SCSI, USB, ThunderBolt` o `FireWire` (`IEEE 1394`).**

L’interfaccia fra il controller e il dispositivo è generalmente di **livello veramente basso**.

Il lavoro del controller è **convertire il flusso seriale di bit in un blocco di byte ed eseguire ogni necessaria correzione di errore.** 

### **`Esempio di Controller in un HDD`**
![alt text](https://www.hddzone.com/blog/images/M5.jpg)

&nbsp;
&nbsp;
&nbsp;

*Input/output mappato in memoria*
======

**Ogni controller dispone di pochi registri usati per le comunicazioni con la CPU.**

Nello scrivere in questi registri **il sistema operativo comanda al dispositivo di inviare dati, accettarli**, porsi in stato di acceso o spento o eseguire qualche altra azione. 

**Leggendo da questi registri, il sistema operativo apprende quale sia lo stato del dispositivo**, se sia predisposto ad accettare un nuovo comando e così via.

Oltre ai registri di controllo, **molti dispositivi hanno un buffer di dati su cui il sistema operativo può scrivere e leggere**.

>**E.g:** **La RAM video (`VRAM`) è fondamentalmente un `buffer di dati`**, a disposizione dei programmi o del sistema operativo per lettura/scrittura, necessario per la visualizzazione.

La questione che sorge è come la CPU comunichi con i registri di controllo e i buffer dei dati del dispositivo.

Esistono due alternative: **I/O mappato su porte(`Port-mapped I/O`) e I/O mappato in memoria (`Memory-mapped I/O`)**

-   `I/O Mappato su Porte`:

    -   Usa lo stesso bus per indirizzare sia la memoria che i dispositivi di I/O, e le stesse istruzioni della CPU utilizzate per leggere e scrivere la memoria sono utilizzate anche per accedere ai dispositivi di I/O. 
    
    -   Per poter alloggiare i dispositivi di I/O, aree di spazio indirizzabile dalla CPU devono essere riservate per l'I/O invece che essere usate per la memoria. Questa suddivisione non deve necessariamente essere permanente, per esempio si può effettuare lo switch tra I/O e memoria. 
    
    -   I dispositivi di I/O controllano il bus indirizzi della CPU e rispondono ad ogni accesso allo spazio indirizzi a loro assegnato, mappando l'indirizzo nei loro registri hardware.

-   `I/O Mappato in Memoria`:

    -   Usa una speciale classe di istruzioni specifiche per l'esecuzione dell'input/output.

    -   I dispositivi di I/O hanno uno spazio indirizzi separato da quello della memoria, ottenuto tramite un extra "I/O" pin sull'interfaccia fisica della CPU oppure tramite un bus dedicato.

### **`Esempio di Port-mapped I/O e Memory-mapped I/O`**
![alt text](https://i.imgur.com/IqwtLX8.png)

&nbsp;
&nbsp;
&nbsp;

**I due schemi per indirizzare i controller presentano punti di forza e punti deboli diversi.**


### **`Vantaggi e Svantaggi`**
-   `I/O Mappato su Porte`:
    
    -   **Il port-mapped I/O separa l'accesso a ingressi/uscite dagli accessi alla memoria, tutto lo spazio di indirizzi può essere usato per la memoria.**

    -   **L'accesso all'I/O è ovvio anche per una persona che legga un programma scritto in assembly, dato che si utilizzano istruzioni speciali.**
    

-   `I/O Mappato in Memoria`:

    -   **La CPU richiede meno logica interna ed è di conseguenza più economica, veloce e facile da costruire.**

    -   **Non serve alcun meccanismo di protezione speciale per evitare che i processi utente eseguano l’I/O**.

    -   **Ogni istruzione che può referenziare della memoria può anche referenziare dei registri di controllo.**

&nbsp;
&nbsp;
&nbsp;



*Direct memory access (DMA)*
======

>**DEF:** Il DMA controller è un processore specializzato nel trasferimento dei dati tra dispositivi di I/O e memoria principale.

**Il DMA controller attua direttamente il trasferimento dati tra periferiche e memoria principale senza l’intervento del processore.**

*Viene usato da molti sistemi hardware come controller di unità a disco, schede grafiche, schede di rete e schede audio.*

A fronte di una richiesta di I/O, **il processore invia al DMA** controller:

-   **Tipo di operazione richiesta**

-   **Dispositivo di I/O**

-   **Indirizzo di memoria da cui iniziare a leggere/scrivere i dati**

-   **Numero di byte da leggere/scrivere**


Il DMA controller avvia l’operazione richiesta e trasferisce i dati da/verso la memoria.


-   Un blocco di memoria viene copiato da una periferica a un'altra. 

-   **Il distacco del bus dati dal processore per assegnarlo al controllo del DMA**, che questi utilizza **per il trasferimento dei dati** tra le due periferiche, **avviene tramite dei `bus switches` su richiesta del `DMAC`**. 

-   La CPU si limita a dare avvio al trasferimento rilasciando il bus dati, mentre **il trasferimento** vero e proprio **è svolto dal controller DMA** (**`DMAC`**). 

-   Completato il trasferimento, **il `DMAC` invia un interrupt al processore** per segnalare il **completamento dell’operazione richiesta.**

Il trasferimento tra DMA e I/O può avvenire in diversi modi:

-   `Burst Transfer`: 

    -   Prevede che, una volta iniziato il trasferimento, **il DMA mantenga il controllo del BUS** a discapito della CPU, **finché esso non è terminato.**
    
    -   **L'accesso al bus da parte della CPU resta negato durante tutto il trasferimento.**
    
    -   Ciò presuppone che **la periferica e la memoria consentano un trasferimento tanto veloce e duraturo** quanto necessita il DMA Controller.

-   `Cycle Stealing`: 

    -   **Il DMA esegue il trasferimento** di parole un solo ciclo completo alla volta, cioè **per ogni ciclo si interfaccia con la periferica ed esegue il trasferimento solo se è pronta**, **tramite un `handshaking`**.
    
    -   **Il tempo durante il quale alla CPU è interdetto l'accesso al bus dati è più frammentato.**

-   `Transparent/Hidden`: 

    -   **Il DMA occupa il BUS solo quando la CPU non ne ha bisogno.**
    
    -   Per far sì che ciò avvenga **il DMA sorveglia la CPU e inizia** un ciclo di bus solo **se l'istruzione in esecuzione nella CPU è abbastanza lunga da consentirlo e** se tale istruzione **non riguarda trasferimenti sul BUS**.


### **`Esempio di un operazione DMA`**

![alt text](https://mediaserver.pearsonitalia.it/mediaserver_uni/books/prod/2017/9788891901026_TANENBAUM/EPUB/ASSETS/images/05-04.png)

&nbsp;
&nbsp;
&nbsp;

# 5.2 Principi del software di I/O


*Obiettivi del Software di I/O*
====

Obiettivi
-   `Indipendenza dal dispositivo`:

    -   Il significato è che dovrebbe essere possibile scrivere programmi che possono accedere a qualsiasi dispositivo di I/O senza dover specificare in anticipo il dispositivo. 

    -   Sta al sistema operativo farsi carico dei problemi causati dal fatto che questi dispositivi siano in realtà diversi e necessitino di sequenze di comandi molto diversi per leggere o per scrivere.

-   `Notazione uniforme`:

    -   È strettamente correlato all’indipendenza dal dispositivo. 

    -   Il nome di un file o di un dispositivo dovrebbe essere semplicemente una stringa o un numero e non dipendere in alcun modo dal dispositivo. 

    -   In UNIX tutti i dischi possono essere integrati nella gerarchia del file system in modo arbitrario, così l’utente non ha bisogno di essere a conoscenza di quale nome corrisponde a quale dispositivo. 

-   `Gestione degli errori`:

    -   In generale gli errori andrebbero gestiti il più possibile a livello hardware.

    -   Se il controller scopre un errore di lettura dovrebbe provare esso stesso a correggerlo, se possibile; altrimenti dovrebbe essere il driver del dispositivo a correggerlo, probabilmente provando a rileggere il blocco.

    -   Solo quando i livelli più bassi non sono in grado di gestire il problema devono essere avvisati i livelli superiori.

    -   In molti casi il recupero dell’errore avviene ai livelli più bassi, senza che i livelli superiori sappiano dell’errore.

-   `Gestione di varie opzioni di trasferimento`:

    -   La maggior parte dell’I/O fisico è asincrono:

        -   La CPU inizia il trasferimento e smette per fare qualsiasi altra cosa finché non arriva un interrupt.

    -   I programmi utente sono molto più semplici da scrivere se le operazioni di I/O sono bloccanti

    -   Sta al sistema operativo fare in modo che operazioni che in realtà sono guidate dagli interrupt sembrino bloccanti al programma utente.

-   `Buffering`:

    -   Spesso i dati che escono da un dispositivo non possono essere memorizzati direttamente nella destinazione finale.

    -   Alcuni dispositivi hanno vincoli real-time stringenti (per esempio i dispositivi audio digitali), per cui i dati devono essere messi anticipatamente in un buffer di output per rendere indipendente la velocità con cui il buffer si riempie dalla velocità con cui si svuota, al fine di evitare il fenomeno detto buffer underrun.

    -   L’uso di un buffer richiede molte operazioni di copia e provoca spesso un impatto pesante sulle prestazioni dell’I/O.


-   `Prestazioni`:

    -   Alcuni dispositivi di I/O, come i dischi, possono essere usati da molti utenti contemporaneamente, causando cali di prestazioni.

    -   Presenza di Deadlock in dispositivi che possono eseguire solo un job alla volta (Es: Stampanti).

    -   Il sistema operativo deve essere in grado di gestire sia i dispositivi condivisi sia quelli dedicati in modo da evitare i problemi.

&nbsp;
&nbsp;
&nbsp;

*I/O Programmato*
====

**Il processore manda un comando di I/O e poi attende che l’operazione sia terminata**, testando lo stato del dispositivo con un loop `busy-wait` (`polling`).

**Efficiente solo se la velocità del dispositivo è paragonabile con quella della CPU.**

![alt text](https://i.imgur.com/K5PSmYy.png)

&nbsp;
&nbsp;
&nbsp;

*I/O Guidato da Interrupt*
====

**Il processore manda un comando di I/O; il processo viene sospeso.**

Quando l’I/O è terminato, **un interrupt segnala che i dati sono pronti e il processo può essere ripreso.**

**Nel frattempo, la CPU può mandare in esecuzione altri processi** o altri thread dello stesso processo.

>**E.g:** Consideriamo il caso di una stampa su una stampante che non ha buffer ma stampa un carattere dopo l’altro man mano che arrivano. La stampante può stampare 100 caratteri/s, ogni carattere impiega 10 ms. Ciò significa che dopo l’operazione di scrittura nel registro dei dati della stampante di ciascun carattere, la CPU si fermerà in un ciclo di inattività di 10 ms, in attesa che le sia consentito l’output del carattere successivo. Questo tempo è più che sufficiente per aeseguire qualche altro processo in quel lasso di tempo.


>**NOTA:** Per permettere alla CPU di fare qualcos’altro nell’attesa che la stampante sia pronta è possibile usare gli interrupt. Avvenuta la chiamata di sistema per stampare la stringa, il buffer è copiato nello spazio del kernel, come abbiamo mostrato prima, e il primo carattere è copiato nella stampante appena è in grado di accettarlo. A questo punto la stampante richiama lo scheduler e viene eseguito un altro processo. Il processo che ha richiesto la stampa della stringa è bloccato finché non è stampata l’intera stringa.

Vettore di interrupt: tabella che associa ad ogni interrupt l’indirizzo di una corrispondente routine di gestione.

**Gli interrupt vengono usati anche per indicare eccezioni (e.g., divisione per zero).**

&nbsp;
&nbsp;
&nbsp;

*I/O con DMA*
====

**Pensato per evitare I/O programmato (PIO) per grandi spostamenti di dati.**

>**NOTA:** D'altronde, usare la CPU per controllare registro di stato del controllore e trasferire pochi byte alla volta è uno spreco.

**Basato sull’idea di bypassare la CPU per trasferire dati direttamente tra I/O e memoria.**

Per effettuare un trasferimento la CPU invia al DMAC:

-   L’indirizzo di partenza dei blocchi/memoria da trasferire 

-   L’indirizzo di destinazione 

-   Il numero di byte da trasferire

-   La direzione del trasferimento

**Il DMA controller gestisce il trasferimento**, comunicando con il
controllore del dispositivo mentre la CPU effettua altre operazioni.

**Il DMA controller interrompe la CPU al termine del trasferimento.**

![alt text](https://i.imgur.com/gAakhu2.png)

&nbsp;
&nbsp;
&nbsp;

# 5.3 Livelli del software di I/O

Il software di I/O è generalmente organizzato in quattro livelli:

-   **Gestori degli interrupt**

-   **Driver di dispositi**

-   **Software del sistema operativo indipendente dai dispositivi**

-   **Software per l'I/O a livello utente**


Ciasuno dei quali ha una funzione ben definita da eseguire e un’interfaccia ben definita verso i livelli adiacenti.


&nbsp;
&nbsp;
&nbsp;

*Gestori degli interrupt*
====

Quando arriva un interrupt, la CPU deve mandare in esecuzione **l'`ISR `(`Interrupt Service Routine`)** predisposta ad hoc dal programmatore per quel particolare interrupt. 

Affinché il meccanismo dell'interruzione funzioni correttamente, è necessario che **tutte le azioni svolte dall'ISR siano trasparenti rispetto al programma interrotto**, cioè che al **termine venga ripristinato tutto come era prima dell'interrupt**, in altre parole **deve essere astratto il più possibile dal resto del S.O.**

Per fare ciò bisogna che **la CPU**, prima di mandare in esecuzione l'ISR, **faccia una commutazione di contesto**, ovvero salvi tutto quello che stava facendo (cioè il suo contesto attuale), **ed alla fine dell'ISR lo ripristini com'era.**

**L'interrupt va gestito rapidamente**, e quindi non va perso tempo per **salvare** tutto il contesto attuale (variabili, stato del programma, l'immagine sullo schermo ecc..) ma **solo quello che effettivamente verrà modificato dall'ISR.**

D'altra parte la CPU non sa esattamente che cosa modificherà l'ISR e quindi non può conoscere a priori cosa salvare. 

Quindi quando si verifica un'interruzione **l'hardware deve provvedere ad effettuare la commutazione** di almeno la seguente porzione del contesto:

-   **Il registro PC (`Program counter`)**
-   **Il registro che definisce lo stato dell'interrompibilità del processore**

Nei moderni sistemi **i gestori di interrupt sono divisi in due parti**:

-   **Gestori di primo livello** (`FLIH`, First-Level Interrupt Handler)
-   **Gestori di secondo livello** (`SLIH`, Second-Level Interrupt Handlers)

**I gestori di primo livello (`FLIH`)** funzionano nello stesso modo delle vecchie routine di interrupt. **In risposta ad un interrupt avviene una commutazione di contesto e il codice per gestire l'interrupt viene caricato in memoria ed eseguito**. 

**Il compito del `FLIH`, comunque**, non è quello di gestire l'interrupt, bensì quello **di schedulare la successiva esecuzione del gestore di secondo livello (`SLIH`)**, nonché quello **di tenere traccia e di memorizzare tutte le eventuali informazioni importanti che fossero disponibili soltanto nel momento in cui si verifica l'interrupt**.

**Il gestore `SLIH` rimane nella coda pronti del sistema operativo finché, quando si rende disponibile tempo macchina del processore, arriva il suo turno di esecuzione, e può essere eseguito il codice per gestire l'evento che ha innescato l'interrupt.**


&nbsp;
&nbsp;
&nbsp;

*Driver del Dispositivo*
====

Abbiamo visto che ciascun controller utilizza alcuni registri dei dispositivi per inviare comandi o leggere il proprio stato o entrambe le cose. 

**Il numero di questi registri e la natura dei comandi varia da dispositivo a dispositivo.**

>**E.g:** Un driver di un mouse deve accettare informazioni dal mouse che indichino di quanto si è mosso e quali pulsanti sono attualmente premuti. 

Diversamente 

>**E.g:** Un driver di un disco dovrà sapere tutto riguardo a settori, tracce, cilindri, spostamento della testina, motori, tempi di fermo delle testine e tutti gli altri meccanismi che consentono al disco di operare correttamente. 

**Questi driver ovviamente saranno molto ­diversi.**

Di conseguenza **ciascun dispositivo di I/O connesso al computer necessita di un certo codice specifico al suo controllo.** 

Questo codice, il **driver di dispositivo** (**`device driver`**), è solitamente scritto dal produttore del dispositivo stesso e rilasciato insieme a esso.

>**DEF:** **Un driver indica l'insieme di procedure software**, spesso scritte in `assembly` (in base alla piattaforma in uso), **che permette ad un sistema operativo di utilizzare un dispositivo hardware.**

Dato che **ogni sistema operativo necessita dei propri driver**, **i produttori generalmente forniscono driver per gran parte dei sistemi operativi più conosciuti (Windows, Linux, Mac OS)**

&nbsp;
&nbsp;
&nbsp;

Al di sopra dell'hardware essenziale (Dischi Interni, GPU e altro) si trovano **le `API di dispositivi secondari`** (interfacce per le memorie di massa, le fotocamere e altro), che **sono pertanto diversi dai driver di HW, anche se condividono una parte dello stack dei protocolli (USB, SATA e altro)**

**Il driver deve normalmente essere parte del kernel del sistema operativo,** nell’ottica di poter accedere all’hardware del dispositivo, il che significa accedere ai registri del controller. 

Effettivamente **è possibile costruire driver eseguiti nello spazio utente**, con chiamate di sistema per eseguire la lettura e la scrittura dei registri dei dispositivi. **Questa progettazione isola il kernel dai driver e i driver l’uno dall’altro**, **eliminando gran parte delle cause di crash del sistema** – driver difettosi che in un modo o nell’altro interferiscono con il kernel. **Questa direzione viene presa dai sistemi molti compatti che devono essere sempre funzionali (Sistemi Embedeed).**

>**NOTA:** Ecco perchè si evitano il più possibile AntiCheat a livello Kernel, dato che per funzionare, installano un driver a livello Kernel, ma se esso presenta codice non ottimizzato che potrebbe generare errori, essi si posso ripercursare sul Kernel stesso, creando rallentamenti, crash o peggio ancora, danni permanenti.

Tuttavia, dato che **la maggior parte degli altri sistemi operativi desktop richiede l’esecuzione dei driver nel kernel**, questo è il modello che considereremo.

Dato che i progettisti di ogni sistema operativo sanno che vi saranno installati dei pezzi di codice (i driver) scritti da terzi, **il sistema operativo deve avere un’architettura che consenta queste installazioni**. 

Ciò comporta l’esistenza di un **modello ben definito di quello che fa un driver e di come interagisce con il resto del sistema operativo.**

&nbsp;
&nbsp;
&nbsp;

I sistemi operativi solitamente classificano i driver all’interno di un ristretto numero di categorie:

-   **Dispositivi a blocchi (`block device`):**

    -   Dischi, contenenti molteplici blocchi di dati indirizzabili indipendentemente
    
-   **Dispositivi a carattere (`character device`):**

    -   Stampanti e tastiere, che generano o accettano un flusso di caratteri.

I driver di dispositivo si posizionano normalmente sotto il resto del sistema operativo, come illustrato nella figura sottostante.

![alt text](https://mediaserver.pearsonitalia.it/mediaserver_uni/books/prod/2017/9788891901026_TANENBAUM/EPUB/ASSETS/images/05-12.png)

**In alcuni sistemi il sistema operativo è un singolo programma binario contenente compilati al suo interno tutti i driver di cui ha bisogno.**

**Questo schema è stato per anni la norma dei sistemi UNIX, infatti si ricompilava semplicemente il kernel con il nuovo driver, per costruire un nuovo ­binario.**

**Oggi invece, la maggior parte dei sistemi operativi non adotta più questo esatto schema, data la grandissima varietà di dispositivi.** A partire dall’MS-DOS, andarono nella direzione in cui i driver erano caricati dinamicamente nel sistema durante l’esecuzione.

>**NOTA:** **Linux** ancora ad oggi **effetua un metodo ibrido**, **ovverò i Moduli Kernel (`DKMS`)**, **cioè dei moduli esterni al kernel**, ma chi si appoggiano ad esso per l'esecuzione, c**he contengono il sorgente per il driver per un specifico dispositivo o applicazione, che poi verrà compilato e eseguito.** Infatti i driver delle GPU Nvidia viene installato come modulo Kernel, mentre le GPU Intel e AMD hanno già il driver compilato in formato binario dentro il S.O

**Un driver di dispositivo ha molte funzioni, del tipo:**

-   Accettare richieste di scrittura e lettura astratte provenienti dal software indipendente dal dispositivo sopra di esso e vedere che vengano portate a termine.

-   Inizializzare il dispositivo. 

-   Gestire i requisiti di alimentazione

-   Gestire il registro degli eventi.

**Esiste inoltre un tipo di sistema chiamato “a caldo”** (**`hot pluggable system`**), **dove i dispositivi possono essere aggiunti o rimossi con il sistema funzionante.** Di conseguenza, **mentre un driver è occupato nella lettura di un dispositivo, il sistema può informarlo che l’utente ha rimosso improvvisamente quel dispositivo dal sistema.**

&nbsp;
&nbsp;
&nbsp;

*Software per l’I/O indipendente dal dispositivo*
====

**Sebbene parte del software per l’I/O sia specifico di un determinato dispositivo, altre parti sono indipendenti dal dispositivo stesso (`device independent`).**

Il limite esatto fra i driver e il software indipendente dal dispositivo dipende dal sistema e dal dispositivo, poiché **alcune funzioni che potrebbero essere svolte in modalità indipendente dal dispositivo sono effettivamente svolte dai driver, sia per efficienza sia per altre ragioni.**


### **`Funzioni del software per l’I/O indipendente dai dispositivi.`**

| Funzioni                                           	|
|-----------------------------------------------------	|
| Interfacciamento uniforme dei driver dei dispostivi 	|
| Buffering                                           	|
| Segnalazione degli errori                           	|
| Allocazione e rilascio dei dispositivi dedicati     	|
| Dimensione dei blocchi indipendente dai dispositivi 	|

&nbsp;
&nbsp;
&nbsp;



    Interfacciamento uniforme dei driver dei dispositivi

**Una questione fondamentale di un sistema operativo è come rendere tutti i dispositivi e i driver più o meno simili tra loro. **

Questo eviterebbe di scrivere un driver per vari dispositivi, anche se molto simili tra di loro (Dischi), evitando anche di modificare il S.O e appesantirlo.

Un aspetto della questione **è l’interfaccia fra i driver di dispositivo e il resto del sistema operativo.** 

Un esempio di ciò si può vedere nella figura sottostante.


### **`(a) Senza un’interfaccia dei driver standard. (b) Con un’interfaccia dei driver standard.`**

![alt text](https://mediaserver.pearsonitalia.it/mediaserver_uni/books/prod/2017/9788891901026_TANENBAUM/EPUB/ASSETS/images/05-14.png)

-   **Figura `(a)`:**

    -   **Vi è illustrata una situazione in cui ogni driver ha una diversa interfaccia verso il sistema operativo.** 

    -   **Quello che significa è che le funzioni dei driver a disposizione del sistema per essere chiamate sono diverse da driver a driver.**

    -   Potrebbe anche significare che le funzioni del kernel di cui il driver ha bisogno sono diverse da driver a driver. 

    -   Nel complesso significa che **l’interfacciamento di ogni nuovo driver richiede un grande sforzo di programmazione.**

-   **Figura `(b)`:**

    -   **Vi è illustrato un diverso modello in cui tutti i driver hanno la stessa interfaccia**.

    -   **È molto più semplice perciò inserire un nuovo driver**, facendo in modo che sia conforme alla loro interfaccia.

    -   Significa, inoltre, che chi scrive i driver conosce che cosa ci si aspetta da lui. 

    -   In pratica, **non tutti i dispositivi sono assolutamente identici, ma di solito vi è solo un ristretto numero di tipi di dispositivi e questi generalmente non cambiano.**



Il relativo funzionamento è descritto di seguito:

-   **Per ciascuna classe di dispositivi (`blocchi e caratteri`) il sistema operativo definisce un insieme di funzioni che il driver deve supportare:**

    -   **`Disco`**:

        -   `Lettura, scrittura, accensione, spegnimento, formattazione` e altro... 

Spesso il driver contiene una tabella con i puntatori a se stesso per queste funzioni. **Una volta caricato il driver, il sistema operativo registra l’indirizzo di questa tabella di puntatori di funzioni**, così quando ha bisogno di chiamarne una **può fare una chiamata indiretta tramite questa tabella.** **Questa tabella di puntatori di funzioni definisce l’interfaccia fra il driver e il resto del sistema operativo.** 

&nbsp;
&nbsp;
&nbsp;

Un altro aspetto dell’uniformità di un’interfaccia riguarda **la denominazione dei dispositivi di I/O.**

Il software indipendente dal dispositivo si prende cura di **mappare i nomi simbolici dei dispositivi nel driver adatto.**

>**E.g:** In UNIX un nome di dispositivo, come `/dev/disk0`, **specifica in modo univoco `l’i-node` per un file speciale** e questo i-node contiene il **`major device number`** (numero di dispositivo primario), **usato per localizzare il driver adeguato**. 

**L’i-node contiene anche il `minor device number`** (numero di dispositivo secondario), **passato come un parametro al driver al fine di specificare l’unità da leggere o da scrivere.** 

**Tutti i dispositivi hanno un numero di dispositivo primario e secondario e l’accesso a tutti i driver avviene selezionandoli usando il numero di dispositivo primario.**

Strettamente correlata alla questione dei nomi è la protezione. 

Come fa il sistema a **prevenire l’accesso di utenti a dispositivi a cui non sono autorizzati**? Sia in UNIX sia in Windows **i dispositivi appaiono nel file system con nomi di oggetti**, il che significa che le **regole di protezione abituali per i file si applicano anche ai dispositivi di I/O**. Essi posso essere modificati da un amministratore di sistema.

&nbsp;
&nbsp;
&nbsp;

    Buffering

**Livello dedicato a gestire letture successive di grandi blocchi di memoria e a fornire dati tutti insieme. Per lo stream di caratteri, ad esempio, un utente scrive troppo veloce sulla tastiera e non riuscirebbe a gestire l’input, senza un buffer.**

**Esso però può creare dei problemi.**

&nbsp;
&nbsp;
&nbsp;

    Segnalazione degli errori

**Gli errori sono molto più frequenti nel contesto dell’I/O** che in altri. **Quando accadono, il sistema operativo deve gestirli meglio che può.** **Molti errori sono specifici del dispositivo e devono essere gestiti tramite un driver appropriato**, ma la struttura per la loro gestione è indipendente dal dispositivo.


-   **`Classe di errori di programmazione`:**

    -   *Questi si verifica­no quando un processo richiede qualcosa di impossibile, come la scrittura su un dispositivo di input o la lettura da un dispositivo di output.*

-   **`Classe di errori effettivi di I/O`:**

    -   *Si verificano quando un tentativo di scrittura su un blocco danneggiato di un disco o provare a leggere da una videocamera che è stata spenta.* 
    
    -   **In questi casi sta al driver decidere che cosa fare:**
    
        -   **Se non sa che cosa fare, il problema passa alla parte di software indipendente dal dispositivo.**

-   **`Classe di errori di buffer`:**

    -   *Questi si verificano quando si fornire un indirizzo di buffer, parametri, un dispositivo errato e così via.*
    
    -   **L’azione da intraprendere** in questo caso è semplice:

        -   **Riportare un codice d’errore al chiamante**.

&nbsp;
&nbsp;
&nbsp;

>**NOTA:** Quello che questo software fa dipende dalla natura dell’errore e dall’ambiente.

-   **`Se è un semplice errore di lettura e c’è un utente interattivo disponibile:`**

    -   *Può visualizzare una finestra di dialogo con la richiesta all’utente sul da farsi*:
    
        -   **Le opzioni possono includere il riprovare un certo numero di volte, ignorare l’errore o terminare il processo chiamante.**

-   **`Se non vi è un utente disponibile:`**

    -   Probabilmente l’unica possibilità reale è **che la chiamata di sistema fallisca riportando un codice d’errore.**


**Alcuni errori tuttavia non possono essere gestiti in questo modo.** 

Per esempio, **potrebbe essere andata distrutta una struttura dati critica**, come la directory principale o la lista dei blocchi liberi. 

**In questo caso il sistema potrebbe dover visualizzare un messaggio d’errore e terminare.** (Windows Moment)

&nbsp;
&nbsp;
&nbsp;

    Allocazione e rilascio dei dispositivi dedicati

**Alcuni dispositivi**, come i masterizzatori di CD-ROM, **possono essere usati solo da un singolo processo in un determinato momento.** 

Sta al sistema operativo **esaminare le richieste per l’uso del dispositivo e accettarle o rifiutarle**, **se il dispositivo richiesto sia disponibile o meno**. 

Un modo semplice per gestire queste richieste è di **richiedere ai processi di fare la `open` direttamente su file speciali per i dispositivi.** La chiusura di tali dispositivi dedicati rilascia i file.

**Un tentativo di acquisizione di un dispositivo che non è disponibile blocca il chiamante invece che fallire.** **I processi bloccati sono messi in una coda.** **Prima o poi il dispositivo richiesto sarà disponibile e il primo processo della coda potrà acquisirlo e continuare l’esecuzione.**

&nbsp;
&nbsp;
&nbsp;

    Dimensione dei blocchi indipendente dal dispositivo

Dischi differenti possono avere settori di diverse dimensioni.

**Sta al software indipendente dal dispositivo nascondere questo aspetto e fornire una dimensione astratta di un blocco uniforme ai livelli più in alto (`User-Land`), cosi facendo anche i programmi in User-Land vedranno e lavoreranno con il blocco astratto, indipendente dalla dimensione del settore fisico.**


&nbsp;
&nbsp;
&nbsp;

*Software per l'I/O a livello utente*
====

Infine **il livello `User-Space I/O Software` mette a disposizione dei programmatori delle librerie collegate con i programmi utente e anche da interi programmi eseguiti al di fuori del kernel.**

**Le chiamate di sistema, incluse quelle per l’I/O, sono normalmente eseguite da procedure di libreria.** 

**Una chiamata classica è (linguaggio C): `count=write(fd, buffer,nbytes);`**

&nbsp;
&nbsp;
&nbsp;

# 5.4 Dischi

&nbsp;
&nbsp;
&nbsp;

*Hardware dei dischi*
====

Esistono molti tipi di dischi. 

**I più comuni sono i dischi rigidi magnetici (`HDD`)**, caratterizzati dal fatto che le operazioni di lettura e scrittura sono ugualmente veloci, rendendoli l’ideale come memoria secondaria (per la paginazione, i file system, e così via). 

**Array di questi dischi sono talvolta utilizzati per fornire memoria altamente affidabile.**

Diversi tipi di **dischi ottici (`DVD` e `Blu-ray`)** sono anch’essi importanti per la distribuzione di programmi, dati e filmati. 

Infine, i **dischi allo stato solido (`SSD`)** sono sempre più comuni, perché sono veloci e non contengono parti in movimento.

&nbsp;
&nbsp;
&nbsp;

*Dischi magnetici*
====

-   **I dischi sono suddivisi in `tracce`** concentriche **e `settori`, ogni settore è una fetta di disco.**

-   **I settori suddividono ogni traccia in** porzioni di circonferenza dette **`blocchi`** (o record fisici).

![alt text](https://i.imgur.com/OktmxjY.png)

-   **I dischi magnetici sono organizzati in `cilindri`, ciascuno contenente tante tracce quante sono le testine impilate verticalmente.**

-   **Il numero dei settori lungo la circonferenza va generalmente da `8` a `32` sui floppy disk, fino a parecchie centinaia sui dischi fissi.**

-   **Il numero di testine varia da `1` a `16`.**

-   **La suddivisione della superficie di un disco in tracce e settori viene detta `formattazione`.**

-   **I dischi magnetici consentono l'accesso diretto** in quanto è possibile posizionare direttamente la testina su un qualunque blocco senza dover leggere quelli precedenti.

-   **I dischi più vecchi hanno poca elettronica** e portano un semplice flusso di bit in serie. 

    -   Su questi dischi **il controller esegue la maggior parte del lavoro.**

    -   **Non vengono più usati.**

-   Sugli altri dischi **`IDE`** (*integrated drive electronics*) e **`SATA`** (*serial ATA*) invece

    -   **Contiene un microcontroller che svolge una parte considerevole del lavoro** e permette al controller reale di inviare un insieme di comandi ad alto livello. 
    
    -   **Il controller spesso fa la cache delle tracce, il rimappaggio dei blocchi difettosi e molto altro.**


![alt text](https://i.imgur.com/GGiZTJb.png)

### **`Parametri del floppy disk da 360 KB e per il disco fisso WD 3000 HLFS.`**


| **Parametro**                         	| **Floppy Disk IBM 360kb** 	| **Disco Fisso WD 3000 HLFS** 	|
|---------------------------------------	|---------------------------	|------------------------------	|
| Numero Cilindri                       	| 40                        	| 36.481                       	|
| Tracce per cilindro                   	| 2                         	| 255                          	|
| Settori per traccia                   	| 9                         	| 63 (media)                   	|
| Settori per disco                     	| 720                       	| 586.072.368                  	|
| Capacità del disco                    	| 360 KB                    	| 300 GB                       	|
| Tempo di ricerca (cilindri adiacenti) 	| 6 ms                      	| 0,7 ms                       	|
| Tempo di ricerca (situazione media)   	| 77 ms                     	| 4,3 ms                       	|
| Tempo di rotazione                    	| 200 ms                    	| 6 ms                         	|
| Tempo di stop/avvio del motore        	| 250 ms                    	| 1 ms                         	|
| Tempo di trasferire un settore        	| 22 ms                     	| 1 ns                         	|
|                                       	|                           	|                              	|


&nbsp;
&nbsp;
&nbsp;

*RAID*
====

Il `RAID`, (**`Redundant Array of Independent Disks`**) ovvero **insieme ridondante di dischi indipendenti**, (originariamente "Redundant Array of Inexpensive Disks", insieme ridondante di dischi economici), **è una tecnica di installazione raggruppata di diversi dischi rigidi in un computer che fa sì che gli stessi nel sistema appaiano e siano utilizzabili come se fossero un unico volume di memorizzazione.**

**Scopi del RAID sono:** 
-   Aumentare le performance
-   Rendere il sistema resiliente alla perdita di uno o più dischi
-   Poter rimpiazzare dischi senza interrompere il servizio. 

**Il RAID sfrutta**, con modalità differenti a seconda del tipo di realizzazione, **i principi di ridondanza dei dati e di parallelismo nel loro accesso** per garantire, rispetto ad un disco singolo, incrementi di prestazioni, aumenti nella capacità di memorizzazione disponibile, miglioramenti nella tolleranza ai guasti e quindi migliore affidabilità.

Le modalità più diffuse sono: 

>`RAID 0, 1, 5 e 10`. 

>**NOTA:** La `3` e la `4` sono state praticamente soppiantate dalla `5`. 

&nbsp;
&nbsp;
&nbsp;

### **`RAID di livello da 0 a 5. I dischi di backup e di parità sono rappresentati in grigio.`**


![alt text](https://mediaserver.pearsonitalia.it/mediaserver_uni/books/prod/2017/9788891901026_TANENBAUM/EPUB/ASSETS/images/05-20.png)

-   **`RAID 0: sezionamento senza ridondanza`:**

    -  **Descrizione:**

        -   Il sistema RAID 0 divide i dati equamente tra due o più dischi, tipicamente tramite sezionamento (o striping), ma senza mantenere alcuna informazione di parità o ridondanza che aumenti l'affidabilità (la dicitura RAID, ancorché diffusa, è pertanto impropria). 
        
        -   RAID 0 è usato generalmente per aumentare le prestazioni di un sistema, o per la comodità di usare un grande numero di piccoli dischi fisici come se fossero un piccolo numero di grandi dischi virtuali.


    -   **Vantaggi:**

        -   Costo di implementazione basso.

        -   Alte prestazioni in scrittura e lettura, grazie al parallelismo delle operazioni I/O dei dischi concatenati.


    -   **Svantaggi:**

        -   Impossibilità d'utilizzo di dischi hot-spare.

        -   Affidabilità drasticamente ridotta, anche rispetto a quella di un disco singolo: l'affidabilità di un sistema di n dischi con affidabilità media A è pari a A/n.

****
-   **`RAID 1: replicazione`:**

    -   **Descrizione:**

        -   Il sistema RAID 1, detta anche `mirror`, mantiene una copia esatta di tutti i dati su almeno due dischi. 
        
        -   È utile quando la ridondanza sia ritenuta un'esigenza più importante rispetto allo sfruttamento ottimale della capacità di stoccaggio dei dischi. 
        
        -   L'insieme, infatti, limita il suo volume a quello del disco di taglia inferiore. D'altro canto, visto che un sistema con n dischi è in grado di resistere alla rottura di n - 1 componenti, l'affidabilità aumenta linearmente al numero di dischi presenti.

    -   **Vantaggi:**

        -   Affidabilità, cioè resistenza ai guasti, che aumenta linearmente con il numero di copie.

        -   Velocità di lettura (in certe implementazioni e sotto certe condizioni).

    -   **Svantaggi:**

        -   Bassa scalabilità.

        -   Costi aumentati linearmente con il numero di copie.

        -   Velocità di scrittura ridotta a quella del disco più lento dell'insieme.

****
-   **`RAID 2: sezionamento a livello di bit`**

    -   **Descrizione:**

        -   Un sistema RAID 2 divide i dati al livello di bit (invece che di blocco) e usa un codice di Hamming per la correzione d'errore che permette di correggere errori su singoli bit e di rilevare errori doppi.
        
        -   Questi dischi sono sincronizzati dal controllore, in modo tale che la testina di ciascun disco sia nella stessa posizione in ogni disco. 
        
        -   Questo sistema si rivela molto efficiente in ambienti in cui si verificano numerosi errori di lettura o scrittura, ma al giorno d'oggi, data l'inclusione della correzione tramite codice di Hamming (ECC) direttamente nel controller del disco, il RAID 2 non viene utilizzato ed è considerato obsoleto.

****
-   **`RAID 3: sezionamento a livello di byte con disco di parità`**

    -   **Descrizione:**

        -   Un sistema RAID 3 usa una divisione al livello di byte con un disco dedicato alla parità

        -    Il RAID-3 è estremamente raro nella pratica

        -   Uno degli effetti collaterali del RAID-3 è che non può eseguire richieste multiple simultaneamente. 

            -   Questo perché ogni singolo blocco di dati ha la propria definizione diffusa tra tutti i dischi del RAID e risiederà nella stessa posizione, così ogni operazione di I/O richiede di usare tutti i dischi.

        -   

    -   **Vantaggi:**

        -   Ridondanza:

            -   In caso di guasto, si accede al disco di parità e i dati vengono ricostruiti. 
            
            -   Una volta che il disco guasto viene rimpiazzato, i dati mancanti possono essere ripristinati e l'operazione può riprendere. 
            
            -   La ricostruzione dei dati è piuttosto semplice.

****
-   **`RAID 4: sezionamento a livello di blocco con disco di parità`**

    -   **Descrizione:**

        -   Il sistema RAID 4 usa una divisione dei dati a livello di blocchi e mantiene su uno dei dischi i valori di parità, in maniera molto simile al RAID 3, dove la suddivisione è a livello di byte.

        -   Questo permette ad ogni disco appartenente al sistema di operare in maniera indipendente quando è richiesto un singolo blocco.

    -   **Vantaggi:**

        -   Resistenza ai guasti

        -   Letture veloci grazie al parallelismo della struttura.

        -   Possibilità di inserire dischi hot-spare.

    -   **Svantaggi:**

        -   Il disco utilizzato per la parità può costituire il collo di bottiglia del sistema

        -   Scrittura lenta a causa della modifica e del calcolo della parità (4 accessi a disco per ogni operazione I/O).

****
-   **`RAID 5: sezionamento a livello di blocco con parità distribuita`**

    -   **Descrizione:**

        -   Un sistema RAID 5 usa una suddivisione dei dati a livello di blocco, distribuendo i dati di parità uniformemente tra tutti i dischi che lo compongono.

        -   È una delle implementazioni più popolari, sia in software, sia in hardware, dove praticamente ogni dispositivo integrato di storage dispone del RAID-5 tra le sue opzioni.

    -   **Vantaggi:**

        -   La parità è distribuita e quindi non esiste il problema del disco collo di bottiglia come nel RAID 4, le scritture sono più veloci rispetto allo stesso RAID 4, perché il disco che nel RAID 4 è dedicato alla parità ora può essere utilizzato per le letture parallele.

    -   **Svantaggi:**

        -   Scritture lente a causa della modifica e del calcolo della parità (4 accessi a disco per ogni operazione I/O).

****
-   **`RAID 6: sezionamento a livello di blocco con doppia parità distribuita`**

    -   **Descrizione:**

        -   Un sistema RAID 6 usa una divisione a livello di blocchi con i dati di parità distribuiti due volte tra tutti i dischi.

        -   Non era presente tra i livelli RAID originari.

        -   Nel RAID-6, il blocco di parità viene generato e distribuito tra due stripe di parità, su due dischi separati, usando differenti stripe di parità nelle due direzioni.

        -   Il RAID-6 è più ridondante del RAID-5, ma è molto inefficiente quando viene usato in un numero limitato di dischi.

    -   **Vantaggi:**

        -   Altissima fault tolerance grazie alla doppia ridondanza.

    -   **Svantaggi:**

        -   Scritture molto lente a causa della modifica e del calcolo della parità (6 accessi a disco per ogni operazione I/O), necessari N+2 dischi, molto costoso economicamente a causa della ridondanza e della complessità del controller della struttura.

        -   Problema del Write Hole:

            -   Le write sui diversi dispositivi non sono atomiche nell'insieme: questo vuol dire che la mancanza di alimentazione durante una scrittura può portare alla perdita di dati.

&nbsp;
&nbsp;
&nbsp;


*Algoritmi di scheduling del braccio del disco*
====

Consideriamo quanto tempo serve per la lettura o la scrittura di un blocco del disco.

Il tempo richiesto è determinato da tre fattori:

-   Il tempo di ricerca (il tempo per muovere il braccio al giusto cilindro);

-   Il ritardo rotazionale (il tempo affinché il settore giusto ruoti sotto la testina);

-   Il tempo effettivo di trasferimento dei dati.

Per gestire le richieste di I/O ed eseguirle il più velocemente possibile vengono impiegati degli algoritmi per lo scheduling del disco.

Ecco un elenco dei più comuni: 
>**`FCFS`** (*First Come First Served*), **`SSTF`** (*Shortest Seek Time First*), **`SCAN`, `C-SCAN`, `LOOK`, `C-LOOK`,** e **`LIFO`**.
****

    FCFS

**Negli algoritmi FCFS le richieste di I/O del disco vengono eseguite in ordine di arrivo.**

>**E.g:** Se la coda di richieste dell'unità disco contiene i cilindri `23`, `184`, `120`, `45`, `89`, `90`, `1`, e la testina del disco si trova nel cilindro `46`, essa dovrà spostarsi al cilindro `23`, poi al succesivo `184`, e via via sino all'ultimo. **La distanza totale coperta dalla testine del disco risulta di 457 cilindri.**

![alt text](https://www.megaoverclock.it/HDSCHEDULINGFCFS.jpg)


&nbsp;
&nbsp;
&nbsp;
****

    SSTF

**L'algoritmo SSTF** o scheduling per brevità **esegue tutte le richieste di I/O della coda più prossime alla posizione corrente della testina prima di elaborarne altre**

**Per tale motivo esso è chiamato algoritmo di servizio secondo il più breve tempo di ricerca.**

Se utilizziamo l'esempio precedente
>La testina del disco dal cilindro `46` si sposterà al cilindro `45`, poi al cilindro `23` e cosi via fino al cilindro `184`. La coda verrà soddisfatta **nell'ordine seguente: `46`, `45`, `23`, `1`, `89`, `90`, `120`, `184`, per una distanza coperta totale di `228` cilindri** (v. figura).

![alt text](https://www.megaoverclock.it/HDSCHEDULINGSSTF.jpg)

&nbsp;
&nbsp;
&nbsp;
****

    SCAN

**L'algoritmo SCAN sposta la testina del disco da un estremo all'altro nella sola direzione possibile.**

In pratica le testine dell'unità disco lo attraversano nelle due direzioni. 

Nel nostro esempio pratico
>**Le richieste verrebberò così soddisfatte ammesso che le testine si stiano spostando verso il cilindro 0: `46`, `45`, `23`, `1`, giungendo fino al cilindro `0`**; **Successivamente le testine si sposteranno verso l'estremo opposto** servendo le richieste ai cilindri `89`, `90`, `120`, `184`. **Nel nostro esempio la distanza coperta totale è di `229` cilindri** 

>Se la testina del disco si muovesse verso l'ultimo cilindro le richieste verrebbero soddisfatte in quest'ordine: 46,89,90,120,184, poi, essa proseguirebbe fino al cilindro 200, e a questo punto invertirebbe la direzione del braccio, e verrebbero servite le richieste ai cilindri 45,23,1, per un totale di 353 cilindri(v. figura). 

![alt text](https://www.megaoverclock.it/HDSCHEDULINGSCAN1.jpg)

&nbsp;
&nbsp;
&nbsp;
****

    C-SCAN

**L'algoritmo C-SCAN (circular SCAN) rappresenta una variante del precedente algoritmo SCAN.**

Ma vediamo come funziona: 
-   **La testina dell'unità disco si sposta nelle due direzioni, ma solo in una direzione serve le richieste I/O;** 

Nel nostro esempio
>**la coda di richieste I/O dei cilindri** `46`, `23`, `184`, `120`, `45`, `89`, `90`, `1` **è così elaborata:** `46`,`89`,`90`,`120`,`184`, **e poi la testina prosegue fino al `200` cilindro;** 

>**Successivamente la testina ritorna al cilindro `0`**, ed elabora le richieste al cilindro `1`,`23`,`45` **per un totale di `399` cilindri**.

![alt text](https://www.megaoverclock.it/HDSCHEDULINGCSCAN.jpg)

&nbsp;
&nbsp;
&nbsp;
****

    LOOK

**L'algortimo SCAN con una piccola variazione possono modificare il loro meccanismo di risposta alle richieste I/O, e diventare rispettivamente la variante `LOOK`**. 

Quest'ultima **"guarda" se sono presenti altre richieste dopo l'ultima richiesta I/O soddisfatta nella direzione di marcia corrente, nel qual caso le servono, oppure in caso contrario invertono la direzione.**

**Il numero totale di cilindri coperti è `321`.**

![alt text](https://www.megaoverclock.it/HDSCHEDULINGLOOK.jpg)

&nbsp;
&nbsp;
&nbsp;
****

    C-LOOK

**Mentre, la variante `C-LOOK**` di C-SCAN dovrebbe funzionare come mostrato in figura. 

**Il numero totale di cilindri percorsi è `367`.**

![alt text](https://www.megaoverclock.it/HDSCHEDULINGCLOOK.jpg)

&nbsp;
&nbsp;
&nbsp;
****

    LIFO

**L'algoritmo `LIFO` (Last In First Out) consiste nel servire le richieste di I/O in ordine inverso di arrivo.**

Nel nostro caso
>la coda di richieste I/O dei cilindri `46`, `23`, `184`, `120`, `45`, `89`, `90`, `1` **verrebbe servita così: `1`,`90`,`89`,`45`, `120`,`184`,`23**` come mostrato nel grafico seguente. **La distanza coperta totale è di `479` cilindri.**

![alt text](https://www.megaoverclock.it/HDSCHEDULINGLIFO.jpg)

&nbsp;
&nbsp;
&nbsp;

# 5.5 I clock

**I clock (`timer`) sono fondamentali ai fini del funzionamento di un sistema multiprogrammato per molte ragioni.**

**Mantengono l’orario del giorno e, fra le altre cose, evitano che un processo possa monopolizzare la CPU.**

**Il software del clock può presentarsi sotto forma di driver di dispositivo, sebbene il clock non sia né un dispositivo a blocchi**, come un disco, **né un dispositivo a caratteri**, come un mouse. 

&nbsp;
&nbsp;
&nbsp;

*Hardware del clock*
====

**Nei computer sono utilizzati comunemente due tipi di clock.**

**I clock più semplici sono connessi a un’alimentazione a 110volt o a 220 volt e provocano un interrupt ogni ciclo di voltaggio, a 50 o 60 Hz.**

**Questi clock erano i più diffusi, ma oggi sono rari.**

**L’altro tipo di clock è composto da tre componenti:**

-   Un oscillatore a cristalli

-   Un contatore

-   Un registro.

Quando **un pezzo di quarzo è tagliato in modo appropriato e messo in tensione, può iniziare a generare un segnale periodico con estrema precisione**, generalmente nell’**intervallo di parecchie centinaia di megahertz**, a seconda del cristallo scelto. 

Tramite apparecchiature elettroniche **questo segnale base può essere moltiplicato per un numero intero piccolo fino a ottenere frequenze di 1000 MHz e oltre**.

Questo segnale è dato in pasto al **contatore, per fare un conto alla rovescia sino a zero.**

Una volta raggiunto lo **zero, causa un interrupt di CPU.**


### **`Un clock programmabile.`**

![alt text](https://mediaserver.pearsonitalia.it/mediaserver_uni/books/prod/2017/9788891901026_TANENBAUM/EPUB/ASSETS/images/05-32.png)

**I clock programmabili hanno di norma più modalità per operare.** 

-   **`Modalità one-shot`:**

    -   Quando il clock parte, copia il valore del registro nel contatore e poi lo decrementa a ogni pulsazione del cristallo. 
    
    -   Quando il contatore raggiunge lo zero causa un interrupt e si ferma finché non è esplicitamente avviato dal software. 

-   **`Modalità onda quadra (square-wave`):**

    -   Dopo aver raggiunto zero e aver causato l’interrupt, il registro è copiato automaticamente nel contatore e l’intero processo viene nuovamente ripetuto a tempo indeterminato. 
    
    -   Questi interrupt periodici sono detti tick del clock.

**Il vantaggio del clock programmabile è che la sua frequenza di clock è controllabile via software.**

Per evitare che l’ora corrente possa perdersi quando viene spento il computer, la maggior parte dei computer dispone di un **clock di backup alimentato da una batteria**, implementato con il tipo di circuiti a basso assorbimento tipici degli orologi digitali. 

**L’orologio della batteria può essere letto all’avvio.**

**Se l’orologio di backup non è presente il software può richiedere all’utente data e ora**.

**L’ora è tradotta in numero di tick del clock a partire dalle 12:00 a.m. UTC del 1° gennaio 1970, come fa UNIX**. **Il punto di partenza in Windows è il 1° gennaio 1980.**

**A ogni scatto del clock il tempo reale è incrementato di un conteggio.** 

Per impostare manualmente il clock di sistema e il clock di backup e per sincronizzarli generalmente sono fornite delle utility.

&nbsp;
&nbsp;
&nbsp;

*Software del clock*
====

**Tutto quello che l’hardware del clock fa è generare interrupt a intervalli ben precisi.** 

**Il resto avviene tramite il software, il driver del clock.** 

**I compiti precisi del driver del clock** cambiano a seconda dei sistemi operativi; i principali **consistono nel:**

-   *Mantenere l’orario e la data del giorno.*

-   *Evitare che i processi siano eseguiti per più di quanto sia loro consentito.*

-   *Contabilizzare l’uso della CPU.*

-   *Gestire la chiamata di sistema* **`valarm`** *eseguita dai processi utente.*

-   *Fornire timer watchdog per parti del sistema stesso.*

-   *Eseguire profiling, monitoraggio ed elaborazioni statistiche.*

&nbsp;
&nbsp;
&nbsp;

****

**`La prima funzione del clock`, mantenere data e ora del giorno (cioè il real-time – tempo reale) non è difficile.**

**Richiede semplicemente l’incremento del contatore a ogni tick del clock**, come detto prima. 

**La sola cosa da controllare è il numero di bit nel contatore “data e ora”.** 

Con una frequenza di clock di 60 Hz, il contatore a 32 bit va in overflow nel giro di circa 2 anni. 

È ovvio che il sistema non può memorizzare in 32 bit il tempo reale come il numero di tick a partire dal 1° gennaio 1970.

**I sistemi per risolvere il problema sono tre:**

-   **`Il primo:`**

    -   è usare un contatore a 64bit.

    -   Seb­bene ciò renda il mantenimento del contatore più costoso, dato che bisogna provvedervi molte volte al secondo. 
    
-   **`Il secondo:`**

    -   è mantenere il tempo in secondi invece che in tick usando un contatore parallelo per contare i tick fino al compimento di un secondo. 
    
    -   Dato che 232 secondi sono più di 136 anni, questo metodo sarebbe valido fino al XXII secolo.

-   **`Il terzo:`**

    -   è quello di contare in tick, ma in relazione al momento in cui il com­puter è stato acceso, invece che a un momento esterno prefissato. 
    
    -   Quando viene letto l’orologio di backup o l’utente digita il tempo reale, il tempo d’avvio del sistema è calcolato dall’ora attuale e archiviato in memoria in modo opportuno. 
    
    -   Più avanti, quando viene richiesto l’orario, l’ora del giorno archiviata è aggiunta al contatore per ottenere l’orario attuale. 
    
**Tutti e tre gli approcci sono mostrati nella figura sottostante.**


### **`Tre metodi per gestire l’ora del giorno.`**

![alt text](https://mediaserver.pearsonitalia.it/mediaserver_uni/books/prod/2017/9788891901026_TANENBAUM/EPUB/ASSETS/images/05-33.png)

****
**`La seconda funzione del clock` è evitare che i processi siano eseguiti troppo a lungo.** 

**In qualunque momento un processo sia avviato, lo scheduler inizializza un contatore con il valore di quel quantum di processo, in colpi di clock**. 

**Quando arriva a zero, il driver del clock richiama lo scheduler per impostare un altro processo.**

&nbsp;
&nbsp;
&nbsp;

****
**`La terza funzione del clock` è fare l’accreditamento della CPU, ossia conteggiare l’uso della CPU. **

**Il metodo più preciso per farlo è far partire un secondo timer, distinto dal timer del sistema principale, ogni qualvolta è avviato un processo.**

**Quando quel processo si ferma, il timer può esser letto per vedere per quanto è stato eseguito.**

**Per fare le cose giuste bisognerebbe salvare questo secondo timer a ogni interrupt e ripristinarlo in seguito.**


### **`Simulazione di molteplici timer con un singolo clock.`**

![alt text](https://mediaserver.pearsonitalia.it/mediaserver_uni/books/prod/2017/9788891901026_TANENBAUM/EPUB/ASSETS/images/05-34.png)
