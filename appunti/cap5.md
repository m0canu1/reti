### Link-State algorithm

Dijsktra

- tutti i nodi conoscono i costi verso tutti gli altri
- viene calcolato il percorso minore da un nodo agli altri (**tabella di inoltro per quel nodo**)
- iterativo2

### Distance Vector (DV) algorithm

**Bellman-Ford** formula:

$d_x(y) = min_v\{c(x,v) + d_v(y)\}$ dove:

 - $min_v$ riguarda **tutti i vicini** di $x$
 - $c(x,v)$ è il **costo** verso il **vicino** $v$
 - $d_v(y)$ è il **costo** dal **vicino** $v$ verso, la destinazione, $y$

**Idea chiave:**

- di tanto in tanto, ogni nodo manda il proprio ***distance vector* stimato** ai suoi vicini
- alla ricezione di un **DV**, il nodo $x$ aggiorna il proprio **DV** stimato con la formula di Bellman-Ford
  
Ciascun nodo $x$ mantiene i seguenti dati di instradamento:

- per ogni vicino $v$ il costo $c(x,v)$
- il vettore delle distanze del nodo $x$
- i vettori delle distanze di **ciascuno** dei suoi vicini.

---

### Scalable Routing

Divisione in **Autonomous Systems** (**AS**) con algoritmi di:

- *intra-routing* (host della stessa rete AS)
- *inter-routing* (tra AS diversi)

La **scalabilità gerarchica** permette di salvare spazio grazie a tabelle più "snelle"
  
**Intra-AS routing** oppure *Interior gateway protocols* (**IGP**), esempi:

- **RIP** - *Routing Information Protocol*
  - è un algoritmo basato su **DV** (distance vector) basato su broadcast
  - la **metrica** è basata sugli **hops** (massimo 15 hops)
  - i *DV* sono scambiati ogni 30 secondi
- **OSPF** - Open Shortest-Path First
  - è un algoritmo basato su **LS** (Link State)
  - usa Dijkstra
  - si appoggia su IP
  - supporta autenticazione
  - supporta **routing gerarchico** (consente di definire delle aree di routing e come esse siano connesse tra di loro):
    - **area border routers** - sintetizzano le distanze nella propria area, fanno gli advertisement agli altri *area border routers*
    - **backbone(spina dorsale) routers**
    - **boundary routers** - si collegano agli altri *AS*
    - Gli stati dei link vengono distribuiti dettagliatamente all'interno di ciascuna area
    - OSPF distrbuisce informazione sintetica alle destinazioni che non appartengono alla stessa area. Quindi il grado di profondità dell'informazione di routing che viene distribuita dipende da dove mi trovo.
  - supporta percorsi multipli verso la destinazione (**bilanciamento** del carico in caso di percorsi di egual costo)
- **IGRP** - Interior Gateway Routing Protocol

**Inter-AS routing**:

- **BGP** - *Border Gateway Protocol*
  - eBGP: ottiene informazoni sui subnet raggiungibili dall'**e**sterno, cioè dagli *AS* vicini
  - iBGP: propaga le informazioni sulla raggiungibilità all'**i**nterno dell'*AS*
  - è un protocollo **Path Vector** : esso è un **messaggio** (con attributi) che indica che per raggiungere la rete $N$ il router successivo è $R$ e il percorso è $AS1, AS2, AS3$.
  - Funziona con i servizi TCP, con connessioni tra BGP-peers
  - Un **Path Vector** può essere "annunciato" ad un BGP-peer (**eBGP**) può decidere, **in base alle proprie politiche**, di:
    - aggiungere alla propria tabella di routing il path
    - inoltrare il *path vector* ai suoi vicini
  - **BGP Route Selection**
    - Hot-Potato Routing - sceglie il  **gateway** 1(router di bordo) che ha il minor costo *intra-domain*; **non** tiene conto del costo *inter-domain*.
    - Shortest AS-PATH