
#CAPITOLO 4

###Funzioni fondamentali di un servizio di comunicazione di livello di rete:

1. **inoltro**
2. **frammentazione** e **riassemblaggio**
3. **instradamento**
4. **segnalazione**, alla sorgente, **di situazioni di errore o di anomalia**, in modo che possano essere evitate e corrette.

**Perché occorre definire un indirizzo internet e non si possono usare gli indirizzi delle reti fisiche?**
Perché:

- nelle reti fisiche gli indirizzi sono di diverso tipo
- alcune reti fisiche non hanno indirizzi universali


**Un indirizzo deve individuare univocamente una macchina.** Una macchina può avere **più di un indirizzo IP**, purché questi siano univoci.

Indirizzo IP formato da:

- NETid (**UNIVOCO**, ma una macchina può averne più di uno) = identifica la rete fisica a cui appartiene la macchina (*indirizzo di rete*)
- HOSTid = identifica la macchina sulla rete identificata da NETid (*indirizzo host relativo alla rete identificata*)

## Indirizzamento basato su Classi (A, B, C)

- Indirizzo diviso in due parti 
  - **A** 235.126.1.0/8  = i primi 8  bit identificano la rete
    - $2^{24}$ macchine in una sottorete
  - **B** 235.126.1.0/16 = i primi 16 bit identificano la rete
  - **C** 235.126.1.0/24 = i primi 24 bit identificano la rete
- I router devono avere **più di un indirizzo IP**, uno per ogni sottorete a cui sono collegati

Ma le cose sono ancora più complesse:

- Ci possono essere più reti logiche diverse sulla stessa rete fisica
- Si possono dare indirizzi multipli ad una stessa interfaccia 
  - con stesso netid, quindi sulla stessa rete logica
  - con netid diversi, quindi su reti logiche diverse 

Conclusione: Gli host possono avere più indirizzi perché:

- hanno più interfacce, oppure
- hanno più indirizzi sulla stessa interfaccia
---
## CIDR (Classless InterDomain Routing)

- a.b.c.d/x - dove x indica i bit della subnet

---
## Forwarding
L'instradamento è la terza funzione fondamentale di un servizio di comunicazione di livello rete:

1) Gli host inoltrano (**consegna diretta**) ad altri host connessi alla  stessa rete (fisica)
   1) Gli host inviano al router i datagram che non possono  consegnare direttamente (**consegna indiretta**)
2) I router inoltrano datagram verso altri router 
3) Il router finale consegna direttamente il datagram al host destinatario (consegna diretta)

**IP** conosciuto da **Tabella di Inoltro**
**MAC** conosciuto da **ARP**

Per **ARP** si intende un protocollo di rete appartenente alla suite del protocollo internet (IP il cui compito è fornire la "mappatura" tra l'indirizzo IP (32 bit - 4 byte) e l'indirizzo MAC (48 bit - 6 byte) corrispondente di un terminale in una rete locale ethernet.

### Instradamento basato sul prossimo salto

Un array contenente:

- **N** (prefisso $<=32 bit$)
- **R** ($32 bit$) che contiene IP del prossimo router (direttamente raggiungibile) nel cammino verso la destinazione.

VANTAGGI

- Le tabelle di routing hanno una dimensione che dipende dal  numero delle reti e non degli host
-  Crescono solo quando si aggiunge una rete all'internet

SVANTAGGI

-  Solo il router finale può scoprire se la macchina destinazione esiste oppure no (occorre definire metodi di segnalazione degli errori!)
-  Il traffico verso una rete destinazione segue sempre la stessa strada, indipendentemente dalle condizioni della internet
-  Se esistono più cammini verso una destinazione, ne viene usato uno solo. Se vi è un guasto su questo cammino, cade la connettività (anche se ci sarebbero cammini alternativi) a meno che si modifichi la tabella di routing 
-  I cammini fra due reti nelle due direzioni possono essere diversi (i router devono cooperare perché la comunicazione bidirezionale sia sempre possibile).

**Questa impostazione non permette:**

- di instradare in base al **TOS**
- dividere il traffico su più strade egualmente convenienti (**LOAD SHARING**)


**Quando IP riceve un datagramma da una certa interfaccia:** 
  
L'indirizzo destinazione è uguale ad uno degli indirizzi  associati alla interfaccia che lo ha ricevuto? 

- Sì: E' un datagramma intero o un frammento?
    - Frammento -> ri-assembla e alla fine tratta come datagramma  intero
    - Intero -> passa al protocollo superiore indirizzato da PROTOCOL
- No: la macchina è host o router?
    - Host -> errore, scarta il datagramma senza segnalare errori
    - Router -> inoltra tramite tabella di inoltro
  
**DOPO** aver controllato la checksum... 

- Negli host si deve: 
  1. Controllare se il datagramma arrivato è destinato a questa interfaccia 
  2. Se è destinato a questa interfaccia, si consegna al livello superiore giusto (Quale è? Come fa a scoprirlo?)
  3. Se non è destinato a questa interfaccia, **scartare** il datagramma (gli *host* **non devono** inoltrare).
- Nei router si deve:
  1. Controllare se il datagramma arrivato è destinato alla interfaccia su cui è  arrivato
  2. Se è destinato alla interfaccia su cui è arrivato, si consegna al livello superiore giusto che è quello indicato dal campo PROTOCOL del  datagramma (in genere sono pacchetti di test o di gestione del router)
  3. Se non è destinato a questo router, decrementare **time-to-live** (se non positivo, *scartare*), instradare con l'algoritmo di instradamento, calcolare il nuovo *checksum* prima di spedire se necessario.

---

###DHCP (Dynamic Host Configuration Protocol)

1. HOST   -> SERVER : DHCP discover (255.255.255.255 Broadcast Limitato)
2. SERVER -> HOST   : DHCP offer
3. HOST   -> SERVER : DHCP request
4. SERVER -> HOST   : DHCP ACK
---
###NAT (Network Address Translation)
I router abilitati al NAT appaiono al mondo esterno come unici dispositivi con un unico IP. Loro poi si occupano di indirizzare all'host giusto della propria sottorete i pacchetti giusti tramite la **NAT translation table**.
**Problema**: Un client esterno vuole collegarsi ad un server che è in una sottorete di un router con NAT abilitato. L'indirizzo del server (per es. 10.10.10.1) è interno alla sottorete, ma l'unico indirizzo visibile è quello del router (es. 138.76.29.7).

Soluzioni:

1. Configurare **manualmente staticamente** il NAT per inoltrare le connessioni su una porta esterna data (del router) verso il server interno con una data porta.
   - es: 138.76.29.7:80 -> 10.10.10.1:2500 (**SEMPRE**)
2. **UPnP** (**U**niversal **P**lug a**n**d **P**lay) che permette di automatizzare la configurazione (*statica*) di *mapping delle porte* NAT.
3. Fare **relay** (come Skype):
     - Gli host NATtati stabiliscono la connessione con un relay esterno
     - Il client esterno si connette al relay
     - Il relay fa il bridge dei pacchetti tra le due connessioni 
     - CLIENT <-> **RELAY** <-> HOST
  ---

###ICMP (Internet Control Message Protocol)

Viene usato da **host** e **router** per scambiarsi informazioni a livello di rete (tipico uso: notifica degli errori).

**ICMP non**:

- Fornisce dei meccamismi che permettono l’interazione tra un router ed  una sorgente (che ha provocato con il suo comportamento dei malfunzionamenti)
-  Consente di mantenere informazioni di stato (ogni pacchetto viene trattato indipendentemente) 
**Conseguenza** :

  Quando un datagram causa un errore ICMP può solo indicare la  condizione di errore alla sorgente del datagram, la quale deve mettere in relazione l’errore con un singolo programma applicativo oppure eseguire un’altra operazione per risolvere il problema

- ICMP è un meccanismo di segnalazione di errori: non  specifica l'azione da intraprendere a fronte degli errori e non  intraprende nessuna azione
- ICMP riporta l'errore alla sorgente del datagramma, e quindi i router intermedi non ne vengono informati (e la strada a  ritorno potrebbe essere diversa da quella all'andata!). La  sorgente del datagramma a volte non è responsabile dell'errore (ad es. errore di routing) e non è in grado di correggerlo.