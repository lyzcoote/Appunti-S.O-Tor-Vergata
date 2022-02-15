# 3.1 Nessuna astrazione di memoria

La più semplice astrazione della memoria è l’assenza di astrazione. 

Dai primi mainframe fino ai primi personal computer, essi non avevano astrazione della memoria. 

Ogni programma vedeva semplicemente la memoria fisica. 

Quando un programma eseguiva un’istruzione come `MOV REGISTER1,1000` il computer spostava semplicemente il contenuto della locazione di memoria fisica 1000 a REGISTER1. 

Con questo metodo però, un altro programma avrebbe sovrascritto quello precendete.

Prendiamo adesso un esempio di una memoria con una migliore astrazione e gestione.


### **`Tre modi per organizzare la memoria con un sistema operativo e un processo utente.`**

![alt text](https://mediaserver.pearsonitalia.it/mediaserver_uni/books/prod/2017/9788891901026_TANENBAUM/EPUB/ASSETS/images/03-01.png)

Nella Figu­ra soprastante **sono illustrate tre varianti:**

-   **Il sistema operativo può trovarsi sul fondo della memoria, nella `RAM`** come mostrato nella Figu­ra (`a`), 

-   **Il sistema operativo può trovarsi in cima alla memoria nella `ROM`** come mostrato nella Figura (`b`), 

-  **I driver dei dispositivi possono essere in cima alla memoria nella `ROM` e il resto del sistema nella `RAM` subito sotto**, come nella Figura (`c`). 

Il primo modello fu usato inizialmente sui mainframe e sui minicomputer, ma ormai è utilizzato raramente. 

Il secondo modello è usato su alcuni computer palmari e sistemi integrati. 

Il terzo modello è stato usato sui primi personal computer, dove la parte del sistema nella `ROM` è chiamata `BIOS`. 

&nbsp;
&nbsp;
&nbsp;

# 3.2 Spazio degli Indirizzi

Una soluzione migliore è inventare una nuova astrazione per la memoria: **`lo spazio ­degli indirizzi`.**

**Così come il concetto di processo crea una specie di CPU astratta per eseguire i programmi, lo spazio degli indirizzi crea una specie di memoria astratta per farci vivere i programmi.** 

>**DEF**: Uno spazio degli indirizzi è l’insieme degli indirizzi che un proces­so può usare per indirizzare la memoria.

**Ogni processo ha il suo spazio degli indirizzi ­personale, ­indipendente da quello appartenente ad altri processi** (a eccezione di alcune circostanze speciali in cui i processi vogliono condividere i loro spazi degli indirizzi).

**Spazio di indirizzamento della CPU = Max quantità di celle indirizzabili = 2°a (dove a è la dimensione in bit degli indirizzi)**

>**E.g:** Una `CPU` a `32 bit` ha uno spazio degli indirizzi massimo equivalente a `4294967295`

&nbsp;
&nbsp;
&nbsp;

*Registri base e registri limite*
====

È necessario garantire che ogni processo acceda solo alla sua area di memoria (a meno di condivisioni volute).

Quindi si utilizzano due registri:

-   **`Registro base:`**

    -   *Contiene il più piccolo indirizzo fisico ammesso (di solito `0`)*


-   **`Registro limite:`**

    -   *Contiene la dimensione dell’intervallo ammesso*

        -   ***Solo il SO può accedere a questi registri ed impedisce ai programmi utente di modificarli*** 


### **`Esempio di un layout di memroria`**

![alt text](https://i.imgur.com/7b2Ck64.png)

&nbsp;
&nbsp;
&nbsp;

*Separazione degli spazi di indirizzamento*
====


**Quando un processo utente cerca di accedere ad un indirizzo la CPU lo confronta con i valori dei due registri.**

**Ogni tentativo da parte di un processo utente di accedere ad un’area non concessa della memoria porta ad un’eccezione:**

-   Il controllo viene restituito al SO

-   Passaggio da modalità utente a modalità Kernel

**Il SO non ha limiti sulle aree di memoria a cui può accedere.**

&nbsp;
&nbsp;
&nbsp;

*Swapping*
====

**Un processo può essere temporaneamente scambiato (swapped), ovvero spostato dalla memoria centrale ad una memoria secondaria (area di swap) e poi in seguito riportato interamente in memoria centrale per continuarne l’esecuzione.**

**Questa tecnica viene chiamata `Swapping`**

>**NOTA:** Come Memoria Secondaria si intende un unità di memoriazione locale abbastanza veloce e capiente, dato che de accogliere le copie di tutte le immagini della memoria centrale per tutti gli utenti.

### **`Esempio di un layout di swapping`**

![alt text](https://i.imgur.com/xxhz9rz.png)

**Inoltre, lo swapping permette di gestire più processi di quelli che fisicamente sarebbero caricabili in memoria.**

**Il periodico scambio tra processi in memoria centrale e secondaria (`swapping`) è controllato dallo scheduler a medio termine.**

**Lo swapping è molto comune nei sistemi con schedulatore Round Robin.**

-   Il processo che finisce il quanto di tempo subisce lo swap-out.

**`Roll out`, `roll in` è una variante dello swapping usata per algoritmi di schedulazione basati sulla priorità.**

-   ***un processo a bassa priorità è scambiato con un processo ad alta priorità.***, in modo che quest’ultimo possa essere caricato ed eseguito.

La zona di memoria in cui i processi che subiscono lo swap-in vengono spostati dipende dalla fase in cui gli indirizzi logici vengono associati a quelli fisici.

**Lo swapping deve tenere conto anche di possibili I/O.**

-   Se i processi sono impegnati in un I/O asincrono non possono essere avvicendati.

&nbsp;
&nbsp;
&nbsp;

# 3.3 Memoria Virtuale

**La memoria virtuale è un'architettura di sistema capace di simulare uno spazio di memoria centrale (memoria primaria) maggiore di quello fisicamente presente o disponibile, dando l'illusione all'utente di un enorme quantitativo di memoria.**

**Il sistema operativo usando una combinazione di hardware e software, mappa gli indirizzi di memoria usati da un programma, chiamati indirizzi virtuali, in indirizzi fisici.** 

**La memoria principale, dal punto di vista di un processo, appare come uno spazio di indirizzi (o una lista di segmenti) contiguo.**

Il sistema operativo gestisce lo spazio virtuale e l'assegnamento di memoria fisica a memoria virtuale. 

**La traduzione di indirizzi è fatta da un hardware presente nella CPU, comunemente chiamato `MMU` (*Memory Managment Unit*), che traduce i vari indirizzi.**

Software presente all'interno del sistema operativo può estendere queste funzionalità per offrire uno spazio virtuale che eccede quello della memoria fisica, indirizzando così più memoria di quella fisicamente presente nel computer.

**La memoria virtuale funziona bene in un sistema multiprogrammazione**, con bit e pezzi di molti programmi contemporaneamente in memoria. 

**Mentre un programma è in attesa che un suo pezzo sia letto, la CPU può essere assegnata a un altro processo.**

&nbsp;
&nbsp;
&nbsp;

*Paginazione*
====

**I meccanismi a partizionamento fisso/dinamico non sono efficienti nell’uso della memoria.**

**La paginazione è l’approccio utilizzato nei SO moderni per:** 

-   *Ridurre il fenomeno di frammentazione esterna allocando ai processi spazio di memoria non contiguo*.

-   *Si tiene in memoria solo una porzione del programma*.

    -   *Aumento del numero dei processi che possono essere contemporaneamente presenti in memoria*.

-   *La possibilità di eseguire un processo più grande della memoria disponibile (memoria virtuale)*.

**Suddivide la memoria fisica (memoria centrale) in blocchi di frame della stessa dimensione (una potenza di `2`, fra `512 byte` e `16 MB`)**.

-   ***Lo spazio degli indirizzi fisici può essere non contiguo***.

Divide la memoria logica in blocchi delle stesse dimensioni dei frame chiamati pagine .

-  *Il processo è sempre allocato all’interno della memoria logica in uno **spazio contiguo***.

Per eseguire un processo di dimensione di n pagine, bisogna trovare n frame liberi e caricare il programma.

Il SO imposta una tabella delle pagine per tradurre gli indirizzi logici in indirizzi fisici.

&nbsp;
&nbsp;
&nbsp;

Ogni indirizzo logico generato dalla CPU è diviso in due parti: 

-   Numero di pagina (p):

    -   Usato come indice nella tabella delle pagine che contiene l’indirizzo di base di ogni frame nella memoria fisica.

-   Spiazzamento nella pagina (d):

    -   Combinato con l’indirizzo di base per calcolare l’indirizzo di memoria fisica che viene mandato all’unità di memoria centrale.


### **`Esempio di un layout di un indirizzo logico`**

![alt text](https://i.imgur.com/E7P5Z6v.png)
****


    Esempio di paginazione

**La tabella delle pagine associa l’indirizzo di una pagina logica al corrispettivo indirizzo della pagina nella memoria fisica**

![alt text](https://i.imgur.com/3dAFdaY.png)

**Le aree di memoria dei processi possono essere interfogliate**

![alt text](https://i.imgur.com/8znaQhb.png)

****

&nbsp;
&nbsp;
&nbsp;

La dimensione di una pagina varia dai 512 byte ai 16MB

La dimensione della pagina è definita dall’architettura del sistema ed è in genere una potenza di 2

-   Perché semplifica la traduzione degli indirizzi

Infatti, se lo spazio logico di indirizzamento è `2^m` e la dimensione di pagina è `2^n unità`, allora: 

-   `m-n bi`t più significativi dell’ind. logico **indicano la `pagina p`**

-   `n bit` meno significativi dell’ind. logico **indicano lo scostamento di `pagina d`**

![alt text](https://i.imgur.com/H2YEjwS.png)

&nbsp;
&nbsp;
&nbsp;

*Tabella dei frame*
====

Oltre alla **tabella delle pagine** (logiche) di ciascun processo, **il SO mantiene un’unica tabella dei frame** (pagine fisiche): 

-   Contiene un elemento per ogni frame, che indica se questo è libero o allocato, e (se allocato) a quale pagina di quale processo o processi.

&nbsp;
&nbsp;
&nbsp;

*Implementazione della tabella delle pagine*
====

**La maggior parte dei sistemi usa una tabella delle pagine per ogni processo.**

-   **`Prima soluzione:`**

    -   **Si usa uno specifico insieme di registri per salvare la tabella**.

    -   **Durante il context switch i valori della tabella delle pagine del processo vengono caricate nei registri**.

    -   *Molto veloce*.

    -   **Ma va bene solo per tabelle con pochi elementi** (`256` circa, quando tipicamente ne servono `1.000.000`).

-   **`Seconda soluzione:`**

    -   **La tabella delle pagine viene mantenuta in memoria centrale** .

    -   **Un registro chiamato `PTBR`** (page-table base register) **punta alla tabella del processo corrente**.

    -   *Cambiare tabella delle pagine significa cambiare valore del registro*.

    -   🔴 Problema: 
    
        -   *Per accedere ad un dato occorrono due accessi alla memoria centrale:*

            -   1 per la tabella delle pagine.

            -   1 per il byte stesso.

    -   **L’accesso alla memoria è rallentato di un fattore 2** (*sarebbe preferibile usare lo swapping*) .

-   **`Terza soluzione:`**

    -   **Si utilizza una cache associativa: `TLB`** (*translation look-aside buffer*).

    -   **Associa ad una chiave un valore**:

        -   *Chiave*: 
        
            -   Indirizzo logico .
            
        -   *Valore*: 

            -   Indirizzo fisico.

        -   **Molto rapida ma anche costosa e piccola** (*max `1024` elementi*).

    -  **I record possono essere scritti e sovrascritti**.

    -  **Ma possono anche essere vincolati** (***non modificabili***) .

    -   **Utilizzo:**

        -   *La `TLB` contiene una piccola parte delle righe della tabella delle pagine*.

&nbsp;
&nbsp;
&nbsp;


![alt text](https://i.imgur.com/bkSN7BT.png)

**Dato un indirizzo logico lo si passa alla `TLB`, se essa contiene tale chiave restituisce l’indirizzo fisico**

Altrimenti (`TLB Miss`) si cerca nella tabella residente in memoria centrale:

-   **Si copiano poi i due indirizzi nella `TLB` in modo da velocizzare accessi futuri**

-   *Se la tabella è piena si sceglie quale record togliere secondo diversi criteri* 
>**E.g:** Elemento usato meno di recente, casuale

![alt text](https://i.imgur.com/LC31WbJ.png)

&nbsp;
&nbsp;
&nbsp;

# 3.4 Algoritmi di sostituzione delle pagine

Elenco algortimi disponibili: `Ottimale, NRU, FIFO, Seconda chance, Clock, LRU, NFU, Aging, Working Set, WSClock`

**Il criterio di valutazione è il numero di `page fault`. Tanto è minore il numero di page fault, tanto più efficace è l'algoritmo.**

&nbsp;
&nbsp;
&nbsp;

*Ottimale*
====

**L'algoritmo Ottimale (`OPT`) è un algoritmo teorico, impossibile da implementare, in quanto richiede una conoscenza perfetta degli eventi futuri da parte del sistema operativo**, l'OPT, infatti, seleziona la pagina che (in futuro) non verrà usata per il periodo di tempo più lungo. 

**L'algoritmo ottimale viene impiegato fondamentalmente come algoritmo di riferimento**, *utile per misurare e giudicare le prestazioni* (tramite comparazione) *degli algoritmi implementabili praticamente*, come l'algoritmo di sostituzione `FIFO` o quello `LRU`.

&nbsp;
&nbsp;
&nbsp;

*NRU*
====

L’algoritmo `NRU` (*Not Recently Used*) **suddivide le pagine in quattro classi che dipendono dallo stato dei bit `R` e `M`.** **Viene scelta una pagina a caso fra le pagine della classe con la numerazione ­inferiore.** **È un algoritmo semplice da implementare, ma molto grezzo (non efficente).**

&nbsp;
&nbsp;
&nbsp;

*FIFO*
====

Un algoritmo di paginazione con basso overhead è il `FIFO` (*first-in, first-out*). 

>**E.g**: Consideriamo *un supermercato che abbia abbastanza scaffali da poter esporre esattamente un numero `k` di prodotti diversi*. Un bel giorno, *un’azienda introduce un nuovo prodotto alimentare* – biologico, istantaneo, essiccato, che può essere riportato allo stato naturale nel forno a microonde. *Ha un successo così immediato che il supermercato deve disfarsi di un vecchio prodotto per fornirsene in magazzino.*

*Una possibilità è di rintracciare il prodotto che il supermercato ha in magazzino da più tempo* (probabilmente qualcosa che ha iniziato a vendere molti anni prima) *ed eliminarlo poiché nessuno se ne interessa più*

Effettivamente *il supermercato tiene una lista collegata di tutti i prodotti in vendita nell’ordine in cui sono stati introdotti*. 

**Quello nuovo va in fondo alla lista; il primo della lista è eliminato.**

**Questo stesso esempio è applicabile anche come algoritmo di sostituzione delle pagine.** 

*Il sistema operativo tiene una lista delle pagine attualmente in memoria, con l’arrivo più recente in coda e il più vecchio in testa.*

**A un `page fault`, la pagina di testa è rimossa e la nuova aggiunta in coda all’elenco**.

**Quando il `FIFO` è applicato ai supermercati rimuove articoli quali la cera per i baffi, ma potrebbe anche rimuovere farina, sale o burro.** 

Quando applicato nei computer sorge lo stesso problema. 

**Per questo motivo il `FIFO` è raramente usato nella sua forma più rigida.**

&nbsp;
&nbsp;
&nbsp;

*Seconda Canche*
====

**L’algoritmo `seconda chance` è una modifica del `FIFO` che controlla se la pagina è in uso prima di rimuoverla.** 

**Se lo è, la pagina è risparmiata. Questa modifica incrementa enormemente le prestazioni.**

&nbsp;
&nbsp;
&nbsp;

*Clock*
====

**L’algoritmo `Clock` è semplicemente una diversa implementazione del `Seconda chance`. Ha le stesse proprietà prestazionali, ma impiega leggermente meno tempo a eseguire l’algoritmo.**

**Ovvero usa un approccio migliore, cioè quello di tenere tutti i frame su una lista circolare a forma di orologio, come mostrato nella Figura sottostante.** 

![alt text](https://mediaserver.pearsonitalia.it/mediaserver_uni/books/prod/2017/9788891901026_TANENBAUM/EPUB/ASSETS/images/03-16.png)

**La lancetta indica la pagina più vecchia.** 

**Questo processo è ripetuto finché non viene trovata una pagina con `R = 0`.**

&nbsp;
&nbsp;
&nbsp;

*LRU*
====

*Una buona approssimazione dell’algoritmo ottimale si basa sull’osservazione che le pagine usate più frequentemente nelle ultime istruzioni lo saranno anche nelle successive. Al contrario quelle inutilizzate da anni lo resteranno ancora per molto.*

Questa strategia si chiama paginazione `LRU` (*least recently used*). 

*Sebbene questo algoritmo sia teoricamente fattibile, non è economico.* **Per implementare pienamente `l’LRU` è necessario mantenere una lista collegata di tutte le pagine in memoria, con davanti quelle più usate e dietro quelle meno usate.** 

**La difficoltà sta nel fatto che l’elenco deve essere aggiornato a ogni riferimento alla memoria. Trovare una pagina in memoria, cancellarla e poi portarla davanti è un’operazione che costa del tempo, anche se eseguita nell’hardware (dando per assunto che un hardware del genere sia realizzabile).**

L’LRU è un algoritmo eccellente, ma non può essere implementato senza un hardware speciale.

&nbsp;
&nbsp;
&nbsp;


*Working Set*
====

**Si prevede il set di pagine di cui il processo da caricare ha bisogno per la prossima fase di esecuzione.**

**L’algoritmo del `working set` fornisce prestazioni ragionevoli, ma è in un certo modo costoso da implementare.**

&nbsp;
&nbsp;
&nbsp;


*WSClock*
====

**L'algortimo `WSClock` è un mix tra il `Working Set` e il `Clock`. Grazie alla sua semplicità di implementazione e alle buone prestazioni, nella pratica è diffusamente usato**.

*La struttura dati necessaria è una lista circolare di frame,* **come nell’algoritmo `Clock`** *e co­me illustrato nella Figura (`a`).*

*Inizialmente questo elenco è vuoto. Quando è caricata la pri­ma pagina, essa viene aggiunta all’elenco. Nel venir aggiunte più pagine, queste vanno nel­l’e­­lenco a formare un cerchio.* 

*Ogni voce contiene il campo Tempo di ultimo utilizzo* (*time of last use*) *dall’algoritmo base del `working set`, così come il bit `R`* (*mostrato*) *e il bit `M`* (*non ­mostrato*)*.*

**Come nell’algoritmo `Clock`, a ogni page fault quella indicata dalla lancetta dell’orologio è esaminata per prima. Se il bit `R è 1`, la pagina è stata usata nel ciclo del clock, quindi non è la candidata ideale alla rimozione. Il bit `R` è impostato a `0`, la lancetta avanza alla pagina successiva e l’algoritmo rieseguito per la nuova pagina. La situazione dopo questa sequenza è mostrata nella Figura (`b`).**

![alt text](https://mediaserver.pearsonitalia.it/mediaserver_uni/books/prod/2017/9788891901026_TANENBAUM/EPUB/ASSETS/images/03-21.png)

**Consideriamo adesso che cosa accade se la pagina indicata ha `R = 0`, come mostrato nella Figura (`c`)**. 

-   **Se almeno un pageout è stato schedulato, si continua a girare** (aspettando che le pagine schedulate vengano salvate).

-   **Altrimenti, significa che tutte le pagine sono nel working set**. Soluzione semplice: 
    
    -   *Si rimpiazza una qualsiasi pagina pulita.*

-   **Se non ci sono neanche pagine pulite, si rimpiazza la pagina corrente.**


&nbsp;
&nbsp;
&nbsp;


*Riepilogo*
====

| **Algortimo**  	| **Commento**                                                        	|
|----------------	|---------------------------------------------------------------------	|
| Ottimale       	| Non implementabile, ma utile come indice di confronto e valutazione 	|
| NRU            	| Approssimazione molto rozza dell’LRU                                	|
| FIFO           	| Potrebbe eliminare pagine importanti                                	|
| Seconda Chance 	| Deciso miglioramento rispetto al FIFO                               	|
| Clock          	| Realistico                                                          	|
| LRU            	| Eccellente, ma difficile da implementare con precisione             	|
| NFU            	| Approssimazione abbastanza rozza dell’LRU                           	|
| Aging          	| Algoritmo efficiente che approssima bene l’LRU                      	|
| Working set    	| Piuttosto dispendioso da implementare                               	|
| WSClock        	| Algoritmo efficiente e buono                                        	|


