# 4.1 I File

**I file sono un'astrazione** che il S.O mette a disposizione dei processi **per poter utilizzare in modo semplice, efficiente e sicuro i dispositivi di memoria secondaria.**

**I processi** effetuano l'astrazione dei file per operare **usando `nomi simbolici`** che il `File System` **fa corrispondere ad un insieme di zone (blocchi) sui dispositivi utilizzati.**

**Ogni file possiede un insieme di attributi (chiamata anche `Metadata`).**
>**E.g:** `Nome, Tipo (Binario o ASCII), Dimensione, Protezione/Permessi, Ora, data, ID Utente`, etc..

**Essi vengono registrati in una struttura di directory mantenuta anch'essa in memoria secondaria.**

Quindi possiamo dire che:

>**DEF:** Un file è **un tipo di dato astratto** sul quale **sono possbili operazioni tipo: Creazione, Lettura, Scrittura, Cancellazione, Rinomiazione** e ect...

&nbsp;
&nbsp;
&nbsp;

# I nomi dei File

I S.O moderni consentono l'utilizzo di stringhe di lunghezza variabile da un minimo di 8 caratteri fino a un massimo di 255 caratteri.

Alcuni S.O comprendono anche l'uso di numeri e caratteri speciali.

Alcuni S.O inoltre sono Case Sensitive (Sensibili tra le minuscole e maiuscole)

Una comparazione è che MS-DOS non è Case Sensitive, mentre UNIX e le sue derivate lo sono.

>**E.g:** Su MS-DOS un file è chiamato `PROTOGEN` ma quando l'utente va a chiamare un editor di testo, può anche scrivere `protogen`, e quel S.O prenderà lo stesso file, dato che MS-DOS non è case sensitive.

&nbsp;
&nbsp;
&nbsp;

*Estensioni dei File*
======


I nomi di file sono spesso divisi in due parti separate da un punto, la seconda parte è detta estensione (`extension`).

In alcuni sistemi l'estensione non ha significati particolari per il S.O (serve solo all'utente), mentre in altri può essere usata per associare attributi al file.

>**E.g:** Un file con `.doc` verrà aperto da un `Word Editor` compatibile con quell'estensione e tipo di file.

&nbsp;
&nbsp;
&nbsp;

### **`Elenco di estensioni tipiche`**

| **Estensione** 	| **Significato**                                          	|
|----------------	|----------------------------------------------------------	|
| file.bak       	| File di Backup                                           	|
| file.c         	| File con codice sorgente in linguaggio C                 	|
| file.gif       	| Immagine Animata                                         	|
| file.hlp       	| File di aiuto                                            	|
| file.html      	| Documento HTML                                           	|
| file.jpg       	| Immagine con codifica JPEG                               	|
| file.mp3       	| Musica con codifica MPEG Layer 3                         	|
| file.mpg       	| Filmato con codifica MPEG                                	|
| file.o         	| File oggetto (output da compilatore, non ancora linkato) 	|
| file.pdf       	| Documento PDF                                            	|
| file.ps        	| File PostScript                                          	|
| file.tex       	| File input con formattazione TEX                         	|
| file.txt       	| File di testo generico                                   	|
| file.zip       	| Archivio compresso                                       	|
| file.sh        	| File con codice Shell                                    	|
| file.exe       	| Eseguibile Windows                                       	|

&nbsp;
&nbsp;
&nbsp;


*Filesystem*
===

In ambito dei **S.O** di Microsoft, i sistemi **che erano basati su Kernel MS-DOS (MS-DOS - Win98SE) hanno usato due file system: FAT16 e FAT32**

Mentre sui S.O che montavano il **Kernel NT (Win2000 - Win10), è stato proposto l'uso di un nuovo file system: NTFS.**


Per quanto riguarda UNIX e le sue derivate, soprattutto **Linux, i file system più usati sono: UFS, EXT2, EXT3, EXT4, BRTFS e ZFS.**

&nbsp;
&nbsp;
&nbsp;


*Struttura di un File*
===

I file possono essere strutturati in tanti modi diversi. Tre delle possibilità più comuni sono descritte nella figura sottostante.

### **`3 tipi di strutture di File`**

![alt text](https://mediaserver.pearsonitalia.it/mediaserver_uni/books/prod/2017/9788891901026_TANENBAUM/EPUB/ASSETS/images/04-02.png)

Il file nella (a) è una sequenza non strutturata di byte. 

In effetti il sistema operativo non conosce e non è interessato a che cosa stia nel file. Tutto ciò che vede sono byte. Qualsiasi significato deve essere imposto dai programmi a livello utente. 

Sia UNIX sia Windows adottano questo approccio.

&nbsp;
&nbsp;
&nbsp;


*Tipi di File*
===

Il S.O può definire ed usare per diversi scopi diversi tipi di file, ad esempio UNIX distingue tra:

-   **File Regolari/Normali**
-   **Directory**
-   **File Speciali a blocchi**
-   **File Speciali a caratteri**
-   **I file regolari sono generalmente file ASCII o Binari.**

>**Nota:** I file normali sono quelli contenenti informazioni utente. 

>**Nota:** Le directory sono file di sistema usati per mantenere la struttura del file system.

>**Nota:** I file speciali a blocchi sono usati per modellare i dischi. In questo capitolo ci interesseremo principalmente dei file normali.

>**Nota:** I file speciali a caratteri sono relativi all’input/output e usati per modellare i dispositivi seriali di I/O, come terminali, stampanti e network.

**I file ASCII possono sempre essere stampati e visualizzati**, mentre i **file binari hanno invece una struttura riconoscibile solo dal programma che li utilizza.**

&nbsp;
&nbsp;
&nbsp;


*Tipi di accesso ai File*
===

**I primi S.O consentivano solo l'accesso sequenziale ai file**, cioè i bytes **potevano essere letti solo in ordine (a partire dal primo settore del supporto fisico)** e la tipica **operazione di aggiornamento era l'`append` di nuova informazione** alla fine dei file.

**Di conseguenza per rileggere una posizione precendente bisognava "riavvolgere" più volte il file.**

Il supporto tipico su cui veniva usato questo tipo di accesso erano i **nastri magnetici.**

Mentre, **sui S.O moderni è presente e concesso anche l'accesso casuale (random)** a **determinate posizioni del supporto dove risiedeva il file tramite operazione di `seek`.**

In passato si poteva scegliere che tipo di accesso usare, mentre **ora i S.O creano file usando sempre l'accesso casuale.**

&nbsp;
&nbsp;
&nbsp;


*Attributi dei File*
===

*Tutti i sistemi operativi associano informazioni a ciascun file.*

Queste voci extra del file sono chiamate **attributi (`metadati`)**. 

>**E.g:** La data e l’ora in cui il file è stato modificato l’ultima volta e la dimensione del file. 

**L’elenco degli attributi cambia in modo considerevole da sistema a sistema.**

&nbsp;
&nbsp;
&nbsp;

### **`Tabella dei più comuni attributi dei file`**


| **Attributo**                      	| **Significato**                                                 	|
|------------------------------------	|-----------------------------------------------------------------	|
| Protezione                         	| Chi può accedere al file e in che modalità                      	|
| Password                           	| Password necessaria per accedere al file                        	|
| Creatore                           	| ID della persona che ha creato il file                          	|
| Proprietario                       	| Proprietario attuale                                            	|
| Flag di sola lettura               	| 0 per lettura/scrittura; 1 per sola lettura                     	|
| Flag di file nascosto              	| 0 per normale; 1 per non visualizzare negli elenchi             	|
| Flag di file di sistema            	| 0 per file normali; 1 per file di sistema                       	|
| Flag per file di archivio          	| 0 per già sottoposto a backup; 1 per file di cui fare il backup 	|
| Flag ASCII/Binario                 	| 0 per file ASCII; 1 per file Binario                            	|
| Flag di accesso casuale            	| 0 per accesso sequenziale; 1 per accesso casuale                	|
| Flag temporaneo                    	| 0 per normale; 1 per cancellare il file alla fine del processo  	|
| Flag di file bloccato              	| 0 per non bloccato; non zero per bloccato                       	|
| Lunghezza del record               	| Numero di byte nel record                                       	|
| Posizione della chiave             	| Offset della chiave in ciascun record                           	|
| Lunghezza della chiave             	| Numero di byte del campo chiave                                 	|
| Data e ora creazione file          	| Data e ora di quando è stato creato il file                     	|
| Data e ora ultimo accesso al file  	| Data e ora di quando è avvenuto l'ultimo accesso al file        	|
| Data e ora ultima modifica al file 	| Data e ora di quando è avvenuta l'ultima modifica al file       	|
| Dimensione attuale                 	| Numero di byte nel file                                         	|
| Dimensione massima                 	| Numero di byte di cui può aumentare il file                     	|

&nbsp;
&nbsp;
&nbsp;


*Operazioni sui File*
===

**Sistemi operativi differenti forniscono operazioni differenti per permetterne la memorizzazione e il recupero.**

Di seguito sono descritte le chiamate di sistema più comuni relative ai file.

| **Nome**       	| **Descrizione**                                                                                                                                                            	|
|----------------	|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------	|
| Create         	| Creazione di un file senza dati all'interno con impostazione di alcuni attributi base.                                                                                     	|
| Delete         	| Cancellazione di un file.                                                                                                                                                  	|
| Open           	| Apertura del file da parte di un processo per permettere la lettura. Vengono letti anche gli attributi di quel file dal sistema.                                           	|
| Close          	| Il file viene chiuso, dopo aver liberato le tabelle interne dagli attributi.                                                                                               	|
| Read           	| Vengono letti i dati del file. Deve essere fornito un buffer dove mantenere i file letti.                                                                                  	|
| Write          	| Vengono scritti dati sul file. Se la posizione è alla fine del file, la sua dimensione aumenta. Se la posizione è in mezzo al file, i dati esistenti vengono sovrascritti. 	|
| Append         	| È una forma limitata di write. Può solo aggiungere file alla fine del file.                                                                                                	|
| Seek           	| Serve per un file ad accesso casuale per specificare da dove leggere i dati.                                                                                               	|
| Get Attributes 	| Legge gli attributi dei file.                                                                                                                                              	|
| Set Attributes 	| Imposta alcuni attributi dati in input dall'utente.                                                                                                                        	|
| Rename         	| Viene cambiato il nome al file in questione.                                                                                                                               	|
|                	|                                                                                                                                                                            	|

&nbsp;
&nbsp;
&nbsp;

# 4.2 Le Directory

>**DEF:** *Una Directory è un elenco di nomi di file (e/o di altre directory) ai quali viene associato un nome.*

**Il concetto di Directory è fondamentale per dare una struttura (tipicamente gerarchica) al sistema dei File che rispecchi i criteri logici dell'amministratore di Sistema e degli utenti.**

In una **strutturazione ad albero** del sistema di archiviazione la radice è detta **radice del file system (`root directory`)**.

Ogni processo ha un attributo che indica la **directory nella quale sta operando** detta **directory di lavoro (`working directory`)**.

I processi possono modificare tale attributo usando un comando (Esempio: **`cd`**).

Sull'oggetto directory sono definite operazioni specifiche: `create, delete, opendir, closedir, readdir, rename, link, unlink.` 

&nbsp;
&nbsp;
&nbsp;

*Strutturazione gerarchica delle directory*
======

La strutturazione più semplice è quella ad **un solo livello** dove tutti i file sono associati ad un campo che può contenere:

-   Il valore degli attributi del file
-   Un puntatore ad una struttura che contiene gli attributi del file.


Il sistema della directory può invece essere organizzato in modo gerarchico.

>**E.g:** Associando ad ogni utente una propria directory.

Questa strutturazione è detta **a due livelli**.

Per fornire agli utenti la possibilità di strutturare le loro directory in modo arbitrario e raggrupare in modo logico i propri file si introduce la **strutturazione ad albero.**


![alt text](https://i.imgur.com/x1mQpmh.png)

&nbsp;
&nbsp;
&nbsp;

*Nome di percorso*
===

L'organizzazione gerarchica dei file introduce la necessità di un sistema per esprimere in modo non ambiguo il nome dei file.

In genere si utilizzando due modalità per specificare il nome di un file:

-   Path Name Assoluto:
    -   **Ovvero il cammino da compiere per arrivare al file partendo dalla root del Sistema.**

    -   **Ogni file ha un solo path name assoluto**
    -   >**E.g:** `Windows`: C:\Users\ast\mailbox
    -   >**E.g:** `UNIX`: /home/ast/mailbox

-   Path Name Relativo:
    -   **Esprime il percorso da compiere per arrivare al file partendo dalla directory corrente.**
    -   La Directory corrente **può essere cambiata con un system call `change directory` (`cd`)**
    -   >**E.g:** `UNIX`: [~/Appunti-SO/4.2 $] **cd** /home/lyz/

-   Diversi Path Name Relativi:
    -   Posso corrispondere allo stesso file.

**In base a che tipo di S.O, ognuno ha un suo Albero di Directory.**

### **`Esempio di Albero di Directory in Linux`**

![alt text](https://vari-linux.readthedocs.io/en/latest/_images/filetree.jpg)

&nbsp;
&nbsp;
&nbsp;

*Operazioni sulle Directory*
======

Le chiamate di sistema consentite per la gestione delle directory differiscono maggiormente da sistema a sistema rispetto alle chiamate di sistema per i file.

Per dare un’idea di quali siano e di come lavorino, ne riportiamo un esempio (preso da UNIX).

| **Nome** 	| **Descrizione**                                                                                   	|
|----------	|---------------------------------------------------------------------------------------------------	|
| Create   	| Viene creata una directory vuota, fatta eccezione per . e .. che sono messi in automatico dal S.O 	|
| Delete   	| Viene cancellata una directory solo se è vuota.                                                   	|
| Opendir  	| Viene aperta una directory. Es: elencare  tutti i file in essa.                                   	|
| Closedir 	| Viene chiusa una directory dopo che è  stata aperta.                                              	|
| Readdir  	| Viene letta una directory.                                                                        	|
| Rename   	| Viene rinominata una directory.                                                                   	|
| Link     	| Viene fatto il collegamento (`linking`) tra  un file e una o più directory.                         	|
| Unlink   	| Elimina, se esistono, i linking ad un  specifico file.                                            	|

&nbsp;
&nbsp;
&nbsp;

# 4.3 Implementazione del File System

*Layout del File System*
======

**I file system sono memorizzati sul disco.**

**Ogni disco è suddivisibile in una o più partizioni, con file system indipendenti su ciascuna.** 

**Il settore 0 del disco** è chiamato **`MBR` (Master Boot Record)** ed è **usato per avviare il S.O computer**. La fine dell’MBR contiene la tabella delle partizioni.

**Questa tabella contiene gli indirizzi di inizio e di fine di ciascuna partizione. Una della partizioni della tabella è indicata come attiva.** 

**Quando il computer è avviato, il BIOS legge ed esegue l’MBR**. La prima cosa che **l’MBR** fa è **localizzare la partizione attiva**, leggerne il primo blocco, **chiamato blocco di boot (`boot block`) ed eseguirlo.**

**Il programma nel blocco di boot carica il sistema operativo contenuto in quella partizione.**

Inoltre sui computer recenti, è presente una partizione EFI (Extensible Firmware Interface).

Questa partizione può essere definita come un evoluzione dell'MBR, ovverò che è sempre responsabile dell'avvio del Bootloader di un S.O, ma permette di avere più Bootloaders, comodo se si fa dual-booting tra, ad esempio, Linux e Windows.


### **`Possibile layout di un file system`**

![alt text](https://mediaserver.pearsonitalia.it/mediaserver_uni/books/prod/2017/9788891901026_TANENBAUM/EPUB/ASSETS/images/04-09.png)

&nbsp;
&nbsp;
&nbsp;

*File System in UNIX*
======

-   `Boot Block`
    -   Contiene le informazioni per il boot del sistema

-   `Super Block`
    -   Informazioni sul layout del FS: tipo di FS, numero di `i-nodes`, numero di blocchi nel disco, inizio dei blocchi liberi, ect...

-   `I-nodes`
    -   Record di 64 byte associati a ciascun file o directory.
    -   Ogni i-node contiene attributi e l'informazione necessaria a risalire alla posizione sul disco di tutti i blocchi di un certo file.

-   `Free`
    -   Struttura per gestire i blocchi liberi (può essere una bitmap o una lista di puntatori)

-   `Data Blocks`
    -   Blocchi di dati relativi al file e directory (non necessariamente contigui)

&nbsp;
&nbsp;
&nbsp;

*Allocazione contigua*
======

-   Consiste nelI'allocazione dei blocchi dei file in posizione contigue del disco.

-   L'allocazione contigua è semplice da implementare (basta memorizzare il primo indirizzo per ogni file);

-   L'allocazione contigua è efficiente in lettura anche nel caso di accesso diretto (basta sommare N all'indirizzo del blocco iniziale);

-   L'allocazione contigua introduce un problema di frammentazione allorchè si iniziano a inserire e rimuovere file.

-   Occorre gestire lo spazio libero (i “buchi”) oppure effettuare frequenti e costose  ricompattazioni del disco;

-   L'allocazione contigua è comunque utile per la gestione di dispositivi a sola lettura come `DVD` e `CD-ROM`;

&nbsp;
&nbsp;
&nbsp;

*Allocazione a lista concatenata*
======

L’ Assegnazione Contigua è uno dei **metodi principali per l’assegnazione dello spazio di un disco.**

**I file sono realizzati come liste concatenate di blocchi che possono essere sparsi ovunque nel disco.**

**In questo modo non si ha spreco di spazio su disco come nel caso dell’ assegnazione contigua.**

![alt text](https://i.imgur.com/iWX3cru.jpeg)

**I vantaggi sono la riduzione del problema della frammentazione esterna del disco** e la **semplificazione della struttura delle directory**.

Lo svantaggio principale è che l’accesso diretto ad un blocco è lento, occorre passare per n blocchi per leggere il blocco n-esimo. 

Per risolvere questo problema l’implementazione della lista concatenata può utilizzare **una tabella di allocazione dei file (`FAT`)** così definita: **in ogni cella della FAT troviamo il puntatore al blocco successivo del file**.

&nbsp;
&nbsp;
&nbsp;

*Allocazione con FAT*
======

-   La `FAT` in sé **mantiene la traccia delle aree del disco disponibili e di quelle già usate dai file  e dalle directory**: la differenza fra `FAT12, FAT16` e `FAT32` consiste appunto in quanti bit sono allocati per numerare i cluster del disco. Con **`12 bit`**, il file system può indirizzare al massimo *`212 = 4096 cluster`*, mentre con **`32 bit`** si possono gestire *`232 = 4.294.967.296 cluster`*;
    -   l'aumento del numero di bit di indirizzo dei cluster si è reso necessario per gestire unità a disco sempre più grandi e capienti.


-   `FAT16`: I cluster potevano **avere fino a `64 settori`**, dando cluster di `32 KB` con la solita dimensione di **`512 byte per settore`**, fissavano **il limite per una partizione FAT16 a `2 GB`.**

-   `FAT32`: numeri per i cluster da `32 bit`, in realtà **ne vengono utilizzati solo 28**;

    -   Con il FAT32 **la dimensione del singolo file non può essere superiore ai `4 GB`**. **La dimensione della partizione può arrivare sino a `1 TB`**

&nbsp;
&nbsp;
&nbsp;

*I-nodes*
======

-   Sono record associati ad ogni file che contengono: gli attributi del file e le informazioni necessarie a reperire i blocchi del file.


-   L’ i-node deve essere caricato in memoria solo quando il file deve essere usato e non deve risiedervi permanentemente!


-   La tabella degli i-nodes dei file in uso è una struttura generalmente molto più piccola della FAT descritta in precedenza.


-   La tecnica degli i-node permette di dimensionare Io spazio della tabella in funzione del massimo numero di file aperti e non della dimensione del disco.


-   Per trattare “file grandi” gli i-nodes supportano quattro tecniche di indicizzazione: **diretta, indiretta, indiretta doppia e indiretta tripla**.

-   L'efficienza diminuisce nel caso di file di grandi dimensioni!!


&nbsp;
&nbsp;
&nbsp;

*Implementazione delle directory*
======

Nel caso un utente specifichi il nome di un file (usando un path name) il FS arriva all'elemento di 
directory relativo che può contenere:

-   **l'indirizzo sul disco del file** (`allocazione contigua`);

-   **l'indirizzo del primo blocco** (`allocazione concatenata e FAT`)

-   **l'indirizzo dell'i-node** (`allocazione indicizzata`)

Un altro problema nell'implementazione di directory è decidere se **inserire le informazioni relative ai file (attributi) nella directory o nella struttura dati (es. i-node)**.

**DOS/Windows fanno la prima scelta, UNIX la seconda.**

