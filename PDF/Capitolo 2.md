# 2.0 Processi e Thread

# 2.1 Processi

Tutti i computer moderni svolgono spesso molti compiti contemporaneamente. 

>***E.g:*** *Quando un utente accende il suo computer, il Sistema Operativo sotto la scocca **fa avviare molti processi che non sono visibile all'utente**, come il server grafico, programma antivirus e così via. Oltre a questi **ci sono da considarare i processi avviati esclusivamente dall'utente**, come un broswer o servizio di stampa. Tutte queste attività **devono essere gestite da un sistema multiprogrammato**.*

In ogni sistema multiprogrammato la **CPU passa da processo a processo rapidamente, eseguendone ognuno per decine o centinaia di millisecondi.** 

***Mentre la CPU esegue un solo processo alla volta, nel corso di 1 secondo può lavorare su parecchi di loro, dando l’illusione di parallelismo.***

**Tener traccia di molteplici attività parallele è un compito difficile.** 

Perciò i progettisti di sistemi operativi nel corso degli anni hanno sviluppato **un modello concettuale** (i processi sequenziali) che **semplifica la gestione del parallelismo**.

&nbsp;
&nbsp;
&nbsp;

# 2.1.1 Modello di Processo

In questo modello **tutto il software eseguibile** sul computer, incluso talvolta il sistema ope­rativo, **è organizzato in un certo numero di processi sequenziali**, **detti processi.** 

>***DEF:*** *Un processo è quindi **semplicemente un’istanza di un programma in esecuzione**, compresi i valori attuali del contatore di programma, dei registri e delle variabili.*

**Concettualmente ogni processo ha la sua CPU virtuale.**

*In realtà la CPU passa avanti e indietro da processo a processo.*
Ciò è chiamato **multiprogrammazione**.

&nbsp;
&nbsp;
&nbsp;

![alt text](https://i.imgur.com/fsk5MvW.png)

Nella Figura ***`a`*** è mostrato un computer che fa multiprogrammazione di quattro programmi in memoria. 

Nella Figura ***`b`*** sono mostrati quattro processi, ognuno con il suo flusso di controllo (cioè il suo contatore di programma personale) e ognuno che gira indipendentemente dagli altri. Naturalmente c’è un solo contatore di programma fisico, così quando ogni processo è in esecuzione, il suo contatore di programma logico è caricato nel contatore di programma vero. Quando ha (momentaneamente) terminato, il contatore di programma fisico viene salvato in memoria nel contatore di programma logico archiviato del processo. 

Nella Figura ***`c`*** vediamo che osservati su un intervallo abbastanza lungo, tutti i processi hanno avuto un avanzamento, ma in un preciso particolare istante solo un processo è realmente in esecuzione.

&nbsp;
&nbsp;
&nbsp;

Si dia per assodato che vi sia una sola CPU *(nonostante al giorno d'oggi ci siano chip con 2,4 o più core)*.

Dato che la CPU passa rapidamente da un processo all’altro, la velocità a cui un processo realizza la sua elaborazione non sarà uniforme e probabilmente nemmeno riproducibile se gli stessi processi fossero eseguiti di nuovo. 
Di conseguenza, i processi non devono essere programmati basandosi su assunti precostituiti riguardo alle loro tempistiche.

>***E.g:*** *Un processo audio che esegue musica per accompagnare un video in alta risoluzione eseguito da un altro apparecchio. Poiché l’audio dovrebbe partire leggermente dopo il video, segnala al server video di avviare la riproduzione, quindi attende per `10.000 cicli` prima di eseguire l’audio. Tutto funziona alla perfezione a condizione che il ciclo sia un timer affidabile, ma se la CPU decide di passare a un altro processo durante il tempo di attesa, il processo audio potrebbe non venire nuovamente eseguito se non dopo che i frame video corrispondenti sono già stati riprodotti.*

Quando un processo ha dei requisiti critici real-time come nell'esempio precedente, vale a dire che eventi particolari devono avvenire all’interno di un determinato numero di millisecondi, devono essere prese delle misure speciali per assicurare che ciò succeda davvero. Tuttavia, la maggior parte dei processi non è in genere influenzata dalla multiprogrammazione della CPU sottostante o dalle velocità relative dei diversi processi.

&nbsp;
&nbsp;
&nbsp;

# 2.2 Thread

**La comunicazione tra processi può richiedere un alto tempo di esecuzione (overhead)** dato che è necessario ricorrere a chiamate di sistema del kernel, come descritto.

**Anche la creazione e la terminazione di un processo risultano costose in termini di overhead.**

La separazione degli spazi di indirizzamento dei processi, consente da una parte la protezione dei dati dei singoli processi, ma dall'altra rende complesso l'accesso a strutture dati comuni, che si realizza attraverso memoria comune o mediante scambio di messaggi.

Molte componenti di un sistema operativo e molte applicazioni possono essere sviluppate in 
moduli che girano in parallelo. Generalmente, ciascun modulo condivide dati e risorse comuni. 

**Per ottenere una soluzione efficiente** per le applicazioni che presentano un tipico grado di 
parallelismo, **sono stati introdotti**, nei sistemi operativi, **i `thread`**.

>**DEF:** Un `thread` è **un flusso di esecuzione all'interno di uno stesso processo**.

**All'interno di un processo è possibile definire più thread, ognuno dei quali condivide le  risorse del processo, appartiene allo stesso spazio di indirizzamento e accede agli stessi dati, definiti con visibilità globale.**

I thread possono essere creati e distrutti più facilmente rispetto ai processi; il cambio di contesto è più efficiente.

>**DEF:** Il termine `multithreading` significa che **un processo possiede più thread**.

Ad ogni thread sono associati:

-   **Un descrittore.**

-   **Uno stato**: 
    -   `esecuzione`, `pronto` e `bloccato`.
    
-   **Uno spazio di memoria per le variabili locali.**

-   **Uno stack**.

-   **Un contesto rappresentato dai valori di registri del processore utilizzati dal thread**.

**Ad un thread non appartengono le risorse, che invece appartengono al processo che lo contiene.**

**Le informazioni relative ai thread sono ridotte rispetto a quelle dei processi**, quindi **le operazioni** di cambio di contesto, di creazione e terminazione **sono molto semplificate rispetto a quelle dei processi.**

&nbsp;
&nbsp;
&nbsp;

### **`Elementi condivisi da tutti i thread di un processo e elementi privati di ciascun thread.`**


| **Elementi per processo**          	| **Elementi per thread** 	|
|------------------------------------	|-------------------------	|
| Spazio degli indirizzi             	| Contatore di programma  	|
| Variabili Globali                  	| Registri                	|
| File aperti                        	| Stack                   	|
| Processi figli                     	| Stato                   	|
| Allarmi in sospeso                 	|                         	|
| Segnali e gestori dei segnali      	|                         	|
| Informazioni relative agli account 	|                         	|

**La gestione dei thread può avvenire sia a `livello utente` che a `livello kernel`.**

&nbsp;
&nbsp;
&nbsp;

*Thread a livello utente*
====

**Modello da molti a uno:**

-   Nel caso di thread a livello utente si **utilizzano librerie realizzate a livello utente** che forniscono tutto il **supporto per la gestione dei thread**: **`creazione`, `terminazione`, `sincronizzazione`** nell'accesso di variabili globali del processo, per lo **`scheduling`,** etc...

-   **Tutte queste funzioni sono realizzate nello spazio utente**, il SO non vede l'esistenza dei thread e considera solo il processo che li contiene.

-   I thread possono chiamare le `system call`, ad esempio per operazione di I/O; in questo caso interviene **il SO che blocca il processo e di conseguenza tutti i thread in esso contenuti.**

-   I thread a livello utente **hanno il vantaggio che possono essere utilizzati anche in SO che non supportano direttamente i thread**; è possibile **usare politiche di scheduling indipendenti dal kernel del SO.**

-   Tuttavia, **non è possibile**, con i thread a livello utente, **sfruttare il parallelismo delle architetture multiprocessore**, dato che quando un processo è assegnato ad uno dei processori, tutti i suoi thread sono eseguiti, uno alla volta, su quel solo processore.

&nbsp;
&nbsp;
&nbsp;

![alt text](https://i.imgur.com/JORxpzr.png)

&nbsp;
&nbsp;
&nbsp;

*Thread a livello kernel*
====

**Modello da uno a uno:**

-   La gestione dei `thread` avviene a `livello kernel`. 

-   **In questo caso a ciascuna funzione di gestione dei thread**, comprese la creazione, la terminazione, la sincronizzazione e Io scheduling **corrisponde a una chiamata di sistema** e **il kernel deve mantenere sia i descrittori dei processi sia i descrittori di tutti i thread**.

-   A differenza del modello precedente, ora **se un thread esegue una chiamata di sistema bloccante**, **il thread si blocca ma gli altri thread possono continuare la Ioro esecuzione**.

-   Tuttavia, il carico dovuto alla creazione di un thread è più onerosa. 

-   Pertanto, **per non degradare eccessivamente le prestazioni** di un'applicazione multithread, generalmente **si limita il numero di thread gestibili dal kernel.**

-   Praticamente **tutti i sistemi operativi moderni**, compresi `Windows`, `Linux`, `MacOS`, `UNIX`, **supportano questo tipo di modello**. 

-   Inoltre, il modello uno a uno **consente la reale esecuzione di thread in parallelo nei sistemi basati su architetture multiprocessore.**

&nbsp;
&nbsp;
&nbsp;

![alt text](https://i.imgur.com/gGINxle.png)

&nbsp;
&nbsp;
&nbsp;

**Modello molti a molti:**

-   **Il modello da molti a molti mette in corrispondenza i thread a livello utente con un numero minore o uguale di thread a livello kernel.** 

-   Questo modello **presenta le caratteristiche vantaggiose dei precedenti modelli.** 

-   **È possibile creare tutti thread che sono necessari all'interno di un'applicazione e i corrispondenti thread a livello kernel possono essere eseguiti in parallelo nelle architetture multiprocessore.** 

-   Inoltre, **se un thread esegue una chiamata di sistema bloccante, il kernel manda in esecuzione un altro thread.**

&nbsp;
&nbsp;
&nbsp;

![alt text](https://i.imgur.com/dOOQMGT.png)

&nbsp;
&nbsp;
&nbsp;

# 2.3 Comunicazione fra processi

**I processi necessitano frequentemente di comunicare con altri processi.** 

**C’è quindi bisogno che i processi comunichino**, preferibilmente in un modo ben strutturato, non usando gli interrupt. 

**Ma esistono dei problemi relativi a questa comunicazione fra processi o `IPC`** (InterProcess Communication).

Molto brevemente, **i problemi da affrontare sono tre**:

-   Come un processo possa **passare informazioni a un altro**. 

-   L’essere sicuri che **due o più processi non vadano ad accavallarsi**, cioè che entrambi i processi lottano per leggere/modificare un specifico valore in memoria. 

-   Il terzo riguarda la **corretta sequenzialità** quando vi sono delle dipendenze.

    -   >**E.g:** Se il processo A produce dei dati e il processo B li stampa, B deve attendere che A abbia prodotto qualche dato prima di iniziare a stampare.

&nbsp;
&nbsp;
&nbsp;


>**NOTA:** È anche importante menzionare che due di questi problemi si possono applicare ugualmente bene anche ai thread. 

Vediamo un po':

-   **Il primo** (passare le informazioni):

    -   **Non è un problema per i thread**, dato che condividono uno spazio degli indirizzi comune.
     
-   **Il secondo** (non intromettersi l’uno nell’altro) e **il Terzo** (la corretta sequenza)

    -   **Questi problemi valgono anche per i thread.**


&nbsp;
&nbsp;
&nbsp;

*Race Condition*
====

**La situazione di corsa (`race condition`), è un fenomeno che si presenta nei sistemi concorrenti quando**, in un sistema basato su processi multipli, **il risultato finale dell'esecuzione di una serie di processi dipende dalla temporizzazione o dalla sequenza con cui vengono eseguiti.**

Spesso, una situazione di corsa **è un effetto indesiderato e genera malfunzionamenti.**

 In questo caso, essa è denominata corsa critica per il sistema.

**Per evitare il verificarsi di situazioni di corsa** quando si impiegano memorie, file o risorse in condivisione, **sono stati studiati diversi algoritmi che prevedono la mutua esclusione**, ovvero **garantiscono che, quando la risorsa condivisa è interessata un processo, nessun altro processo possa accedervi.**

Se più processi hanno la possibilità di accedere ad una risorsa in modalità di scrittura, **è importante prevedere ed includere l'utilizzo di questi algoritmi**, dei quali **si può invece fare a meno se la risorsa è accessibile solamente in modalità di lettura**, perché i processi non possono influenzare lo stato della risorsa, quindi non è fisicamente possibile la realizzazione di situazioni di corsa.

&nbsp;
&nbsp;
&nbsp;

*Regioni critiche*
====

Come evitare le race condition? La chiave per prevenire i problemi in questo caso e in molte altre situazioni riguardanti:

-   La memoria condivisa

-   File condivisi 

-   Altri elementi condivisi 

è trovare un qualche modo per proibire a più di un processo di leggere e scrivere dati condivisi contemporaneamente. 

Ovvero un qualche sistema per essere certi che, se un processo sta usando una variabile o un file condivisi, l’altro processo venga escluso dal fare la stessa cosa. 

**La difficoltà sta nel fatto che, per esempio, il processo B ha iniziato a usare una delle variabili condivise prima che il processo A avesse terminato di usarla.**

Però, **possiamo notare che un processo è, per parte del tempo, impegnato in calcoli interni e altre attività che non portano a race condition.** 

Tuttavia, **un processo deve talvolta accedere alla memoria o a file condivisi** o svolgere attività critiche **che possono portare alle race condition.**

**La parte di programma in cui si accede alla memoria condivisa è chiamata regione critica o sezione critica.**

Se potessimo fare in modo **che due processi non si trovino mai nelle loro regioni critiche allo stesso momento, avremmo trovato la soluzione alle race condition.**

Per ottenere una buona soluzione servono **quattro condizioni**:

-   **Due processi non devono mai trovarsi contemporaneamente nelle loro regioni critiche;**

-   **Non può essere fatto alcun presupposto sulle velocità o sul numero delle CPU;**

-   **Nessun processo in esecuzione al di fuori della sua regione critica può bloccare altri processi;**

-   **Nessun processo dovrebbe restare in attesa di entrare nella sua regione critica per ­sempre.**


### **`Mutua esclusione usando le regioni critiche.`**

![alt text](https://i.imgur.com/Uk6KL7K.png)

&nbsp;
&nbsp;
&nbsp;

*Mutua esclusione con busy waiting*
====

Adesso esamineremo varie proposte per ottenere la mutua esclusione in modo che, mentre un processo è occupato nell’aggiornare la propria memoria condivisa nella sua regione critica, nessun altro processo entri nella propria regione critica e causi problemi.

**Busy Waiting**

-   Soluzione generale: si fa aspettare chi deve entrare quando la sezione critica è già occupata, valutando continuamente una variabile: `Busy waiting`

-   Si usano variabili di controllo condivise (`lock`) lette prima di entrare e scritte appena entrati in sezione critica.

>**E.g:** Se si esegue un installazione di un pacchetto tramite APT su una Debian based su un processo A e un altro processo B tenta di installare un altro pacchetto sempre tramite APT, il processo B aspetterà che la variabile di `lock` si liberi dal processo A


### **`Esempio di Busy Waiting.`**
![alt text](https://i.imgur.com/pQv7ae9.png)

****

&nbsp;
&nbsp;
&nbsp;

**Disabilitare gli interrupt**

Si può garantire l’accesso in mutua esclusione ad una sezione critica disabilitando gli interrupt.

**Pro:**

-   Utile per il kernel per aggiornare le sue variabili

**Contro:**

-   Non è raccomandabile dare all’utente il potere di disabilitare gli interrupt:

    -   Dato che è un istruzione privilegiata.

    -   L’utente potrebbe non riabilitarli più e non cedere più la CPU

-   Rallenta la risposta agli eventi

-   Inutile in un sistema multiprocessore

-   Non è strutturata e rende di difficile comprensione il codice

****

&nbsp;
&nbsp;
&nbsp;

**Soluzione di Peterson**


L'algoritmo di Peterson o algoritmo `tie-breaker` **è un algoritmo sviluppato nella teoria del controllo della concorrenza per coordinare due o più processi o thread che hanno una sezione critica in cui deve esservi mutua esclusione.**

**L'algoritmo assicura la corretta sincronizzazione, impedendo lo stallo (`deadlock`) ed assicurando che soltanto un processo alla volta possa eseguire una sezione critica (serializzazione).**

>**E.g:** Algoritmo con due processi usando `pseudocodice C`

```
 //'' dichiarazione delle variabili globali comuni''
 boolean in1 = false, in2 = false;
 int turno;
 
 // processo #1
 Process CS1 {
     while(1) {
         in1 = true;
         turno = 2;
         while (in2 && turno == 2)
            ; // finché è il turno del processo #2, il processo #1 rimane all'interno del while
         <sezione critica 1>
         in1 = false;
         <sezione non critica 1>
     }
 }
 
 // processo #2
 Process CS2 {
     while(1) {
         in2 = true;
         turno = 1;
         while (in1 && turno == 1)
            ; // finché è il turno del processo #1, il processo #2 rimane all'interno del while
         <sezione critica 2>
         in2 = false;
         <sezione non critica 2>
      }
 }
```

>**Funzionamento:**

**Se il processo #1 viene eseguito per primo, `in1` viene impostato a `true` *prima* di impostare `turno` a ``2.** 
Dal momento che `in2` è stato inizializzato a `false`, la condizione dell'iterazione while non è soddisfatta e il processo #1 accede alla sezione critica.

**Se nel frattempo viene avviato il processo #2, questo imposterà `in2` a `true` e `turno` a `1`.** **`in1` è già stato impostato a `true` dal processo #1**, perciò **la condizione dell'iterazione `while del processo #2` è soddisfatta**, così che questo deve aspettare. **Solo dopo che il processo #1 ha abbandonato la sezione critica `in1` diventa `false`, e il processo #2 può accedere alla sua sezione critica.**

**Se il processo #1 viene nel frattempo riavviato, imposterà `turno` a `2`**, e **dovrà aspettare che il processo #2 abbia abbandonato la sua sezione critica.**

-   La m**utua esclusione è garantita** dal fatto che **il controllo di `in` e di `turno` viene fatto in modo atomico.**

-  **L'assenza di deadlock viene garantita** dal fatto che solamente **una delle due condizioni while può essere vera nello stesso momento.**

-  **Il progresso dell'elaborazione è garantito** dal fatto che se uno dei due processi tenta di entrare e l'altro non è in sezione critica può tranquillamente entrare.

-   **L'attesa di un processo è limitata** in quanto, se i due processi tentano di entrare in sezione critica, entrano alternamente.

****

&nbsp;
&nbsp;
&nbsp;

**Semaforo**

Il semaforo è uno strumento di sincronizzazione dei processi.

Il semaforo è in genere una variabile intera S che viene inizializzata al numero di risorse presenti nel sistema e il valore del semaforo può essere modificato solo da due funzioni wait() e signal() oltre all’inizializzazione.

L’operazione wait() e signal() modifica indivisibilmente il valore del semaforo. Significa che quando un processo modifica il valore del semaforo, nessun altro processo può modificare contemporaneamente il valore del semaforo. 

I semafori si distinguono per il sistema operativo in due categorie: il semaforo intero e il semaforo binario.

Semaforo intero:

-   Nel semaforo intero, il valore del semaforo S viene inizializzato al numero di risorse presenti nel sistema. 

-   Ogni volta che un processo vuole accedere alla risorsa, esegue un’operazione wait() sul semaforo e diminuisce di uno il valore del semaforo. 

-   Quando rilascia la risorsa, esegue l’operazione signal() sul semaforo e incrementa il valore del semaforo di uno.

-   Quando il semaforo intero va a 0, significa che tutte le risorse sono occupate dai processi. Se un processo deve utilizzare una risorsa quando il semaforo intero è 0, esegue wait() e viene bloccato fino a quando il valore del semaforo diventa maggiore di 0.

Semaforo binario

-   Nel semaforo binario, il valore del semaforo è compreso tra 0 e 1. 

-   È simile al blocco mutex, ma il mutex è un meccanismo di blocco, mentre il semaforo è un meccanismo di segnalazione.

-   Nel semaforo binario, se un processo vuole accedere alla risorsa esegue un’operazione wait() sul semaforo e decrementa il valore del semaforo da 1 a 0. 

-   Quando rilascia la risorsa, esegue un’operazione signal() sul semaforo e incrementi il suo valore su 1. 

-   Se il valore del semaforo è 0 e un processo desidera accedere alla risorsa, esegue un’operazione wait() e si blocca fino a quando il processo corrente che utilizza le risorse rilascia la risorsa

****

&nbsp;
&nbsp;
&nbsp;

**Mutex**

**L’oggetto di esclusione reciproca è chiamato `Mutex`.** 

**Precedentemente solo un processo alla volta poteva accedere a una determinata risorsa.**

L’oggetto mutex **consente ai thread di più programmi di utilizzare la stessa risorsa ma uno alla volta non contemporaneamente.**

All’avvio di un programma, **richiede al sistema di creare un oggetto mutex per una determinata risorsa**. 

**Il sistema crea l’oggetto mutex con un nome o ID univoco.** 

**Ogni volta che il thread del programma desidera utilizzare la risorsa che occupa il blocco sull’oggetto mutex, utilizza la risorsa e dopo l’uso rilascia il blocco sull’oggetto mutex.** 

Quindi al **processo successivo è consentito acquisire il blocco sull’oggetto mutex.**

Nel frattempo, un processo ha acquisito il blocco sull’oggetto mutex a cui nessun altro thread/processo può accedere a quella risorsa. 

Se l’oggetto mutex è già bloccato, **il processo che desidera acquisire il blocco** sull’oggetto mutex **deve attendere e viene messo in coda dal sistema** fino a quando l’oggetto mutex non viene sbloccato.

****

&nbsp;
&nbsp;
&nbsp;

**Futex**

Un futex è una caratteristica che implementa un sistema di lock molto basico (**simile a un mutex**) tipica di Linux, **senza però andare a toccare il kernel, a meno che non sia assolutamente necessario.**

**Poiché passare al kernel e quindi tornare è un’operazione onerosa, questo comportamento migliora sensibilmente le prestazioni.**

Un futex è costituito di due parti:

-   Un servizio kernel:

    -   Il servizio kernel fornisce una `coda d’attesa` che consente a più processi di attendere un `lock`.

    -   I processi non sono in esecuzione a meno che il kernel non li sblocchi esplicitamente. 

    -   Inserire un processo nella coda d’attesa richiede una chiamata di sistema piuttosto onerosa; è quindi meglio evitarlo. 

    -   In assenza di contese, pertanto, il futex opera interamente in spazio utente; in particolare, i processi condividono una variabile lock comune (un intero allineato a 32 bit che funge da lock).

-   Una libreria utente.


### **`Esempio di Futex.`**
****

Supponiamo che, inizialmente, **il lock abbia valore 1, ossia che sia libero.** 

**Un thread si appropria del lock** eseguendo un’istruzione `decrement and test`.

**Il thread verifica quindi i risultati per controllare se il lock era libero o no:**

-   **Se non era in stato locked:**

    -   Va tutto bene e il thread è riuscito ad appropriarsi del lock. 

-   **Se invece il lock è detenuto da un altro thread:**

    -   Il nostro thread deve attendere. 
    
    -   In questo caso, la libreria del futex non entra in spin lock, ma utilizza una chiamata di sistema per posizionare il thread nella coda di attesa all’interno del kernel.

A questo punto, **il costo del passaggio al kernel dovrebbe essere giustificato**, perché il thread sarebbe stato comunque bloccato. 

**Quando un thread ha terminato di utilizzare il lock, lo rilascia con un’istruzione atomica `increment and test`** e **verifica il risultato per sapere se nella coda di attesa del kernel sono presenti altri processi bloccati.**

-   **In caso positivo (ci sono contese):**

    -  *Chiama il Kernel in causa e gli fa sapere che può sbloccare uno o più di questi processi.

-   **In caso negativo (non ci siano contese):**

-   Il kernel non viene chiamato in causa.
****

&nbsp;
&nbsp;
&nbsp;

# **Scheduling**

>***DEF:*** *Lo scheduler dei processi è quel **componente del sistema operativo** che si occupa di **decidere quale processo va mandato in esecuzione**. Nell'ambito della multiprogrammazione è pensato per **mantenere la CPU occupata il più possibile**.*

&nbsp;
&nbsp;
&nbsp;

>***E.g:*** *Ad esempio avviando un processo mentre un altro è in attesa del completamento di un'operazione di I/O.*

&nbsp;
&nbsp;
&nbsp;

*Comportamento tra processi*
======

Durante la sua attività, un processo generalmente alterna
le seguenti fasi:

>`CPU burst`: fase in cui viene **impiegata soltanto la CPU senza I/O**;

>`I/O burst`: fase in cui il processo **effettua input/output da/verso una risorsa (dispositivo) del sistema**.


>***DEF:*** **`burst` (raffica)** *indica una sequenza di istruzioni.*

&nbsp;
&nbsp;
&nbsp;

![alt text](https://mediaserver.pearsonitalia.it/mediaserver_uni/books/prod/2017/9788891901026_TANENBAUM/EPUB/ASSETS/images/02-38.png)

Alcuni processi, come quelli nel­la **Figu­ra `a`** **spendono** la maggior parte del loro **tempo in elaborazione (uso CPU)**, mentre altri, come quelli nella **Figura `b`**, **spendono** la maggior parte del loro **tempo in atte­sa del­l’I/O.** 

**La prima situazione è chiamata `compute-bound` o `CPU-bound`, la seconda `I/O-bound`.** 

>***DEF:*** *I processi* **`compute-bound`** *hanno in genere **burst di CPU lunghi e attese di I/O poco frequenti***

>***DEF:*** *I processi* **`I/O-bound`** ***hanno burst di CPU brevi e attese di I/O frequenti.***

>***NOTA:*** *Quando un processo esegue `I/O burst`, **non utilizza la CPU**. In un sistema multiprogrammato, **lo scheduler assegna la CPU a un nuovo processo**.*

&nbsp;
&nbsp;
&nbsp;

*Categorie Algortimi di Scheduling*
======

Gli algoritmi di scheduling si possono classificare in **due categorie**:

>**Con prelazione (`pre-emptive`):** ***al processo in esecuzione può essere revocata la CPU***. *Il SO può, in base a determinati criteri, **sottrarre ad esso la CPU per assegnarla ad un nuovo processo.***

>**Senza prelazione (`non pre-emptive`):** ***la CPU rimane allocata al processo in esecuzione fino a quando esso si sospende volontariamente** (ad esempio, per I/O), **o termina**.*

&nbsp;
&nbsp;
&nbsp;

*Parametri dello Scheduling*
======

Per analizzare e confrontare i diversi algoritmi di scheduling, si considerano i seguenti parametri, i primi tre relativi ai processi e gli altri relativi al sistema:

>**Tempo di attesa (`Wait time`):** *è la **quantità di tempo che un processo trascorre nella coda di pronto, in attesa della CPU.***

>**Tempo di completamento (`Turnaround time`)**: *è **l’intervallo di tempo che passa tra l’avvio del processo e il suo completamento.***

>**Tempo di risposta:** *è **l’intervallo di tempo tra avvio del processo e l’inizio della prima risposta.***

>**Utilizzo della CPU:** *esprime la **percentuale media di utilizzo della CPU nell’unità di tempo**.*

>**Produttività (`Throughput rate`):** *esprime il n**umero di processi completati nell’unità di tempo.***

&nbsp;
&nbsp;
&nbsp;

Generalmente i parametri che devono essere massimizzati sono:

>**Utilizzo della CPU (al massimo: 100%)**

>**Produttività (`Throughput`)**

Invece, devono essere minimizzati:

>**Tempo medio di completamento (`Turnaround`) (soprattutto nei sistemi batch)**

>**Tempo medio di attesa**

>**Tempo medio di risposta (soprattutto nei sistemi interattivi)**

### **`[!] Non è possibile ottenere tutti gli obiettivi contemporaneamente.`**

&nbsp;
&nbsp;
&nbsp;

*Categorie dei Sistemi dello Scheduler*
======

Inoltre poiché differenti aree di applicazione (e differenti sistemi operativi) hanno obiettivi diversi. In altre parole, quello che lo scheduler dovrebbe ottimizzare non è la medesima cosa in tutti i sistemi. 

Tre sistemi che vale la pena distinguere sono i seguenti: **`batch`, `interattivo`, `real-time`**.

&nbsp;
&nbsp;
&nbsp;


    Sistemi Batch

>Sistemi che fanno prevalentemente calcolo (**uso CPU massimo e massimo throughput**) e non fanno quasi **mai operazioni di I/O** e **minimizzano il tempo medio di completamento**.

&nbsp;
&nbsp;
&nbsp;

>****** 
### **`[i] DESCRIZIONE OPZIONALE.`**


I sistemi batch sono ancora diffusamente usati nel mondo degli affari per l’elaborazione di paghe, inventari, esecuzione di accrediti e addebiti su conti corrente, calcolo degli interessi bancari, gestione dei reclami (nelle compagnie assicurative) e altre attività periodiche.

Nei sistemi batch non ci sono utenti impazienti in attesa al loro terminal di una risposta rapida a una piccola richiesta. Di conseguenza sono spesso accettabili algoritmi non preemptive o algoritmi preemptive con lunghi periodi per ciascun processo.
Un simile approccio riduce gli scambi di processo e migliora così le prestazioni. 

Gli algoritmi batch sono effettivamente abbastanza generali e spesso applicabili ugualmente ad altre situazioni, il che fa sì che valga la pena studiarli, anche da parte di persone non necessariamente coinvolte in elaborazioni di mainframe aziendali.
>****** 


&nbsp;
&nbsp;
&nbsp;


    Sistemi Interattivi


>Qui si tiene a **minimizzare il tempo medio di risposta e attesa dei processi**.

&nbsp;
&nbsp;
&nbsp;

>****** 
### **`[i] DESCRIZIONE OPZIONALE.`**
In un ambiente con utenti interattivi, l’uso della preemption è essenziale per evitare che un processo monopolizzi la CPU e neghi il servizio agli altri. Anche se intenzionalmente nessun processo viene eseguito all’infinito, a causa di un errore di programma un processo potrebbe escludere tutti gli altri indefinitamente. La preemption è necessaria per prevenire questi comportamenti. Anche i server ricadono in questa categoria, dato che normalmente servono molteplici utenti (remoti), e tutti sempre di fretta.
>****** 

&nbsp;
&nbsp;
&nbsp;


    Sistemi Real-Time

>Nei sistemi real-time, l’algoritmo con revoca non sempre è necessario dato che i **programmi real-time sono progettati per essere eseguiti per brevi periodi di tempo e poi si bloccano**. Inoltre si tende di più ad avere il **Rispetto delle scadenze e Prevedibilità**.

&nbsp;
&nbsp;
&nbsp;

>****** 
### **`[i] DESCRIZIONE OPZIONALE.`**
In sistemi con vincoli real-time, la preemption è, stranamente, non sempre necessaria, poiché i processi sanno che non possono essere eseguiti per lunghi periodi di tempo e generalmente fanno il loro lavoro e si bloccano rapidamente. La differenza con i sistemi interattivi è che i sistemi real-time eseguono solo programmi destinati a promuovere l’applicazione a portata di mano. I sistemi interattivi sono generalistici e possono eseguire arbitrariamente programmi che non cooperano o che addirittura mirano a fare danni.
>****** 

&nbsp;
&nbsp;
&nbsp;

### **`Riassunto`**

![alt text](https://i.imgur.com/8jFPvoy.png)

&nbsp;
&nbsp;
&nbsp;

*Principali algoritmi di scheduling*
====

Alcuni algoritmi sono usati sia nei sistemi batch che nei sistemi interattivi.

&nbsp;
&nbsp;
&nbsp;

    FCFS (First Came First Served)

Questo è l’algoritmo più semplice.

La CPU è assegnata ai processi seguendo l’ordine con cui essa è stata richiesta**, ovvero la cpu è assegnata al processo che è in attesa da più tempo nello stato di pronto. 

Quando un processo acquisisce la CPU, resta in esecuzione fino a quando si blocca volontariamente o termina. 

Quando un processo in esecuzione si blocca si seleziona il primo processo presente nella coda di pronto.

Quando un processo da bloccato ritorna pronto, esso è accodato nella coda di pronto.

E’ inefficiente nel caso in cui ci sono molti processi che si sospendono frequentemente o nel caso di sistemi interattivi. 
Il tempo di attesa risulterebbe troppo lungo. 

**E’ un algoritmo senza diritto di prelazione.**

>**E.g:** Calcoliamo il tempo medio di attesa di tre processi `P1`, `P2` e `P3` avviati allo stesso istante e aventi rispettivamente i tempi (in millisecondi) di completamento pari a: **`T1`=30, `T2`=6 e `T3`=10**.
### **`Esempio sui processi`**
![alt text](https://i.imgur.com/xBTRKrA.png)

**Il tempo di attesa medio = (0+30+36)/3= 22**.

Se cambiassimo l’ordine di scheduling nel seguente: {`P2,P3, P1`} il tempo medio di attesa passerebbe da **22 a 7,33**.

### **`Esempio Cambio Ordine Processi`**
![alt text](https://i.imgur.com/GgwiBrV.png)

**Ma il FCFS non consente di cambiare l’ordine dei processi.**

Quindi nel caso in cui ci siano processi in attesa dietro a processi con lunghi CPU burst (processi CPU bound), il tempo di attesa sarà alto. 

Inoltre, si ha la possibilità che si verifichi il cosiddetto ***effetto convoglio*** che si ha se molti processi I/O bound seguono un processo CPU bound.
**Questa situazione porta ad basso livello di utilizzo della CPU.**

### **`Esempio Effetto Convoglio`**
![alt text](https://i.imgur.com/WTkYWMs.png)
>****** 

&nbsp;
&nbsp;
&nbsp;

    Shortest job first (SJF)

Gli algoritmi SJF, **possono essere sia non-preemptive (SNPF) sia preemptive (SRTF)**.

>****** 

&nbsp;
&nbsp;
&nbsp;

    Shortest Next Process First (SNPF)

L’algoritmo SNPF prevede che sia **eseguito sempre il processo con il tempo di esecuzione più breve** tra quelli pronti.

>E.g: Supponiamo ad esempio che si trovino nello stato di pronto i seguenti processi, con la rispettiva durata di esecuzione in millisecondi:

![alt text](https://i.imgur.com/T3hzhHe.png)

&nbsp;
&nbsp;
&nbsp;

Di conseguenza con SNPF, l'ordine dei processi diventa così.

![alt text](https://i.imgur.com/k1jDw0I.png)

P2 non attende nulla, perché va subito in esecuzione, P4 attende 2 millisecondi, perché va in esecuzione subito dopo P2, quindi P3 attende 6 millisecondi e P1 ne attende 12. 

Il tempo di attesa medio è pari a:

Tempo attesa medio = **(0+2+6+12)/4 = 5 millisecondi**

>****** 

&nbsp;
&nbsp;
&nbsp;

    Shortest Remaining Time First (SRTF)

L'algoritmo SRTF è la versione preemptive del precedente.

Con SRTF, se un **nuovo processo**, entrante nella coda di pronto, **ha una durata minore del tempo restante al processo in esecuzione** per portare a terminare il proprio CPU-burst, allora **lo scheduler provvede ad effettuare un cambio di contesto e assegna l’uso della CPU al nuovo processo.**

Ovvero, quando arriva un nuovo processo nella Ready List viene stimato il tempo di esecuzione che rimane al processo in possesso della CPU e lo si confronta con il tempo previsto del nuovo processo; **se quest’ultimo è minore si effettua il cambio di contesto** mandando in esecuzione il nuovo processo.

## **Esempio:**

### **`Esempio Lista Processi prima SRTF`**
![alt text](https://morethaninfo.com/wp-content/uploads/2021/04/Cattura6.png)

All’istante 0 il processo P1 viene messo nello stato di READY e, **non essendoci altri processi ad occupare la CPU viene mandato in esecuzione.**

P2 viene inserito nella Ready List all’istante 2, con 6 istanti d'esecuzione.

Questo dato viene confrontato con il tempo di esecuzione rimanente a P1, ovvero 1 istante (3 – 2) che essendo minore di 6 **non fa scaturire il cambio di contesto**.

P2 va in esecuzione all’istante 3, quando P1 termina (senza nessun confronto, visto che non ci sono altri processi nella Ready List).

All’istante 4 arriva in Ready List il processo P3, con tempo di esecuzione pari a 4, **un tempo minore rispetto a quello rimanente di P2** (ovvero 5, 6-1).

In questo caso **il sistema operativo procede con il cambio di contesto assegnando quindi la CPU a P3. P2 torna in Ready List con un tempo di esecuzione pari a 5.**

### **`Lista Processi con scambio d'ordine`**
![alt text](https://morethaninfo.com/wp-content/uploads/2021/04/Grafico12-1-960x434.png)

Questo procedimento continua **finchè i processi precedenti non finisco e vengono ri-eseguiti quelli che erano stati rimessi nella Ready List** (P2 e P5).

Alla fine si avrà questo risultato.

### **`Lista Processi dopo SRTF`**
![alt text](https://morethaninfo.com/wp-content/uploads/2021/04/Grafico16-960x314.png)
>****** 

&nbsp;
&nbsp;
&nbsp;

    Round Robin

E’ stato realizzato **per i sistemi time-sharing**.

**Consente il prerilascio e utilizza la modalità FIFO.** (ovvero una lista in cui i processi sono ordinati **secondo l’ordine di arrivo**)

Al **primo processo in coda viene assegnata la CPU per un periodo di tempo prefissato** (`time slice`).

**Se il processo termina prima** della fine del quanto di tempo **lascia volontariamente la CPU** (che verrà assegnata al nuovo primo processo in coda), **se non è così viene rimosso** dal sistema operativo **e posto in fondo alla coda** (anche in questo caso l’esecuzione passerà al primo processo nella coda).

&nbsp;
&nbsp;
&nbsp;

## **Esempio:**

Supponiamo che all’istante T0 siano presenti i seguenti quattro processi nella coda di pronto. Calcoliamo il tempo medio di attesa e di risposta per quattro processi P1, P2, P3 e P4 aventi rispettivamente i tempi di arrivo e durata di CPU-burst (in millisecondi) pari a:

>**`P1` (`10ms`), `P2` (`100ms`), `P3` (`50ms`), `P4` (`20ms`)**

>****** 
&nbsp;
&nbsp;
&nbsp;


        Usando FCFS

### **`Intervallo di tempo usando FCFS`**
![alt text](https://i.imgur.com/LrPgCj1.png)

**Usando FCFS** si hanno questi valori:

| Processo 	| Tempo di Attesa 	| Tempo di Risposta 	|
|----------	|-----------------	|-------------------	|
| P1       	| 0               	| 10                	|
| P2       	| 10              	| 110               	|
| P3       	| 110             	| 160               	|
| P4       	| 160             	| 180               	|

Quindi:

>**Attesa Medio** = (P1 + P2 + P3 + P4)/nP = **70**

>**Risposta Medio** = (P1 + P2 + P3 + P4)/nP = **115**

>****** 
&nbsp;
&nbsp;
&nbsp;

        Usando RR


**Viene impostato un quanto di tempo (`Δ`) = `20 ms`** si ha questo instante:

### **`Intervallo di tempo usando RR`**
![alt text](https://i.imgur.com/3Lt3h1s.png)

**Usando RR** si hanno questi valori:

&nbsp;
&nbsp;
&nbsp;
&nbsp;
&nbsp;
&nbsp;

| Processo 	| Tempo di Attesa     	| Tempo di Risposta 	|
|----------	|---------------------	|-------------------	|
| P1       	| 0                   	| 10                	|
| P2       	| (10)+(40)+(20)+(10) 	| 180               	|
| P3       	| (30)+(40)+(20)      	| 140               	|
| P4       	| 50                  	| 70                	|

Quindi

>**Attesa Medio** = (P1 + P2 + P3 + P4)/nP = **55**

>**Risposta Medio** = (P1 + P2 + P3 + P4)/nP = **100**



