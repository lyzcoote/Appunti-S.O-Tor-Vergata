# Capitolo 1

# 1.5 Concetti base di Sistemi Operativi
La maggior parte dei sistemi operativi presenta alcuni concetti di base e astrazioni, quali sono **processi, spazi degli indirizzi e file**.
Essi sono fondamentali per la comprensione del funzionamento del S.O.

# 1.5.1 I Processi
Un concetto chiave di ogni S.O è quello del **processo**.

>**DEF:** Un processo *è un programma in esecuzione* che funge anche da ***contenitore** che raccoglie tutte le informazioni necessarie per far girare quel programma*

Associato a ogni processo c'è il suo **spazio degli indirizzi**, *un elenco di locazioni di memoria da 0 a un massimo*, che il processo può leggere e scrivere.

Lo spazio degli indirizzi contiene **il programma eseguibile, i dati del programma e il suo stack**

In molti S.O, *tutta l'informazione riguardante ciascun processo*, diversa dai contenuti del suo spazio degli indirizzi, *è salvata in una tabella del S.O*, chiamata **tabella del processo**, **che è un array di strutture**, una per ogni processo in essere.

Un processo (sospeso) consiste quindi del suo spazio degli indirizz, generalmente chiamato **Immagine Core** e del suo elemento nella tabella di processo, *insieme al contenuto dei suoi registri e a molti altri oggetti necessari per avviare il processo successivamente*.

Le chiamate chiave del sistema di gestione del processo sono quelle che hanno a che fare con la creazione e chiusura dei processi.

>**E.g:** 
Un processo chiamato **interprete dei comandi** o **shell** legge i comandi a un terminale. L'utente ha appena digitato un comando con cui richiede la compilaizone di un programma. Quando quel processo ha terminato il suo lavoro, effetuerà una **chiamata di sistema** o **System Call** per terminare se stesso.

Quindi, se un processo può creare uno o più processi (chiamati ***processi figli***) e questi processi a loro volta possono creare dei processi figli (haha, process inception 😂💀), arriviamo a creare ***una struttura ad albero***. 
I processi relazionati che cooperano per realizzare uno specifico lavoro (E.g: Un videogioco) *necessitano di comunicare l'uno con l'altro e sincronizzare le loro attività/dati*.

Questa comunicazione è chiamata **Comunicazione tra processi** (***I**nter***P***rocess **C**ommunication*) aka **IPC**

Un processo ha a disposizione altre System Calls ***per richiedere più memoria** (o rilasciare memoria non più usata/necessaria), **attendere un processo figlio** che viene terminato o sovrapporre il suo programma a un altro*

![alt text](https://i.imgur.com/dFJ930m.png)

Inoltre, *ad ogni persona autorizzata* a usare il sistema è assegnato un **UID** (***U**ser **ID**entification*) dall'amministratore di sistema.

***Ogni processo che parte ha l'UID della persona che lo fa partire***.

***Un processo figlio ha lo stesso UID del padre***.

Gli utenti posso appartenere a dei gruppi, ognuno dei quali ha lo stesso **GID** (***G**roup **ID**entification*)

Un preciso UID, chiamato **Superuser** (in UNIX uwu) o **Administrator** in Windows, ha un potere speciale e può violare molte regole della protezione.

&nbsp;
# 1.5.2 Spazi degli Indirizzi
Ogni computer ha una memoria principale impiegata per il mantenimento dei programmi in esecuzione.

Gli S.O più sofisticati consentono a più programmi di essere in memoria allo stesso tempo.

Per evitare che un processo interferisca con l'altro (e con l'S.O) (E.g: Il ***DeadLock***), sono stati implementati *diversi metodi*.

Anche se questi metodi di controllo *dovrebberò risiedere nell'hardware, essi vengono contrallati dall'S.O*.

Inoltre, un problema che affligeva i vecchi calcolatori era **l'uso, la gestione e la protezione della memoria.**
>**E.g:** Se un processo doveva **allocare uno spazio degli indirizzi equivalente a 2^32** (4 Gigabyte) ma il **calcolatore aveva a disposizione solo 4 Gigabyte di Memoria** (RAM), si sarebbe andati in contro a una **situazione di Deadlock**, dato che ***non sarebbe più rimasto spazio in memoria*** per gli altri processi e funzioni fondamentali del S.O

Nei nuovi calcolatori esiste una tecnica denominata **Memoria Virtuale** (in UNIX chiamata **Swap**) che essenzialmente consente al S.O di *prendere una parte dello spazio degli indirzzi in memoria* e *spostarli su un'unità di memorizzazione fisica* e ***fissa*** (E.g: SSD o HDD), cosi facendo **liberando memoria**. E quando un processo deve accedere a quello spazio di memoria, *il S.O prende quella parte di spazio degli indirizzi e la ricarica in memoria*.

Questa tecnica però ha un grave **punto dolente**, ovverò che ***in base al dispositivo di memorizazzione*** (E.g: *Un HDD invece che un SSD*), **il processo di ricaricare quella parte degli indirizzi può richiedere un gran tempo**, portando **latenza e rallentamenti**.

&nbsp;
# 1.5.3 File
Un altro concetto fondamentale di un S.O è il **File System**.

Ovvero quella parte del S.O che
>Nasconde tutte le *informazioni e dati tecnici dei dischi e dispostivi di I/O* e *li presenta al programmatore/utente* in una **forma astratta, gradevole e pulita** in modo da *facilitarne l'uso* (E.g: Cancellare, creare e modificare un documento di testo)

Ovviamente sotto la scocca **sono necessarie delle System Calls** che *eseguono le operazioni richieste*.

Per tenere traccia di tutti i file scritti e presenti sul disco, è necessario un *sistema di raggruppamento*, che in un S.O è espresso sottoforma di **Directories** (**Cartelle**)

***Esempio di Cartelle e sottocartelle con file all'interno***
![alt text](https://i.imgur.com/93G5tht.png)

Ogni directory inoltre* può essere specificata* tramite il **path name** (**nome di percorso**)

***Esempio di Path Name***:
>`C:\Users\fois2\Desktop\Uni\Libri Uni`

&nbsp;

>***NOTA:*** In UNIX il simbolo per separare le diverse directory nel path name è il seguente:  ***`/`***

>Mentre su Windows viene usato il simbolo seguente:  ***`\`***


&nbsp;
&nbsp;
&nbsp;

Ogni directory **è figlia della directory di partenza**, chiamata **root directory (cartella principale)**

***Esempio di Root Directory e le sue Sub Directory in Windows***
    
    C:
        -Programmi
            - *tutte le cartelle di ogni programma installato*
                - *tutti i file di ogni programma installato*
        -Utenti
            -fois2
                -Desktop
                    - *tutti i file presenti nel desktop*
                -Documenti
                    - *tutti i documenti creati dall'Utente*
                - *Altre cartelle*
        -Windows
            - *tutte le cartelle dove risiedono i file essenziali per il funzionamento del S.O*
            - *altri file essenziali*


&nbsp;
&nbsp;
&nbsp;

Prima che un file possa essere letto, deve essere **localizzato sul disco e aperto**, **effettuare** (*se sono presenti*) **le modifiche** e alla fine occorre **chiuderlo**.

Ma prima della scrittura di eventuali modifiche, bisogna controllare i suoi **permessi**.

Se è consentito l'accesso allora il S.O restituisce un piccolo numero intero chiamato **Descrittore di File (file descriptor)** da usare nelle operazioni di modifica.

Ma **se l'accesso viene proibito**, viene restituito un **codice d'errore** e **nessuna modifica** viene apportata a quel file. 


&nbsp;
&nbsp;
&nbsp;


In UNIX a differenza di Windows, un **dispostivo di memorizazzione esterno**, come può essere un Floppy, CD, DVD, USB e HDD/SSD Esterni, **non viene automaticamente messo a disposizione dell'Utente/Programmatore**.

>***NOTA:*** Nei Sistemi **Linux con una GUI**, questi dispositivi di memorizzazione esterni **vengono automaticamente messi a disposizione** dell'Utente/Programmatore, mentre nei sistemi **solo CLI** (***C**omand **L**ine **I**nterface*) **rimane il funzionamento come sui sistemi UNIX**

Per fare in modo che questi dispotivi *vengano messi a disposizione dell'Utente/Programmatore*, bisogna prima fare il loro **Mount**.

>***DEF:*** Per **Mount** si definisce un metodo per fornire un'elegante modalità di gestione di questi supporti rimovibili.

UNIX consente al *file system presente del dispostivo removibile* **di connettersi** a*l file system del sistema*.

&nbsp;
&nbsp;
&nbsp;
>Esempio **prima del Mount**

![alt text](https://i.imgur.com/bbnGaAH.png)

&nbsp;
&nbsp;
&nbsp;
>Esempio **dopo il Mount**

![alt text](https://i.imgur.com/f5KFqno.png)

&nbsp;
&nbsp;
&nbsp;

Un altro concetto importante in UNIX sono i **file speciali**.
Questi sono derivati da una filosofia UNIX che cita:
>`Tutto è un file`

>`Everything is a file`

&nbsp;
&nbsp;
&nbsp;

>***DEF***: I file speciali sono pensati per far sì che i dispositivi di I/O siano visti come file. Questo consente che essi vengano letti e scritti con le stesse chiamate di sistema usate per leggere e scrivere i file.

Essi sono contenuti nella directory `/dev`
>***E.g:*** Nella directory `/dev/fd0` è presente il Link Simbolico che rimanda al Floppy Drive

&nbsp;
&nbsp;
&nbsp;

Un ultimo concetto riguardante sia i processi che i file sono le **pipe**

>***DEF:*** Per **pipe** si può intendere una specie di pseudofile che può essere usato per connettere due processi.

>***E.g:*** Esistono due processi denominati **A** e **B**. Essi posso leggere i dati dalla pipe che se fosse un file di input e vicerversa.


&nbsp;
&nbsp;
&nbsp;
&nbsp;
&nbsp;
&nbsp;

# 1.5.6 La Shell

>***DEF:*** La **shell** è un componente fondamentale di un sistema operativo che **permette all'utente di impartire comandi** (E.g: richiedere l'avvio di altri processi) poi eseguiti dal S.O.

In UNIX esistono molte shell, fra cui *sh, csh, ksh, zsh e **Bash***, *la più usata* nei sistemi moderni UNIX based.

La Shell **prende comandi da dispositivi di input** (principalmente la tastiera) e **fa vedere l'output principalmente a uno schermo o a altri dispositivi**.

Una volta avviata la shell, viene mostrato un **prompt**, ovvero un *carattere simbolico* (di solito indicato da `$` nella shell Bash) che informa l'utente che **la shell è pronta a ricevere e eseguire comandi**

Eseguendo un qualsiasi comando, per esempio `date`, la **shell crea un processo figlio che eseguirà il comando `date`**, mentre il comando viene eseguito, **la shell viene messa in pausa** affinchè il **processo figlio verrà terminato**. *Appena questo accade, verrà fatto vedere a schermo il prompt e cosi via.*

Negli S.O dotati di ***GUI***, la **shell** è **disponibile** nell'applicazione grafica **Terminale.**

&nbsp;
&nbsp;
&nbsp;

# 1.6 Chiamate di Sistema

Abbiamo già visto che i sistemi operativi hanno due funzioni principali: **Fornire astrazioni ai programmi utente** e **gestire le risorse del computer**

Etrambe queste funzioni vengono eseguite automaticamente dal S.O senza intervento dell'Utente tramite le **Chiamate di Sistema (System Calls)**.

>***DEF***: Da come indica il nome, le **chiamate di sistema** sono delle *chiamate effetuate dall'Utente/Programmatore/Processo* che *indicano il meccanismo usato da esso per richiedere un servizio/dato* (E.g: La cancellazione/lettura di un certo file) e vengono eseguite direttamente dal Kernel del S.O

Esse vengono implementate in programmi/compilatori scritti in linguaggio **Assembly (di solito nei compilatori) e nel C.**


***Esempio di un codice in C che usa una chiamata di sistema UNIX***
>`int fd = open("foo.txt", O_RDONLY | O_CREAT);`

Il codice scritto sopra indica che viene creata una variabile di tipo `int` chiamata `fd` e gli viene assegnata la **syscall** (*abbreviazione di System Call*) `open()` che **apre un file** chiamato `foo.txt` **per leggerne il contenuto** senza scrivere al suo interno (notare bene il *`O_RDONLY` che definisce che il file deve solo letto*).

Se esso **non viene trovato, viene creato** (notare bene il *`O_CREAT` che definisce che deve essere creato se esso non esiste nella stessa directory dove viene eseguito il codice C*)

Se la chiamata di sistema **non può essere effettuata** (*sia per un parametro errato o errore su disco o altre cause*), **viene settato numero d'errore** che viene inserito in una variabile globale chiamata `errno`.

 Questo segnalatore di errori però **da solo non fa nulla**, sarà poi compito del programmatore di *implentarlo* in una funzione di ***handling*** (`Exception Handler`).


&nbsp;
&nbsp;
&nbsp;

Tipi di chiamate di Sistema
======

Le System Calls possono essere classificate in diverse categorie, al riguardo sono stati definiti in particolare i seguenti tipi di classificazione:

1. **Gestione dei file**:
    
    I programmi applicativi richiedono chiamate di sistema di questo tipo per **ottenere l’accesso alle tipiche operazioni sui file** come leggere, creare o modificare un file.

2. **Gestione e Comunicazione dei processi**:

    Tutti i processi di un S.O devono essere **controllati e devono comunicare tra di loro**, affinché possano essere interrotti in qualsiasi momento o essere pilotati da altri processi. 
    
    A tal fine le System Calls di questa categoria controllano ad esempio l’avvio o l’esecuzione oppure lo stop o l’interruzione dei processi e la creazione dei loro figli.

&nbsp;
&nbsp;
&nbsp;

# 1.6.1.1 Chiamate di Sistema di gestione dei file

Funzionamento Tecnico durante una chiamata di sistema
======

    Preparazione 

Si prenda sempre come esempio il seguente codice in C:

>`count = read(fd, buffer, nbytes);`

Come preparazione, il programma di chiamata prima **mette i parametri** (***`fd, buffer, nbytes`***) **nello stack** dei registri della CPU.

Dopodichè, il compilatore C **mette i parametri nello stack in ordine inverso**.

Il secondo parametro **verrà passato come riferimento**, *cioè viene passato l'indirizzo di memoria del buffer, ma non il suo contenuto*.

    Chiamata alla procedura

Viene **effettuata la chiamata alla procedura di libreria** (il `read`) e ne viene **messo il suo codice** (di solito scritto in Assembly) **in un registro**.

    Chiamata TRAP

Viene eseguita un istruzione `TRAP` **che passa dalla usermode** (modalità utente, dove si ha più restrizioni per garantire più sicurezza) **alla kernel mode** (modalità kernel, dove vengono bypassate tutte le restrizioni) e effettua un **esecuzione a un indirizzo fisso** all'interno del kernel.

    Esecuzione nel Kernel

Il codice del Kernel che parte in seguito all'istruzione `TRAP` **esanima il numero della chiamata di sistema** (come una sorta di ID) e poi **indirizza al corretto gestore** di chiamate di sistema tramite una tabella di puntatori ai gestori.

    Esecuzione gestore delle chiamate

A questo punto **viene eseguito il gestore** della chiamate di sistema.

    Ritorno in User Mode

Una volta che il gestore delle chiamate di sistema ha finito, **viene effettuata un altra `TRAP`, che effettua il ritorno in User Mode.**

    Ritorno al chiamante

Dopo essere tornata in User Mode, la procedura **ritorna al programma chiamante** (nel nostro caso, il codice in C).

    Conclusione e Pulizia

Per concludere il lavoro, il programma (sempre il nostro codice in C) **deve pulire lo stack** che ha precedentemente usato.

&nbsp;
&nbsp;
&nbsp;

# 1.6.1.2 Chiamate di sistema della gestione dei processi

Tabella con tutti i processi più usati con descrizione e esempio in codice C

| Tipo di Sys Call    | Funzione                                            | In Linux  | in Windows            | Esempio con codice                             |
|---------------------|-----------------------------------------------------|-----------|-----------------------|------------------------------------------------|
| Controllo Processo  | Crea un processo                                    | fork()    | CreateProcess()       | process1 = fork()                              |
| Controllo Processo  | Termina un processo                                 | exit()    | ExitProcess()         | exit(statusCode)                               |
| Controllo Processo  | Attende che un processo termini                     | waitpid() | WaitForSingleObject() | statProcess1 = waitpid(pid, &statloc, options) |
| Gestione File       | Crea/Apre un file                                   | open()    | CreateFile()          | file1 = open(file, O_CREAT, O_RDWR)            |
| Gestione File       | Legge un file                                       | read()    | ReadFile()            | fileData = read(file, buffer, nbytes)          |
| Gestione File       | Scrive in un file                                   | write()   | WriteFile()           | writeData = write(file, buffer, nbytes)        |
| Gestione File       | Chiude un file                                      | close()   | CloseHandle()         | file1 = close(file)                            |
| Gestione File       | Ottiene informazione sullo stato di un file         | stat()    | GetFileInformationByHandle() | fileStatus = stat(name, &buffer)               |
| Gestione Directory  | Crea una nuova Directory                            | mkdir()   | CreateDirectory() | newDir = mkdir(name, mode)                     |
| Gestione Directory  | Rimuove una Directory vuota                         | rmdir()   | RemoveDirectory() | remDir = rmdir(name)                           |
| Gestione Filesystem | Crea un link simbolico che punta al primo parametro | link()    | CreateSymbolicLinkA() | var = link(oldLink, newLink)                   |
| Gestione Filesystem | Rimuove un link simbolico                           | unlink()  |  | var = unlink(newLink)                          |
| Gestione Filesystem | Monta un dispostivo                                 | mount()   | SetVolumeMountPointA() | var = mount(devPath, name, flag)               |
| Gestione Filesystem | Smonta un dispostivo                                | unmount() | DeleteVolumeMountPointW() | var = unmount(devPath)                         |
| Gestione Directory  | Cambia la directory corrente                        | chdir()   | SetCurrentDirectory() | var = chdir(dirPath)                           |
| Gestione Directory  | Cambia i bit di protezione di un file               | chmod()   | SetFileSecurity() | var = chmod(filePath, flags)                   |
| Gestione Processi   | Manda un segnale di arresto al processo             | kill()    | killProcessByPID()   | var = kill(pid, signal)                        |
| Gestione Processi   | Sospende un processo                                | sleep()   | Sleep()               | var = sleep(pid)                               |
| Gestione Processi   | Crea un pipe tra due file                           | pipe()    | CreatePipe()          | var = pipe(pid1, pid2)                         |
|                     |                                                     |           |                       |                                                |


&nbsp;
&nbsp;
&nbsp;

Piccole Note riguardo alcune syscalls
===

**Fork()**
> Il processo figlio creato dal processo padre useguendo `fork()` avrà un **PID diverso da quello del padre**. Inoltre i **dati** del processo padre **verranno copiati nel figlio**, ma esso potrebbe prendere un via separata rispetto al padre e i cambiamenti dei valore presente nel figli **NON** verrano copiati nel processo padre.

&nbsp;
&nbsp;
&nbsp;


**Parementri** `O_RDONLY` , `O_WRONLY`, `O_RDWR` e `O_CREAT`

Questi parametri possono essere definiti durante le SysCalls riguardanti la Gestione dei File.


    O_RDONLY

> Il parametro `O_RDONLY` indica che il file sarà **solo letto** (Read-Only)

    O_WRONLY

> Il parametro `O_RWONLY` indica che il file sarà **solo scritto** e non letto (Write-Only)

    O_RDWR

> Il parametro `O_RDRW` indica che il file verrà **sia letto che scritto** (Read-Write)

    O_CREAT

> Il parametro `O_CREAT` indica che se il file non viene trovato sul disco, esso verrà **creato**

&nbsp;
&nbsp;
&nbsp;

# 1.7 Struttura di un sistema operativo

Esistono sei strutture per quanto riguarda i sistemi operativi:
**`Sistemi Monolitici`, `Sistemi a livelli/strati`, `Microkernel`, `Sistemi Client-Server`, `Macchine Virtuali`** e **`Exokernel`**

# 1.7.1 Sistemi Monolitici

>***DEF:*** Un sistema operativo monolitico è un’architettura del sistema operativo in cui l’intero sistema operativo funziona nello spazio del kernel. 

Esistono tre livelli principali nei sistemi operativi monolitici: 
**`Livello applicazione`**, **`Kernel monolitico`** e **`Livello hardware`**.


In questi sistemi operativi, **ogni applicazione ha il proprio spazio di indirizzi**. Pertanto, le applicazioni sono più sicure. **Il kernel gestisce i servizi del sistema operativo**, che includono il `file system`, `lo scheduler della CPU` e il `gestore della memoria`.


Le applicazioni richiedono servizi dal kernel tramite **chiamate di sistema**. 

Quando un’applicazione richiede un servizio, lo spazio degli indirizzi hardware dell’applicazione passa allo spazio degli indirizzi hardware del sistema operativo per eseguirlo. I sistemi operativi monolitici gestiscono un’interfaccia virtuale di alto livello sull’hardware del computer. Inoltre, in questo, è possibile aggiungere driver di dispositivo al kernel come moduli.

Un esempio di sistema che usa il Kernel Monolitoco è **Linux**


&nbsp;
&nbsp;
&nbsp;


# 1.7.2 Sistemi a livelli/strati


Al contrario,

>***DEF:*** Un sistema operativo a più livelli è un’architettura del sistema operativo divisa in un numero di livelli, ciascuno dei quali esegue una funzionalità specifica.

Lo scopo dello sviluppo di sistemi operativi a più livelli è evitare i limiti dei sistemi operativi monolitici.

Nei sistemi operativi a più livelli, tutti i livelli esistono separatamente e la modifica in un livello non influisce sugli altri livelli. 

Pertanto, è anche più facile creare, mantenere e aggiornare i sistemi operativi a più livelli. Inoltre, il livello più basso gestisce le operazioni relative all’hardware mentre il livello più alto gestisce le applicazioni utente.

Ci sono sei livelli principali nei sistemi operativi a più livelli: 


    Hardware

Questo è il livello più basso nell’architettura del sistema operativo. 
Questo livello **gestisce i dispositivi hardware**.


&nbsp;
&nbsp;
&nbsp;



    Livello CPU

 
**Gestisce le attività di pianificazione e pianifica i processi per la CPU**. 

&nbsp;
&nbsp;
&nbsp;


    Gestione della memoria

**Gestisce la memoria**. 

Sposta i processi dal disco alla memoria primaria per l’esecuzione e invia i processi eseguiti al disco.

&nbsp;
&nbsp;
&nbsp;


    Gestione dei processi: 

**Gestisce i processi**. 
Questo livello assegna la CPU per eseguire i processi.

&nbsp;
&nbsp;
&nbsp;


    Buffer IO

Consente agli utenti di **interagire con il sistema e gestisce i buffer per i dispositivi IO**, assicurando che i dispositivi IO funzionino correttamente.

&nbsp;
&nbsp;
&nbsp;


    Programmi utente

Il livello più alto nel sistema operativo a più livelli e **gestisce i programmi utente come elaboratori di testi, browser** ecc.

&nbsp;
&nbsp;
&nbsp;


# 1.7.3 Microkernel


>***DEF:*** Un microkernel in un Sistema Operativo è un software o anche un codice che contiene la quantità quasi minima di funzioni e caratteristiche necessarie per implementare un sistema operativo.

Questa scelta di implementare una quantità quasi minima di funzioni e numero minimo di meccanismi, **nasce dall'idea di differire da un sistema a livelli/strati**.

Tradizionalmente, tutti gli strati andavano nel kernel, ma ciò non è necessario. In realtà, si può fare un grande sforzo per collocare il meno possibile in modalità kernel, dato che errori del kernel possono far cadere immediatamente il sistema.

Quindi l'idea alla base del progetto del microkernel è di **raggiungere un'alta stabilità suddividendo il sistema operativo in `piccoli moduli`** ben defniti, **uno solo** dei quali (microkernel) **verrà eseguito in modalità kernel** e il ***resto come uno dei più normali processi utente*** relativamente deboli. 


**In particolare, facendo girare ogni driver di dispositivo e file system come processo utente separato**

>***E.g***: Un driver audio potrà far sì che il suono sia storpiatoo non si senta bene, ma non bloccherà il computer, mentre in un kernel monolitico può causare un crash/blocco totale.

&nbsp;
&nbsp;
&nbsp;


![alt text](https://i.imgur.com/AzHIM7q.png)

Un esempio di Microkernel sono: QNX, MINIX3 e POSIX.

&nbsp;
&nbsp;
&nbsp;

**Piccole Note**
======

    Strato Server

Lo **`strato Server`** compie la maggior parte del lavoro del sistema operativo. 
Uno o più server **gestiscono il file system, creano il gestore del processo, cancellano e gestiscono processi** e così via. 

Vengono inviati brevi messaggi ai server, **richiedendo le chiamate di sistema POSIX**.

&nbsp;
&nbsp;
&nbsp;


    Reincarnation Server

Questo server ha il compito è di **verificare se gli altri server e driver stanno funzionando correttamente**. 

Nel caso che ne riscontri uno mal funzionante, **viene immediatamente sostituito** senza alcuna interruzione per l’utente. In questo modo il sistema si fa **automanutenzione e può raggiungere una stabilità notevole**.


&nbsp;
&nbsp;
&nbsp;

# 1.7.4 Client-Server

Una leggera variazione rispetto all’idea del microkernel è distinguere due classi di processi, **`i server`**, *ognuno dei quali mette a disposizione alcuni servizi*, e i **`client`**, *che li utilizzano*. 

Questo modello è conosciuto come **modello client-server**. 

Speso il livello più basso è un microkernel, ma non è necessario. L’aspetto principale è la presenza di processi client e processi server.

Per ottenere un servizio, **un processo client costruisce un messaggio indicando la propria richiesta e lo invia al servizio appropriato (server)**. **Il servizio quindi esegue il lavoro e rimanda indietro la risposta**. Se client e server sono eseguiti sulla stessa macchina sono possibili alcune ottimizzazioni, ma concettualmente si tratta del passaggio di un messaggio.

**Questo modello client-server offre un’astrazione utilizzabile per una macchina singola o per una rete di macchine**, dato che al client non fa differenza se il server è locale o in rete.

![alt text](https://i.imgur.com/Hg8OYvm.png)

&nbsp;
&nbsp;
&nbsp;

# 1.7.5 Macchine Virtuali

Nel passato molte aziende hanno tradizionalmente tenuto i loro server di posta, server web, server FTP e altri server su computer separati, talvolta con sistemi operativi diversi. 

Una soluzione a quessto è la **`Virtualizzazione`**.

Tramite la virtualizzazione **si possono tenere tutti questi servizi/S.O sulla stessa macchina**, *senza che il blocco di un server/S.O determini il crollo degli altri*.

> ***E.g***: Nell'hosting Web al giorno d'oggi vengono **usate piccole Macchine Virtuali installate su Server potenti**, **così da avere tantissimi utenti (clienti) connessi ad una macchina**, portando **meno consumi, spazio fisico usato e scalabità**. I clienti vedranno solo la macchina virtuale, che opera come un computer normale.

&nbsp;
&nbsp;
&nbsp;

Questo tipo di esempio è rappresentato nella figura (a)

![alt text](https://i.imgur.com/V3qapOM.png)

&nbsp;
&nbsp;
&nbsp;

**Un grande problema della Virtualizzazione era lo sviluppo del software**.

Per permettere che **il software della macchina virtuale fosse eseguito su un computer**, **la sua CPU doveva essere virtualizzabile.**

Quando un **sistema operativo in esecuzione su una macchina virtuale** (in modalità utente) **esegue un’istruzione privilegiata, come fare un I/O**, è essenziale che **l’hardware esegua un trap verso l'hypervisor**, in modo che l’istruzione possa essere emulata via software.

*Su alcune CPU, tipo i Pentium, i tentativi di eseguire istruzioni privilegiate in modalità utente sono ignorati. Cioè rende impossibile disporre di macchine virtuali su questo hardware, tranne alcuni tentativi riusciti che avevano terribili problemi di performance.*

Ai giorni nostri, oltre a **`VMware e a Xen`**, fra gli hypervisor più conosciuti troviamo oggi **`KVM` (Linux), `VirtualBox` (Oracle) e `Hyper-V` (Microsoft)**. 

***Alcuni progetti di ricerca inizialmente migliorarono le prestazioni rispetto agli interpreti come Bochs traducendo blocchi di codice al volo, memorizzandoli in una cache interna e quindi riutilizzandoli se dovevano essere nuovamente eseguiti. Le prestazioni venivano in questo modo sensibilmente migliorate,***

Ma nonostante questi miglioramenti, queste soluzioni non erano ancora abbastanza veloci da poter essere utilizzati in ambiente commerciale, dove invece le prestazioni sono fondamentali.

**Il passaggio successivo per migliorare le prestazioni fu l’aggiunta di un modulo kernel che eseguisse il grosso del lavoro.**

In pratica, oggi tutti gli hypervisor in commercio, come VMware Workstation, utilizzano una strategia ibrida. Sono chiamati **`hypervisor di tipo 2`**.

&nbsp;
&nbsp;
&nbsp;

**La macchina virtuale Java**
========


Un’altra area in cui le macchine virtuali sono utilizzate, ma in modo un po’ diverso, è l’esecuzione di programmi `Java`. 

Quando Sun Microsystem inventò il linguaggio di programmazione Java, c**reò anche una macchina virtuale** (cioè un’architettura di computer) chiamata **`JVM`** (***J**ava **V**irtual **M**achine*).

**Il `compilatore Java` produce codice per la `JVM`, poi generalmente eseguito da un interprete software `JVM`**. Il vantaggio di tale metodo è che il **`codice JVM` (bytecode) può essere spedito tramite Internet su qualunque computer che disponga di un interprete JVM ed eseguito lì**. 

![alt text](https://i.imgur.com/Wdcrt7s.png)

# 1.7.6 Exokernel

Piuttosto che clonare la macchina reale, come avviene con le macchine virtuali, un’altra strategia è partizionarla, assegnando quindi a ogni utente un sottoinsieme delle risorse. 

>***E.g:*** Una macchina virtuale potrebbe così prendersi i blocchi del disco da `0` a `1023`, la successiva i blocchi da `1024` a `2047` e così via.

Il livello alla base, che gira in modalità kernel, è un programma chiamato **exokernel**.

**Il suo compito è quello di allocare risorse alle macchine virtuali e poi controllare i tentativi di impiego per essere certo che nessuna macchina provi a usare risorse di un’altra**. **Ogni macchina virtuale** a livello utente e**segue il suo sistema operativo personale**, a parte il fatto che **ognuna è limitata a usare le sole risorse che ha richiesto e che le sono state allocate**.

**Il vantaggio dello schema dell’exokernel è che risparmia uno strato di corrispondenza**. *Negli altri progetti ogni macchina virtuale pensa di avere il proprio disco, con blocchi che vanno da `0` a un `massimo`, così il monitor della macchina virtuale deve mantenere delle tabelle per rimappare gli indirizzi del disco (e di tutte le altre risorse)*. 

Con l’exokernel, questo rimappaggio non è più necessario. **L’exokernel necessita solo di tenere traccia di quale sia la macchina virtuale a cui è stata assegnata una certa risorsa**. Questo metodo **ha ancora il vantaggio di tenere separata la multiprogrammazione dal codice del sistema operativo utente** (nello spazio utente), **ma a un costo inferiore**, dato che il **compito dell’exokernel è far sì che tutte le macchine virtuali siano tra loro indipendenti**.

